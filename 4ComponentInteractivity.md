# 4 - Component Interactivity



In the end, React is just plain JavaScript. They have resisted the urge to add a lot of magic and have made strides to remove magic to reduce coupling to the magic. That was abstract, but I'm just saying that loose coupling is a good thing because it enable reuse. The React team believes our code should have a lot of coupling to React. 

One example of this is function context binding. There was a time when React automatically bound functions to `this`. If you aren't a JavaScript developer there is a chance that you don't know that `this` in JavaScript is different from `this` in C# or Java or `self` in Ruby.

In some strongly typed languages, `this` is bound to an object instance. In JavaScript `this` is bound to the function it is called in. This causes a lot of confusion, but this loose coupling allows us to reuse functions. We can define the context for a function and have `this` represent what we want it to.



