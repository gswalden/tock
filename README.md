# Tock #

[![Bower version](https://badge.fury.io/bo/tock.svg)](http://badge.fury.io/bo/tock)

A javscript timer/countdown clock. 

[View Demo](http://deviouschimp.co.uk/misc/tock)

Based on an idea by James Edwards:
http://sitepoint.com/creating-accurate-timers-in-javascript/


# Readme Contents #

 * [What's so good about it?](#whats-so-good-about-it)
 * [When would I use it?](#when-would-i-use-it)
 * [How do I get it?](#how-do-i-get-it)
 * [How do I use it?](#how-do-i-use-it)
 * [Options](#options)
 * [Callbacks](#callbacks)
 * [Methods](#methods)
 * [License](#license)


# What's so good about it? #

* Pure Javascript - no dependencies
* Self-correcting time based on the system clock - won't go out of time unlike clocks based solely on setInterval or setTimeout (see the link above).
* It can be used to count up from any arbitrary time (or 0:00) or countdown from a given time. 
* It can call a callback function every tick (10 milliseconds) and (for countdown clocks) when the clock reaches 0:00.
* It's about as accurate a clock as you can get with Javascript.


# When would I use it? #

 * Countdown counters, e.g. "site will launch in..."
 * Timers
 * Accurate timing of any repeated action


# How do I get it? #

You probably use a dependency manager, such as Bower:

    bower install tock

...or maybe NPM:

    npm install tocktimer

Or you can just download a zip file using the "download zip" button on the right.


# How do I use it? #

Tock.js works behind the scenes - it doesn't alter anything on screen - so here I'll show how to make a stop-watch that updates in real-time on screen.

### 1) Make some html to show the clock. ###

      <button id="start">Start</button> 
      <button id="stop">Stop</button> 
      <input id="clock" value="10:00">
      <script> // javascripts... </script>

### 2) Instantiate a Tock ###

Now we write some Javascript. First we'll create a new instance of Tock and assign it to a variable called *timer*.

    var timer = new Tock();

This will give you a clock that will count up from 00:00 when the start() method is called. The stop() and reset() methods can also be used.

For more control we can pass in some options. *Note that all options are... optional.*

    var timer = new Tock({
      countdown: true,
      interval: 10,
      callback: someCallbackFunction,
      complete: someCompleteFunction
    });

[See available options below](#options)

### 2) Add some controls ###

You'll need some way of controlling your clock. Let's set up some buttons *(using jQuery for example)*.

    $('#start').on('click', function() {
	    timer.start($('#clock').val());
	});

Note that we get the time from the clock input and pass it to the start function as the start time.

    $('#stop').on('click', function() {
	    timer.stop();
	});

If you're not using a countdown clock you can make a reset button, too.

    $('#reset').on('click', function() {
	    timer.reset();
	});

You could also create a reset button if you *are* using a countdown clock, but that's beyond the scope of this walkthrough. The tools are there. Do with them what you can. After this next section you're on your own. Good luck. We're all counting on you.


# Options #

  * **countdown** *boolean*  Default: false. If true, the clock will count down from a given time, otherwise it will count up from 0:00.
  * **interval** *integer* Default: 10. How often, in milliseconds, that the clock will tick.
  * **callback** *function* Default: null (see below)
  * **complete** *function* Default: null (see below)


## Callbacks ##

The callback option is a function that will be called once every `interval` milliseconds.

Here we'll use the `lap()` method to get the current clock time (in milliseconds). We'll then pass that through `msToTime()` to format it nicely before displaying it in the `input` field.

    callback: function () {
        var current_time = timer.msToTime(timer.lap());
        $('#clock').val(current_time);
    }

As we are have set `countdown` to `true` we can also pass in a function to call once the countdown reaches zero.

    complete: function () {
        alert("Time's up!");
    }


# Methods #

 * start(time)
   * *time* (optional) can be either a countdown value or a starting value.
     * If a countdown timer then set *time* to count down from.
     * If a starting value then set *time* to the desired start time to count up from.
     * For both timer types the following time formats are possible:
        * "MM:SS"
        * "MM:SS:ms"
        * "MM:SS.ms"
        * "HH:MM:SS"
        * "yyyy-mm-dd HH:MM:SS.ms"
        * milliseconds
 * stop()
   * Stops the clock and clears the timeout() from memory.
 * pause()
   * Stop the clock if it's running, continue clock if paused. Keeps the timeout() in memory.
 * reset()
   * Restart times to zero. Countdown clocks still need a duration to be passed to `start()` after `reset()` is called.
 * lap()
   * Returns elapsed time in milliseconds.
 * msToTime(ms)
   * Note: this is rudimentary - won't handle > 1 hour
 * timeToMS(time)
   Time should be a string of form:
   * "MM:SS"
   * "MM:SS:ms"
   * "MM:SS.ms"
   * "HH:MM:SS"
   * "yyyy-mm-dd HH:MM:SS.ms"
   * If the input cannot be recognized as one of the above then 0 is returned
 * msToTimecode(time)
   * returns an HH:MM:SS form from milliseconds


# Development #

I'm using [Grunt](http://gruntjs.com/) for task running and [Mocha](http://mochajs.org/) for testing. 

Get all dependencies with:

    npm install

Run all tasks:

    grunt

Run all tasks automatically when you save.

    grunt watch


# License #

MIT License.
