# Summary

Design a trading platform Web UI, including order list and deal list.

A order has such fields: number(id), bid/ask, quantity, price, like this:

* (1001, ask, 6, 480)
* (1002, bid, 8, 470)
* (1003, bid, 5, 460)

When there is a bid order whose price is higher than another ask order's price, these two orders can be make a deal. The price is the average number of the two orders. When a order is made deal with multiple orderes, the higher price will get higher priority; If two orders have same price, the order with lower number/id have higher priority.

For example, a ask order with price 480 is higher than a bid order with price 470, so there two orders can not make a deal. Then a new order (1004, ask, 10, 460) comes, it will make deal with order 1002, at price 465, the volume is min(10, 8) = 8; then with order 1003, price 460, volume is min(10 - 8, 5) = 2. So the order book becomes liek this:

* (1001, ask, 6, 480)
* (1003, bid, 3, 460)

## Backend API

The backend service generate a order per 1 or 2 seconds, the order number begins from 0; price is integer.

The http API is exposed on TCP 9888:

1. `GET /listOrders?start=<start order number>&size=<how many orders to fetch>`

Exmaple: `http://0.0.0.0:9888/listOrders?start=10&size=100`

Fetch 100 orders from number 10. If the number of last order, mark as n, in the memory in less than 110, it will return order 10 to n; if n < 10, will return empty results.

Exmaple:

```
[{"number":10,"side":"ask","quantity":13,"price":160},{"number":11,"side":"ask","quantity":10,"price":87}]
```

2. `GET /reset`

Reset backend; clear up all the orders. For test.

## Frontend UI

* UI has two queue:

  * ask order queue: show 20 ask orders, order by price desc
  * bid order queue: show 20 bid orders, order by price asc
  * deal queue: show latest 30 deal, order by created time desc. click an item in the queue, should show the deal info, including deal time, price, ask order, bid order, etc.
  * Use any js & css framework you like. Bonus: when there's new deal order, the elements in the queue move downward with animation.

In common, the deal is processed in backend, we move to frontend just for exercise.

The UI is very like the orders & trades show on the bottom of https://www.okcoin.com/market-btc.html.

## Download code & submit

1.	Fork https://github.com/libreoscar/ethmarket_interview
2.	set golang environment, version 1.6 or up
3.	go get github.com/libreoscar/ethmarket_interview
4.	cd ethmarket_interview && go run main.go
5.	visit http://0.0.0.0:9888/ you will see an example. the JS files is under static direcotory;
    1.	there are 100 orders showed in the example, just for example and nothing to do with our requiremnets.
    2.	it use Vue.js, you can choose what you like.

When you finished, please check in the code to your github.