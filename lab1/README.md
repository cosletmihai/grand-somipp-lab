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
### Disclamer!

For ease of use and how clear is the `json` format, I used it to explain the design of each of the `data structures` you will need in your system.
----

To start, here is the *menu* from which a waiter can generate an order:
- pizza, `id=1`
```json
{
 'preparation-time': 20,
 'complexity': 2,
 'cooking-apparatus': 'oven'
}
```
- salad, `id=2`
```json
{
 'preparation-time': 10,
 'complexity': 1,
 'cooking-apparatus': null
}
```
- zeama, `id=3`
```json
{
 'preparation-time': 7,
 'complexity': 1,
 'cooking-apparatus': 'stove'
}
```
- Scallop Sashimi with Meyer Lemon Confit, `id=4`
```json
{
 'preparation-time': 32,
 'complexity': 3,
 'cooking-apparatus': null
}
```

An *order* should contain the following information:
- one or more [[1]]() menu items
- the priority of the order (where it ranges from `1` to `5`, `1` being the smallest priority, and `5` -  with the highest one)
- maximum wait time that a client is willing to wait for its order [[2]]()
- **rating** (or *reputation points*) that your restaurant can either win or lose depending on the time it takes to complete the order [[2]]()

An example of an order could look like this:
```json
{
 'items': 1,
 'priority': 2,
 'max-wait`: 40,
 'reputation`: 0.5
}
```

or

```json
{
 'items': [3, 1, 2, 2],
 'priority': 3,
 'max-wait`: 35,
 'reputation`: 2.1
}
```

where the `items` indicate the `id`s of the menu items