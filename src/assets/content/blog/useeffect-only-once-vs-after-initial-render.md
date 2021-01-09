---
title: useEffect - Only Once vs After Initial Render
author: Eric Broucek
date: 2021-01-08T08:00:00Z
hero_image: "/src/assets/content/images/img_5164.jpg"

---
Well, I didn't do the greatest job posting over the last 6 months.  Turns out that between work and family the pandemic has been an incredibly busy time for me.  However, I did jot down a number of ideas while working with some new technologies (to me) that I'll try to catch up on in the near future.

First one is a quickie note to self.  I've been on a project for the last 4-5 months using React, and this is the first time I've been able to use hooks on a regular basis.  They took some getting used to but for most common uses I find they're a huge improvement.

`useEffect` replaces the need to use the old lifecycle methods `componentDidMount` and `componentDidUpdate` when watching for changing props or other values in order to perform some side effect.  You simply pass in a callback and it will perform that function on every render.  And by passing in a second argument of an array of values to watch, the callback will only run when one of those values have changed.  Something like this:

    useEffect(() => { 
    	console.log("useEffect callback fires only when watchedVar1 or watchedVar2 have changed!")
    },[watchedVar1, watchedVar2])

But what if you want the effect to only run on the initial render?  Well it turns out all you need to do is pass an empty array for that second  argument and you're all set!

    useEffect(() => { 
    	console.log("useEffect callback only fires on initial render")
    },[])

From [the docs](https://reactjs.org/docs/hooks-effect.html "useEffect docs"): "This tells React that your effect doesn’t depend on any values from props or state, so it never needs to re-run." The docs also link you to 

So how do you do the opposite and skip the initial render but ensure that your effect runs on subsequent updates?  Well, you can use the `useRef` hook in conjunction with `useEffect`.  According to [the docs](https://reactjs.org/docs/hooks-reference.html#useref "useRef docs"), `useRef` essentially "give\[s\] you the same ref object on every render" so you can use it to set a flag on your first render to say "hey, this already rendered once".  Then you check that value inside `useEffect` to determine whether or not the callback should be run, like so: