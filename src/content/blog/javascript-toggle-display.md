---
title: "The Correct way to Toggle Display with JavaScript"
pubDate: "2022-06-23"
description: "How to toggle display with JavaScript at any given event; bugs in the traditional show/hide method, fixes, and advanced techniques."
tagline: "Show/Hide things on Click with JavaScript"
---

Toggling an element’s display with JavaScript is not a big deal if you know the correct way to do it.

This toggling, in general, depends on two things; a **target** and a **trigger**. The display state for the target toggles every time an event gets performed on the trigger element; usually, it’s a click event.

In this tutorial, you’ll learn how to toggle display with JavaScript the right way. But before that, let’s look at some quick and not-so-elegant solutions for the same.

![JavaScript Toggle Display](http://w3bits.com/wp-content/uploads/javascript-display-toggle.jpg)

A photo by [Marijn Vandevoorde](https://unsplash.com/photos/8gK9siTwqN8)

## Quick n’ dirty JavaScript to show/hide stuff

Let’s construct a simple method to receive an element as an argument and manipulate its display in a toggling manner.

    const toggleDisplay = target => target.style.display =
      (target.style.display == 'none') ?
        'block' :
        'none'

Above is the traditional way to show or hide any given HTML element with plain JavaScript. It looks for the display CSS property in the DOM and updates it according to the basic toggling logic whenever the trigger gets clicked.

In simpler terms, it takes a reference to an element and then toggles its display between “block” and “none”. Let’s attach it to a button’s click.

    <div id="target">...</div>
    <button
      onclick="toggleDisplay(document.querySelector('#target'))">
        Toggle display
    </button>

Above is the basic HTML required to implement the `toggleDisplay` method.

The code is pretty simple and may look correct at first, but it has a problem that we’ll address in the next section. Here’s [a demo you should see](http://w3bits.com/demo/simple-javascript-display-toggle/) before we move any further.

### The problem

This problem isn’t quite visible yet. Suppose we use this function with an element with a grid display property. I have a demo prepared for you to [notice this issue](http://w3bits.com/demo/simple-javascript-toggle-display-glitch/‎).

As you can see, it messed up the whole grid arrangement. The next segments cover some quick fixes to its limitation to only one type of display.

## The fix

It’s time to make our existing toggle method a bit more intelligent. Let’s add two more parameters to reference the trigger and the default display property.

    const
      toggleDisplay = (trigger, target, defaultDisplay = 'block') => {
      ...
    }

As discussed above, adding the default display as inline CSS fixes the first problem. One more way to do that is by adding the default display with the [DOM style object](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style).

Let’s also shift the event listener to our method so that it could use the trigger to implement the toggle logic. I’m demonstrating it with a `click` event, you can add a different event like `touchstart`, etc.

    const toggleDisplay = (trigger, target, defaultDisplay = 'block') => {
      target.style.display = defaultDisplay
      trigger.addEventListener('click', e => {
        target.style.display =
          (target.style.display == 'none') ?
            defaultDisplay :
            'none'
      })
    }

See it [in action here](http://w3bits.com/demo/better-javascript-display-toggle/). The only problem with this one is that you have to provide the default display manually.

## Advanced JavaScript Display Toggling

Our previous solution depends on the inline display style of the element to perform display toggling.

An easy way to create a better display toggle solution is by defining hidden and visible classes in CSS and then toggling them using the classList APIs toggle function.

But then it would have one more dependency to function correctly: the CSS classes. We certainly can do better than that.

The best approach, in this case, would be to remove the above-discussed dependencies. Let’s grab the default display from the author (or user-agent, if author CSS definitions aren’t available) stylesheets directly using the [getComputedStyle Web API](https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle).

    let defaultDisplay =
        window.getComputedStyle(target).getPropertyValue('display')

The rest of the logic will remain unchanged. Here’s the outcome with some changes based on the above plan.

    const toggleDisplay = (target, trigger) => {
      let defaultDisplay =
        window.getComputedStyle(target).getPropertyValue('display')
      trigger.addEventListener('click', e => {
        target.style.display =
          (target.style.display == 'none') ?
          defaultDisplay:
          'none'
      })
    }

[See the Live Demo](http://w3bits.com/labs/javascript-toggle-display/)

### Further Improvements

We can add a DOM check for the target and the trigger elements. This will keep us from getting errors if one of these elements is not available in the DOM.

    const toggleDisplay = (target, trigger) => {
      if(!target || !trigger) return
      let defaultDisplay =
        window.getComputedStyle(target).getPropertyValue('display')
      trigger.addEventListener('click', e => {
        target.style.display =
          (target.style.display == 'none') ?
          defaultDisplay:
          'none'
      })
    }

The above code will not produce any error even if you provide non-existent references to the `toggleDisplay` function. Alternatively, you can add a warning statement like this to assist the user:

    console.warn(`toggleDisplay: Make sure the target and trigger elements exist in the DOM.`)

## Conclusion

We learned about the traditional way to toggle the display of a given element in JavaScript. We also noticed the problems with it and how easy it is to fix them.

Then we created an advanced method to show/hide elements on click event. This can be developed further and infused with the [pure JavaScript SlideToggle](http://w3bits.com/javascript-slidetoggle/) for animated toggling.
