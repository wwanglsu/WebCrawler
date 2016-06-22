# WebCrawler
This is a simple Web Crawler application using JavaScript based on Nodejs.

JS notes:
Usually callbacks are only used when doing I/O, e.g. downloading things, reading files, talking to databases, etc.

When you call a normal function you can use its return value:

var result = multiplyTwoNumbers(5, 10)
console.log(result)
// 50 gets printed out
However, functions that are async and use callbacks don't return anything right away.

var photo = downloadPhoto('http://coolcats.com/cat.gif')
// photo is 'undefined'!
In this case the gif might take a very long time to download, and you don't want your program to pause (aka 'block') while waiting for the download to finish.

Instead, you store the code that should run after the download is complete in a function. This is the callback! You give it to the downloadPhoto function and it will run your callback (e.g. 'call you back later') when the download is complete, and pass in the photo (or an error if something went wrong).

downloadPhoto('http://coolcats.com/cat.gif', handlePhoto)

function handlePhoto (error, photo) {
  if (error) console.error('Download error!', error)
  else console.log('Download finished', photo)
}

console.log('Download started')


Note that the handlePhoto is not invoked yet, it is just created and passed as a callback into downloadPhoto. But it won't run until downloadPhoto finishes doing its task, which could take a long time depending on how fast the Internet connection is.

This example is meant to illustrate two important concepts:

The handlePhoto callback is just a way to store some things to do at a later time
The order in which things happen does not read top-to-bottom, it jumps around based on when things complete.


Summary:
Don't nest functions(callback hell). Give them names and place them at the top level of your program
Use function hoisting to your advantage to move functions 'below the fold'
Handle every single error in every one of your callbacks. Use a linter like standard to help you with this.
Create reusable functions and place them in a module to reduce the cognitive load required to understand your code. Splitting your code into small pieces like this also helps you handle errors, write tests, forces you to create a stable and documented public API for your code, and helps with refactoring.
The most important aspect of avoiding callback hell is moving functions out of the way so that the programs flow can be more easily understood without newcomers having to wade through all the detail of the functions to get to the meat of what the program is trying to do.

You can start by moving the functions to the bottom of the file, then graduate to moving them into another file that you load in using a relative require like require('./photo-helpers.js') and then finally move them into a standalone module like require('image-resize').

Here are some rules of thumb when creating a module:

Start by moving repeatedly used code into a function
When your function (or a group of functions related to the same theme) get big enough, move them into another file and expose them using module.exports. You can load this using a relative require
If you have some code that can be used across multiple projects give it it's own readme, tests and package.json and publish it to github and npm. There are too many awesome benefits to this specific approach to list here!
A good module is small and focuses on one problem
Individual files in a module should not be longer than around 150 lines of JavaScript
A module shouldn't have more than one level of nested folders full of JavaScript files. If it does, it is probably doing too many things
