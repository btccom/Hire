# Summary

Build a trading platform Web UI, including order list and match list.

A order has the following fields: id:int, type:string(buy or sell), quantity:int, price:int, eg:

* (1001, sell, 6, 480)
* (1002, buy, 8, 470)
* (1003, buy, 5, 460)

When there is a buy order whose price is higher than another sell order's price, these two orders can be matched.  
The price is the average between the buy and the sell order.  
When a match can be made with multiple orderes, the higher price will get first priority; If two orders have same price, the oldest order (with the lowest id) will have higher priority.

For example, an sell order with price 480 is higher than a buy order with price 470, so there two orders can not make a match.  
Then a new order (1004, sell, 10, 460) comes, it will make match with order 1002, at price 465, the volume is min(10, 8) = 8; then with order 1003, price 460, volume is min(10 - 8, 5) = 2.  
So the order book becomes like this:

* (1001, sell, 6, 480)
* (1003, buy, 3, 460)


**note:** terminology sometimes changes; sell orders can also be called 'ask' and buy orders can also be called 'bid'.
we've chosen to stick to 'sell' and 'buy' here because it's easier to grasp.


## Backend API

There's a backend service for this assignment that generates random orders every 1 or 2 seconds, the order id begins from 0; price is integer.

The http API is exposed on TCP 9888:

1. `GET /listOrders?start=<start order number>&size=<how many orders to fetch>`

For example; `http://localhost:9888/listOrders?start=10&size=100` will fetch 100 orders from number 10. 
If the number of last order, mark as n, in the memory in less than 110, it will return order 10 to n; if n < 10, will return empty results.

Example Output:

```
[{"id":10,"side":"sell","quantity":13,"price":160},{"id":11,"side":"sell","quantity":10,"price":87}]
```

2. `GET /reset`

You can call this to reset the backend and clear up all the orders (for testing purposes).

## Frontend UI

* UI has two queues:
  * sell order queue: show 20 sell orders, order by price desc
  * buy order queue: show 20 buy orders, order by price asc
  * match queue: show latest 30 match, order by created time desc. 
    click an item in the queue, should show the match info, including match time, price, sell order, buy order, etc.
  * Use any js & css framework you like.
  * Bonus: apply an animation to the queue's when new orders & matches appear.

IN the real world the matching of matches would ofcourse be done in a backend system,, but for this exercise the matching of the orders should be done in the frontend codebase.

An example of a real world system like this can be seen here:
 * https://www.okcoin.com/market#product=btc_usd (at the bottom you can see Orders and Trades)

## Download backend code
1. You can checkout the repo with the backend code from here: https://github.com/btccom/fe-exercise-backend  
2. `npm install` -> `node app.js`
3. `http://localhost:5001/listOrders`

## What is expected of you

Build your own frontend in a seperate (clean) repo and just connect to the backend API (so **don't** build into the example project).  
When you finished, please check in the code to your github and send us the URL of the repo.

You can (and should) use a frontend framework of your choosing.

This exercise does **NOT** require you to write (or even read) any of the backend code, 
it's purely focused on consuming the data from the backend API and processing + displaying it!

## Extra

As an example case of orders being matched you can check this gist; https://gist.github.com/rubensayshi/65989f3f4e19036e16aab12216fbc9bd  
It describes a couple of incoming orders from the backend API and the state changes that happen when processing them.
