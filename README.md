# Tweezer.js

[![Tweezer.js on NPM](https://img.shields.io/npm/v/tweezer.js.svg)](https://www.npmjs.com/package/tweezer.js)

A small, dependency-free, ES6 library for smooth animations. [Demo](http://jaxgeller.com/tweezer.js/).

Tweezer.js is the last tweening library you'll ever need. It provides the building blocks for any animation, allowing you to construct beautiful animations simply and without the need of requiring lots of other dependencies like smoothScroll, countUp.js, and GSAP.

+ [Use](https://github.com/jaxgeller/tweezer.js#use)
+ [Examples](https://github.com/jaxgeller/tweezer.js#examples)
+ [Configuration](https://github.com/jaxgeller/tweezer.js#configuration)
+ [Browser Support](https://github.com/jaxgeller/tweezer.js#browser-support)
+ [What is tweening?](https://github.com/jaxgeller/tweezer.js#what-is-tweening)

## Use

Tweezer was developed with a modern JavaScript workflow in mind. To use it, it's recommended you have a build system in place that can transpile ES6, and bundle modules. For a minimal boilerplate that fulfills those requirements, check out [outset](https://github.com/callmecavs/outset) or the [gh-pages branch](https://github.com/jaxgeller/tweezer.js/tree/gh-pages) of this repo.

### Install and Import

In the commandline
```bash
$ npm install tweezer.js --save
```

### Instantiate and Configure

Two parameters are required to start Tweezer, a `start` and an `end` value, the rest is optional. Tweezer works by emitting values via an event emitter. **It is up to you on how to use these values.**

Below are all of the configuration options. Note: **all methods can be chained.**

```es6
import Tweezer from 'tweezer.js'

new Tweezer({
    start: 0,
    end: 0,
    duration: 1000,
    easing: (t, b, c, d) => {
      if ((t/=d/2) < 1) return c/2*t*t + b
      return -c/2 * ((--t)*(t-2) - 1) + b
    }
})
.on('tick', value => {
  // do something with value
})
.on('done', ()=> {
  // all done
})
.begin()
```

## Examples and Use Cases

##### Smooth Scroll to an Element

```es6
let button = document.querySelector('#jump-button')

button.onclick = ()=> {
  new Tweezer({
    start: window.scrollY,
    end: document.body.clientHeight - window.innerHeight
  })
  .on('tick', v => window.scrollTo(0, v))
  .begin()
}
```

##### Add a tweened count up button

```es6
let countUpText = document.querySelector('#count-up')
let countUpButton = document.querySelector('#count-up-button')

countUpButton.onclick = ()=> {
  new Tweezer({
    start: 0,
    end: 123456
  })
  .on('tick', v=> countUpText.textContent = v)
}
```

##### Move an element across the screen

```es6
let mover = document.querySelector('#move-across-screen')
let moverButton = document.querySelector('#move-across-screen-button')

moverButton.onclick = ()=> {
  new Tweezer({
    start: mover.getBoundingClientRect().left,
    end: window.innerWidth - mover.getBoundingClientRect().width
  })
  .on('tick', v=> {
    mover.style.transform = `translate3d(${v}px, 0, 0)`
  })
}
```

## Configuration

Two parameters are required to start Tweezer, a `start` and an `end` value. Tweezer works by emitting values via an event emitter. **It is up to you on how to use these values.**

Below are all of the configuration options. Note: **all methods can be chained.**

#### Options
Defaults are shown below, explanation of each option follows.

```es6
new Tweezer({
    start: 0,
    end: 0,
    duration: 1000,
    easing: (t, b, c, d) => {
      if ((t/=d/2) < 1) return c/2*t*t + b
      return -c/2 * ((--t)*(t-2) - 1) + b
    }
})
.on('tick', value => {
  // do something with value
})
.on('done', ()=> {
  // all done
})
.begin()
```

For a list of easings, checkout [ez.js](https://github.com/jaxgeller/ez.js). Just make sure to implement the four parameters `t, b, c, d`.

#### Using the emitter

To handle events, use the `.on(handlerName, callback)` method.

##### Tick Event

This is where you should handle the values returned by tweezer.

```es6
new Tweezer({start: 0, end: 9000})
  .on('tick', v => el.style.transform = `transform3d(v, 0, 0)`)
```

##### End Event

This event fires when tweening is completed.

```es6
new Tweezer({start: 0, end: 9000})
  .on('done', v => alert('All Done!'))
```

## Browser Support

Tweezer.js depends on the following browser APIs:

* [requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)

Consequently, it supports the following natively:

* Chrome 24+
* Firefox 23+
* Safari 6.1+
* Opera 15+
* IE 10+
* iOS Safari 7.1+
* Android Browser 4.4+

To add support for older browsers, consider including [polyfills/shims](https://gist.github.com/paulirish/1579671) for the APIs listed above. There are no plans to include any in the library, in the interest of file size.

## What is tweening?

A tween is when you animate something with some kind of [easing](http://easings.net/). Any time you want to animate something smoothly with JS, you need to tween it. For example, you can tween count-up-buttons, smooth scrolling, and the height of elements. Instead of requiring libraries for all of these same functions, you can use this library as a utility to build out these examples.

## License

[MIT](https://github.com/jaxgeller/tweezer.js/blob/master/LICENSE)

[![Built With Love](http://forthebadge.com/images/badges/built-with-love.svg)](http://forthebadge.com)
