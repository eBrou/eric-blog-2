---
title: 'Boolean Values Using Dotenv '
author: Eric Broucek
date: 2020-04-13T07:00:00Z
hero_image: ''

---
It's funny, sometimes you get so used to using a package or other piece of technology that you take it for granted and forget something fundamental about how it works under the hood.  One such instance came up last week on a project I was working on and the dotenv package. Unexpectedly, I was looking at requests in my network tab and noticed that while working on a feature I was making api requests with test credentials by mistake.  It took a bit of doubling back through my code to realize that the problem stemmed from the fact that the creds were being set based on a boolean flag - something like:

    USE_TEST_CREDS=false

Of course, dotenv loads all the variables from your .env file into process.env as strings so this has the potential to lead to some unexpected behavior.

    const apiKey = process.env.USE_TEST_CREDS ? process.env.TEST_KEY : process.env.PROD_KEY;

The above statement would assign the test key to the apiKey variable whether the value of the boolean flag were 'true' or 'false'.  This is because both are non-empty strings, which evaluate to true- not booleans (duh!).  A similar problem could arise if I set some variables to a number value, or to undefined, or null, and then tried to use them in my code as if they were the actual types instead of the strings '17', 'undefined' or 'null'.

There are a couple ways to handle this.  Some people write little functions or use various packages to check your variables when you import them and convert them to their intended types.  Others say to avoid setting flags as environment variables: instead of the logic to decide which one to use you would have a single variable in the .env file for your api key depending on the environment you're working in.

I find that in real world development I have to switch back and forth a lot - between projects, environments, etc - so utilizing environment variables as boolean flags can come in handy when I'm trying to get up and running quickly.  I don't feel like I need to overthink a solution so updating the logic in the above code to use an equivalency check will likely be my move going forward.

    const apiKey = process.env.USE_TEST_CREDS === 'true' ? process.env.TEST_KEY : process.env.PROD_KEY;

To wrap up, remember that the variables you access in process.env are always going to be strings.  Writing up this post (hopefully!) means I won't forget this little fact any time soon.