# SOMIPP Lab
## Let's open a restaurant!

### Keywords
`OS`, `pthread`, `producer-consumer`, `readers-writers`, `thread`, `lock`

### Objective
Getting a solid grasp on all of the theory revolving around **threads**, **locks**, and **multiprogramming** in general, as these are some of the most fundamental concepts that the `operating system` designer should understand.

The laboratory will touch upon two of the classic multi-process synchronization problems:
- the [producer-consumer problem](https://en.wikipedia.org/wiki/Producer–consumer_problem)
- the [readers-writers problem](https://en.wikipedia.org/wiki/Readers–writers_problem)

The first "task" of this laboratory is to figure out how each of these problems will fit into *our* own.

## Let's get to the meat of the problem
##### (get it? because it's a problem about a restaurant)
The purpose of this laboratory is for you to write a somewhat realistic simulation of how a **restaurant** works. <br/>
> The general idea is that you have **waiters** (which are a bit counter intuitively named) that take **orders** from the clients, and give these orders to the **cooks** who know how to make them. <br/>

----
### Disclaimer!

For ease of use and how clear is the `json` format, I used it to explain the design of each of the `data structures` you will need in your system.

----

To start, here is the *menu* from which a waiter can generate an order:
- pizza, `id=1`
```json
{
 "preparation-time": 20,
 "complexity": 2,
 "cooking-apparatus": "oven"
}
```
- salad, `id=2`
```json
{
 "preparation-time": 10,
 "complexity": 1,
 "cooking-apparatus": null
}
```
- zeama, `id=3`
```json
{
 "preparation-time": 7,
 "complexity": 1,
 "cooking-apparatus": "stove"
}
```
- Scallop Sashimi with Meyer Lemon Confit, `id=4`
```json
{
 "preparation-time": 32,
 "complexity": 3,
 "cooking-apparatus": null
}
```

An *order* should contain the following information:
- one or more [[1]]() menu items
- the priority of the order (where it ranges from `1` to `5`, `1` being the smallest priority, and `5` -  with the highest one)
- maximum wait time that a client is willing to wait for its order [[2]]() and it should be calculated by taking the item with the highest `preparation-time` from the order and multiply it by `1.3` [we should analyze what the most realistic coefficient should be]. The timer of an order starts from the moment it's created.

An example of an order could look like this:
```json
{
 "items": 1,
 "priority": 2,
 "max-wait": 26
}
```

or

```json
{
 "items": [3, 1, 2, 2],
 "priority": 3,
 "max-wait": 35
}
```

where the `items` indicate the `id`s of the menu items.

The purpose of **waiters** is to generate **orders**; how this happens (either by some random mechanism at random time intervals, either by reading it from a file, or by requiring input from a user) it up to you, this is not the important part. <br/>
The important part is how you will solve the problem of them writing to the **order `list`** and allowing the **cooks** to "take" orders from it as quickly as possible with as little wait time and overhead as possible.

Talking about **cooks**: their job is to take the order and "prepare" the menu item(s) from it, and return the orders as soon and with as little idle time as possible. <br/>
You will have to define the mechanism that will decide which cook takes which order.

Each **cook** has the following characteristics:
- `rank` [[3]](), which defines the complexity of the food that they can prepare (one caveat is that a cook can only take orders which his current rank or one rank lower that his current one):
  - Line Cook (`rank = 1`)
  - Saucier (`rank = 2`) 
  - Executive Chef (Chef de Cuisine) (`rank = 3`)
- `proficiency` [[3]](): it indicates on how may dishes he can work at once. It varies between `1` and `4` (and to follow a bit of logic, the higher the rank of a cook the higher is the probability that he can work on more dishes at the same time).

So a **cook** could be have the following definition:
```json
{
 "rank": 3,
 "proficiency": 3,
 "name": "Gordon Ramsay",
 "catch-phrase": "Hey, panini head, are you listening to me?"
}
```
Get creative on where and when to use this precious information about the cooks.

Another requirement not for the *faint of heart* is to implement the **cooking apparatus** rule [[4]](). It comprises of the fact that a kitchen has limited space, thus a finite number of ovens, stoves and the likes.
As you've noticed some *menu items* require one of these appliance and it's up to you to define what happens when a cook runs into the problem of no available machinery. I give you the green light to experiment and find what abstract data type will best fit to solve the problem.

Some mention should be given to the **reputation system**. It should, theoretically, indicate the success of your implementation.
It is based on the `0` to `5` :star: system, `0` being the worst rating, and `5` - the highest. How you get the assigned the stars, you might ask? Well, since we can't really give it a taste test, what will define the number of stars given to each order would be the time-frame it took to complete.

| Time frame       | :star: |
|------------------|--------|
| `< max-wait`     | 5      |
| `max-wait*1.1`   | 4      |
| `max-wait*1.2`   | 3      |
| `max-wait*1.3`   | 2      |
| `max-wait*1.4`   | 1      |
| `> max-wait*1.4` | 0      |