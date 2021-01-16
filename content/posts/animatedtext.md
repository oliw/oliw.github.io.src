+++
title = "Animating Text"
date = "2021-01-16T00:28:56-08:00"
categories = ["Coding"]
summary = "How to create a typing animation effect in Javascript"
+++

<span id="ref1"></span>

<script>
const TYPING_SPEED_MS_PER_CHAR = 45;

function animateText(text, elementId) {  
    const target = document.getElementById(elementId)
    const startTimeMs = performance.now();

    // The browser will call this method just before it repaints the screen
    function onScreenAboutToRedraw(currentTimeMs) {
      const timeElapsedMs = currentTimeMs - startTimeMs;

      const numCharactersToShow = Math.floor(timeElapsedMs/TYPING_SPEED_MS_PER_CHAR);

      const charactersToShow = text.substr(0, numCharactersToShow);
      target.innerText = charactersToShow;

      if (numCharactersToShow >= text.length) {
        // All the characters are revealed. 
        // No need to continue animating.
        return 
      } else {
        // Ask the browser to call onAnimationFrame the next time its about to redraw the screen
        requestAnimationFrame(onScreenAboutToRedraw);
      }
    }

    // Ask the browser to call onAnimationFrame the next time its about to redraw the screen
    requestAnimationFrame(onScreenAboutToRedraw);
  }
  
  animateText(
    "In this guide, I'll show you how to create easy typing animations using Javascript using less than 25 lines of code.",
    "ref1"
  );
</script>

<!-- Insert snippet of Animated Text-->

We start by defining an empty `<span/>` in the body of your html document. This is where we'll make our typing text appear.

```
<span id="animatedText"></span>
```

Next we define a function `animateText()` that takes two arguments, the text we'd like to see typed out, and the ID of the `span` element we created earlier. For now, the function will do nothing.

```
function animateText(text, elementId) {

}
```

We'll need to get a handle on the `span` element. People used to rely on JQuery for this sort of thing, but now you can use one of the standard methods available in the browser `document.getElementById`.

```
function animateText(text, elementId) {
    const target = document.getElementById(elementId)
}
```

Now that we have a handle on our `span` element, we are able to programatically fill the span with text of our choosing like this

```
target.innerText = "Hello!"
```

This will come in handy later.

Nows its time to set up our animation itself.

Animating with javascript is typically done by making recursive calls to a built-in method called `requestAnimationFrame `

When you call `requestAnimationFrame`, you provide it with a callback function of your choice. By calling `requestAnimationFrame(callbackFn)` you're saying

> "Hey browser, the next time you're about to redraw whats on the screen, please call this callback function first".

Typically browsers redraw their screens pretty often, around 60 times a second.

We'll call our callback function `onScreenAboutToRedraw()`. To begin with, we'll make it empty i.e. do nothing.

```
function animateText(text, elementId) {
    const target = document.getElementById(elementId)

    function onScreenAboutToRedraw() {

    }
    onRequestAnimation(onScreenAboutToRedraw)
}
```

When `onScreenAboutToRedraw` is called, we want to calculate how much time has elapsed since the animation began; and thus how many characters of the text should be on the screen already.

To keep track of how much time has elapsed, we'll need two pieces of information

- a timestamp representing the start of our animation,
- a timestamp representing when our callback function was run.

A timestamp representing the start of the animation can be obtained by calling the built-in function `performance.now()` at the beginning of our `animateText()`.

```
function animateText(text, elementId) {
    const target = document.getElementById(elementId)
    const startTimeMs = performance.now();

    function onScreenAboutToRedraw() {

    }
    onRequestAnimation(onScreenAboutToRedraw)
}
```

To get a timestamp representing the time that the callback function ran is easy because according to the specification of `onRequestAnimation`, the browser actually calls callbackFn with the current time in milliseconds as a first arguement.

```
function animateText(text, elementId) {
    const target = document.getElementById(elementId)
    const startTimeMs = performance.now();

    function onScreenAboutToRedraw(currentTimeMs) {
        const timeElapsedMs = currentTimeMs - startTimeMs;
    }
    onRequestAnimation(onScreenAboutToRedraw)
}
```

