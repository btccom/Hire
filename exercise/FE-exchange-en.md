# Summary

Design a trading platform Web UI, including order list and match list.

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
[{"number":10,"side":"sell","quantity":13,"price":160},{"number":11,"side":"sell","quantity":10,"price":87}]
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
 * https://www.okcoin.com/market-btc.html (at the bottom you can see Orders and Trades)

## Download code & submit

1. install golang locally (v1.6 or higher)
  * on ubuntu just do the following:
    * `sudo apt-get install golang-go`
    * then you need to pick a "root dir" for your golang projects so set `export GOPATH="~/golang"` or some other path where you prefer storing your go things.  
      you can put that line in your `~/.bashrc` so you don't have to keep doing it when you open new terminals.  
      also make sure that that directory exists ;)
2. run `go get github.com/libreoscar/ethmarket_interview`, this will install the repo in your golang `src` folder
3. go into the place where it was installed  
   `cd $GOPATH/src/github.com/libreoscar/ethmarket_interview`
4. run `go run main.go` to start the backend
5. you can now call the backend API
6. you can also visit `http://localhost:9888/` for a tiny example project of using the backup API  
   it just fetches the first 100 orders and lists them.  
   if you want to see the source of this stream you can checkout the `static` directory (it's Vue.js code)

build your own frontend in a seperate (clean) repo and just connect to the backend API (so **don't** build into the example project).  
When you finished, please check in the code to your github and send us the URL of the repo.
