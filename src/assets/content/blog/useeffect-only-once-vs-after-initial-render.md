---
title: useEffect - Only Once vs After Initial Render
author: Eric Broucek
date: 2021-01-08T08:00:00Z
hero_image: "/src/assets/content/images/img_5164.jpg"

---
Well, I didn't do the greatest job posting over the last 6 months.  Turns out the pandemic has been an incredibly busy time for me.  However, I did jot down a number of ideas while working with some new technologies that I'll try to catch up on in the near future.

First one is a quickie note to self.  I've been on a project for the last 4-5 months using React, and this is the first time I've been able to use hooks on a regular basis.  They took some getting used to but for most common uses it's a huge improvement.

`useEffect` replaces the need to use the old lifecycle methods `componentDidMount` and `componentDidUpdate` when watching for changing props in order to update some state or perform some other side effect.  You simply pass in a callback and it will perform that function on every render.  By passing in a second argument of an array of values to watch, the callback will only run when one of those values have changed.  Something like this:

\`useEffect(() => {

console.log("useEffect callback fires!)

},\[watchedVar1, watchedVar2\])\`