Now we know how much time has elapsed, we need to calculate how many characters from our text we want to display.

We'll do that by first deciding how long it should take to type each character. We'll store that as a constant at the top of our javascript.

```
const TYPING_SPEED_MS_PER_CHAR = 45;
```

We then make the following calculation in our `onScreenAboutToRedraw` callback function

```
const numCharactersToShow = timeElapsedMs/TYPING_SPEED_MS_PER_CHAR
```

This will actually return an imperfect number like `3.234234` so we'll round it down using `Math.floor` to get

```
const numCharactersToShow = Math.floor(timeElapsedMs/TYPING_SPEED_MS_PER_CHAR)
```

Next, we'll use the built-in string method `substr` to pluck out the first `numCharactersToShow` from the original text.

```
const charactersToShow = text.substr(0, numCharactersToShow);
```

Finally we'll update the contents of the span with that `innerText` we saw earlier.

```
target.innerText = charactersToShow;
```

Last but not least, we need to decide if we should continue the animation or not.

We should keep animating if we haven't drawn all the characters on the screen yet.

To keep animating, we request another animation frame.

To stop animating, we do nothing further and return from our callbackFunction instead.

```
    if (numCharactersToShow >= text.length) {
        // All the characters are revealed.
        // No need to continue animating.
        return
    } else {
        // Ask the browser to call onAnimationFrame the next time its about to redraw the screen
        requestAnimationFrame(onScreenAboutToRedraw);
    }
```

## Putting it all together

```
const TYPING_SPEED_MS_PER_CHAR = 45;

function animateText(text, elementId) {
    const target = document.getElementById(elementId)
    const startTimeMs = performance.now();

    // The browser will call this method just before it repaints the screen
    function onScreenAboutToRedraw(currentTimeMs) {
      const timeElapsedMs = currentTimeMs - startTimeMs;

      const numCharactersToShow = Math.floor(timeElapsedMs/TYPING_SPEED_MS_PER_CHAR);

      const charactersToShow = text.substr(0, numCharactersToShow);
      target.innerText = charactersToShow;

      if (numCharactersToShow >= text.length) {
        // All the characters are revealed.
        // No need to continue animating.
        return
      } else {
        // Ask the browser to call onAnimationFrame the next time its about to redraw the screen
        requestAnimationFrame(onScreenAboutToRedraw);
      }
    }

    // Ask the browser to call onAnimationFrame the next time its about to redraw the screen
    requestAnimationFrame(onScreenAboutToRedraw);
  }

  animateText(
    "Lets try this out! Wahoo it seems to work",
    "part1"
  );
```

## Going Further

What we've built today has a simple amount of functionality. In the next tutorial, I'll show you how we can evolve what we've built and turn it into something a lot more advanced.

<span id="ref2"></span>

