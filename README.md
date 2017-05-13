# Game Points

This is my solution to a front-end developer test for Kahoot!

## Build setup

You will need a relatively up-to-date version of [Node](https://nodejs.org/en/) (I used v6.9.4).

``` bash
# install dependencies
npm install

# serve at localhost:8080
npm run dev
```

You can also preview the app here: http://staceyfenton.com/gamepoints/

## The brief

Implement the code for a simple browser based game points system that calculates the total points awarded to a player for a number of items they have collected in a game. 

We’ll use individual letters of the alphabet to identify the items (A, B, C, and so on). Our items are scored individually. Some items are worth more if collected in multiples: collect n of them, and you’ll get y points. For example, item ‘A’ might be worth 50 points individually, but this week we have a special bonus: collect three ‘A’s and they’ll be worth 200 points instead of 150. In fact this weeks rewards are:

| Item   | Unit Points   | Bonus     |
| ------ |:-------------:| --------- |
| A      | 50            | 200 for 3 |
| B      | 30            | 90 for 2  |
| C      | 20            |           |
| D      | 15            |           |

Our points system accepts items in any order, so that if we collect a B, an A, and another B, we’ll recognise the two B’s and score them at 90 (for a total score so far of 140).