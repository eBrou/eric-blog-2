---
title: React useEffect Hook - Only Once vs Only On Subsequent Renders
author: Eric Broucek
date: 2021-01-08T08:00:00.000+00:00
hero_image: "/src/assets/content/images/img_5164.jpg"

---
Well, I didn't do the greatest job posting over the last 6 months.  Turns out that between work and family the pandemic has been an incredibly busy time for me.  However, I did jot down a number of ideas while working with some new (to me) technologies that I'll try to catch up on in the near future.

I've been on a project for the last 5 months using React, and this is the first time I've been able to use hooks on a regular basis.  I was familiar with class-based components so hooks took a little getting used to but for the most part I find they're a huge improvement.  And they can mean so much less code to write!

`useEffect` replaces the need to use the old lifecycle methods `componentDidMount` and `componentDidUpdate` when watching for changing props or other values in order to perform some side effect.  You simply pass in a function as an argument and it will perform that function on every render.  And by passing in a second argument of an array of values to watch, the function will run only if one of those values has changed.  Something like this:

    useEffect(() => { 
    	console.log("useEffect function fires only when watchedVal1 or watchedVal2 have changed!")
    },[watchedVal1, watchedVal2])

But what if you want the effect to only run on the initial render?  Well it turns out all you need to do is pass an empty array for that second  argument and you're all set!

    useEffect(() => { 
    	console.log("useEffect function only fires on initial render")
    },[])

From [the docs](https://reactjs.org/docs/hooks-effect.html "useEffect docs"): "This tells React that your effect doesn’t depend on any values from props or state, so it never needs to re-run." 

So how do you do the opposite and skip the initial render but ensure that your effect runs on subsequent updates?  Well, you can use the `useRef` hook in conjunction with `useEffect`.  According to [the docs](https://reactjs.org/docs/hooks-reference.html#useref "useRef docs"), `useRef` "give\[s\] you the same ref object on every render" so you can use it to set a flag on your first render to say "hey, this already rendered once".  Then you check that value inside `useEffect` to determine whether or not you should run your function, like so:

    const firstRenderRef = useRef(false);
    
    useEffect(() => {
    	if (firstRenderRef.current) {
        	console.log("Block only runs AFTER initial render");
        } else {
        	firstRenderRef.current = true;
        }
    }, [watchedVal1, watchedVal2]);

This could also be made into a custom hook very easily with arguments for your function and any values to be watched:

    function useDidUpdate(func, values) {
      const firstRenderRef = useRef(false);
    
      useEffect(() => {
        if (firstRenderRef.current) {
        	func();
        } else {
        	firstRenderRef.current = true;
        }
      }, inputs);
    }

Import this hook into your component and its usage would look just like the original `useEffect` example above.

    useDidUpdate(() => { 
    	console.log("useDidUpdate function only fires AFTER initial render")
    },[watchedVal3])

Both situations - running some function on just the first render and running it only on subsequent renders - are bound to come up when working on a React project so it's good to keep these approaches handy.