<script>
    function randomNumber(min, max) {
  return Math.random() * (max - min) + min;
}

   class AdvantedTextAnimator {
  constructor(elementId, options = {}) {
    this.elementId = elementId;
    this.target = document.getElementById(elementId);
    this.options = options;
    this.promiseQueue = [];
    this.targetText = "";
  }

  async play() {
    while (this.promiseQueue.length > 0) {
      const nextAnimation = this.promiseQueue.shift();
      await nextAnimation();
    }
  }

  // Animations
  wait(ms) {
    const callBack = () => {
      return new Promise((resolve) => {
        setTimeout(resolve, ms);
      });
    };
    this.promiseQueue.push(callBack);
  }

  delete(text) {
    const callBack = () => {
      const DEFAULT_MS_PER_CHAR = 60;
      const target = this.target;
      const oldText = this.targetText;
      if (oldText.endsWith(text)) {
        this.targetText = oldText.substr(0, oldText.length - text.length);
      }

      let charTimes = Array(text.length).fill(DEFAULT_MS_PER_CHAR);

      if (this.options.humanize) {
        // Humanize
        charTimes = charTimes.map((time) => {
          return time + randomNumber(0, 10);
        });
      }

      const startTime = performance.now();

      // Input: total milliseconds elapsed
      // Output: the number of letters to show
      function timing(msElapsed) {
        let amountRemaining = msElapsed;
        let count = 0;
        while (amountRemaining > 0) {
          amountRemaining =
            amountRemaining - charTimes[charTimes.length - 1 - count];
          count = count + 1;
        }
        return count;
      }

      // Input: the number of chars to draw
      const draw = (progress) => {
        const substr = oldText.substr(0, oldText.length - progress);
        target.innerText = substr;
      };

      return new Promise(function (resolve, reject) {
        function onAnimationFrame(time) {
          // calculate the ms elapsed
          const elapsed = time - startTime;

          // calculate the current animation state
          let progress = timing(elapsed);
          draw(progress); // draw it

          if (progress < text.length) {
            requestAnimationFrame(onAnimationFrame);
          } else {
            resolve();
          }
        }

        requestAnimationFrame(onAnimationFrame);
      });
    };
    this.promiseQueue.push(callBack);
  }

  write(text) {
    const callBack = () => {
      const DEFAULT_MS_PER_CHAR = 60;
      const target = this.target;
      const startingText = this.targetText;
      this.targetText = this.targetText + text;
      let charTimes = Array(text.length).fill(DEFAULT_MS_PER_CHAR);

      if (this.options.humanize) {
        // Humanize
        charTimes = charTimes.map((time) => {
          return time + randomNumber(0, 10);
        });
      }

      const startTime = performance.now();

      // Input: total milliseconds elapsed
      // Output: the number of letters to show
      function timing(msElapsed) {
        let amountRemaining = msElapsed;
        let count = 0;
        while (amountRemaining > 0) {
          amountRemaining = amountRemaining - charTimes[count];
          count = count + 1;
        }
        return count;
      }

      // Input: the number of chars to draw
      const draw = (progress) => {
        const substr = text.substr(0, progress);
        target.innerText = startingText + substr;
      };

      return new Promise(function (resolve, reject) {
        function onAnimationFrame(time) {
          // calculate the ms elapsed
          const elapsed = time - startTime;

          // calculate the current animation state
          let progress = timing(elapsed);
          draw(progress); // draw it

          if (progress < text.length) {
            requestAnimationFrame(onAnimationFrame);
          } else {
            resolve();
          }
        }

        requestAnimationFrame(onAnimationFrame);
      });
    };
    this.promiseQueue.push(callBack);
  }
}

function isScrolledIntoView(el) {
    var rect = el.getBoundingClientRect();
    var elemTop = rect.top;
    var elemBottom = rect.bottom;

    // Only completely visible elements return true:
    var isVisible = (elemTop >= 0) && (elemBottom <= window.innerHeight);
    // Partially visible elements return true:
    //isVisible = elemTop < window.innerHeight && elemBottom >= 0;
    return isVisible;
}

const advancedAnimator = new AdvantedTextAnimator("ref2", { humanize: true });
advancedAnimator.write("We can take this a lot further");
advancedAnimator.wait(2000);
advancedAnimator.delete("We can take this a lot further");
advancedAnimator.wait(500);
advancedAnimator.write("For");
advancedAnimator.wait(500);
advancedAnimator.write(" instance");
advancedAnimator.wait(500);
advancedAnimator.delete("For instance");
advancedAnimator.wait(1000);
advancedAnimator.write("We can make it so we can fix our misfakes")
advancedAnimator.wait(600)
advancedAnimator.delete("misfakes")
advancedAnimator.write("mistakes!")

const elem = document.getElementById('ref2');

const scrollHandler = () => {
  if (isScrolledIntoView(elem)) {
    console.log("Here!")
    advancedAnimator.play();
    unbind();
  }
}

const unbind = () => document.removeEventListener('scroll', scrollHandler);
document.addEventListener('scroll', scrollHandler);

</script>
