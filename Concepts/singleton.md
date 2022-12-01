# All About Singleton

## Query

I've been reading that singleton is an anti-pattern but it helps when I want to connect to a DB instance.

## Findings

There are so many thoughts. For starters, the above is a very generic statement. Some languages rely on singletons, and some languages don't support them at all. I think that everything has a place. And as long as the consequences of doing a thing are understood, and acceptable, then it is ok to do the thing.

Singletons. People normally say that they are bad - my feeling is they are misunderstood. If you understand that every time you require (CommonJS) or import (ESM) a file, you will receive the same instance of the file as long as you are running in the same thread, then you can use this to your advantage - i.e. what I like to refer to as a ‘poor man's cache’ it's a cheap and easy way to cache an expensive value (like a database connection) as long as you don't rely on it always (if your js runs in a new thread, you won't have the old value).

I often encourage people to look at the basics of functional programming. Specifically, in this case, the concepts of functions without side effects, vs functions with side effects & pure functions. A side effect is when a function manipulates something that is outside its scope (like a database) a pure function will always produce the same output when you give it the same input.

If a function takes a DB connection as an input and read from the DB, it does not have side effects, but it is not pure.
because you are dependent on the state of the external service (the DB).

Loosely speaking, you should try to keep all your non-pure or side-effect production functions together, and isolated from your pure functions.

While people will say things like “it's not functional programming if you have even one non-pure function” that's silly because in modern development, like web development - the act of rendering to a browser itself is a side effect.

anyways - back to the question - singletons and non-pure functions go hand in hand in my opinion. they both follow the same concepts - use them wisely, but most importantly, understand the implications and the consequences, and accept them.

So I guess summarising, on singletons.

- Don't overuse them
- Understand the intent of why they are useful
- Make careful decisions about when to use them
- Consider what the alternatives are

Wrapping that up into a logical example - the DB connection that has been raised, a singleton class, will give you access to the DB connection anywhere you need it. The alternative is that you initiate the class once, and you pass it around up and down the stack to where you need it. This becomes quite messy and will introduce unnecessary complexity.

React state is another good example. In the beginning, when learning to React (Vue is probably the same) you pass props up and down your component stack. Over time, you get more and more components. Then you end up ‘prop drilling’.

So Redux becomes popular, and you use essentially a singleton state object - that you can call up when needed. Redux abstracts a lot of the complexity - but at its core - it’s baked into the react Context - which is a singleton concept for providing a shared context.
