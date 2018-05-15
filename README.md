# Whack-a-mole

A whack a mole game built solely in vanilla JavaScript for Wes Bos' [JavaScript 30](https://javascript30.com/) course.

[![Screenshot of the Whack-a-mole game](http://res.cloudinary.com/gerhynes/image/upload/v1519758114/whack-a-mole_vyyfua.jpg)](https://gk-hynes.github.io/whack-a-mole/)

## Notes

The goal is to have moles appear in random holes for random amounts of time. The tally of clicked on moles is also logged.

First, select the holes, moles, and scoreboard.

Create a function, `randomTime`, which takes a min and max time, and returns a random time between them.

```js
function randomTime(min, max) {
  return Math.round(Math.random() * (max - min) + min);
}
```

Create another function, `randomHole`, which takes in a list of holes and selects one at random.

`holes` is a NodeList that contains the six holes.

Set a variable, `hole`, to `holes[idx]`.

You will run into a problem in that it could return the same hole twice in a row.

Create a let variable, `lastHole`.

At the end of the function let `lastHole` = `hole`. This will save a reference to the previous hole and allow you to check.

If `hole` equals `lastHole` run the function again.

If `hole` doesn't equal `lastHole`, return `hole`. Otherwise return `randomHole(holes)`.

Create a function, `peep`, which will cause the moles to pop up.

Select a random time (between 200ms and 1s) and a random hole.

Take the random hole and add a class of `up` to it. This will trigger the CSS to animate it in.

But, they will stay up. To remedy this, use `setTimeout` to remove the class after the random time is complete.

```js
function peep() {
  const time = randomTime(200, 1000);
  const hole = randomHole(holes);
  hole.classList.add("up");
  setTimeout(() => {
    hole.classList.remove("up");
    if (!timeUp) peep();
  }, time);
}
```

At this stage `peep()` will run indefinitely.

Create a variable, `timeUp`, and set it to false.

Make a function, `startGame`.

Restart the scoreboard, set `timeUp` equal to false, and call `peep()`.

Use a `setTimeout` to set `timeUp` to false after 10 seconds.

Finally, when you click on a mole, it needs to bonk them on the head.

Make a function, `bonk`, which takes in an event.

Take all of the moles and listen for a click on each of them.

On all events is a property called `isTrusted`. If you fake clicking something, e.g. using JavaScript, this will be false.

If `e` is not `isTrusted` then return.

Crete a let variable, score, equal to 0.

Inside `bonk()` increment `score`, remove the `up` class, and set the text content of `scoreBoard` to `score`.

```js
function bonk(e) {
  if (!e.isTrusted) return; // Catch cheaters faking clicks
  score++;
  this.classList.remove("up");
  scoreBoard.textContent = score;
}
```
