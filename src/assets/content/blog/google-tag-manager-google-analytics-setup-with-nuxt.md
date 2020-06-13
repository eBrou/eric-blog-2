---
title: Google Tag Manager / Google Analytics Setup with Nuxt
author: Eric Broucek
date: 2020-06-13T07:00:00Z
hero_image: "/src/assets/content/images/img_3289.jpg"

---
Much of this info comes from this [stack overflow post](https://stackoverflow.com/a/52885317/6933347) but I'm laying it out here with some additions mostly as a reminder for myself:

## Problem

When using GTM with Nuxt only initial page loads are counted as page view events, potentially missing navigation between app pages.

## Solution

Create a custom trigger in GTM to fire on nuxtRoute events coming from your app via the @nuxtjs/gtm module ([https://github.com/nuxt-community/gtm-module](https://github.com/nuxt-community/gtm-module "https://github.com/nuxt-community/gtm-module"))

1. Create a "container" in Google Tag Manager for your app or site. Note the container id # - e.g. GTM-XXXXXXX
2. Create a "property" in Google Analytics for your app or site. Note the property tracking id # - e.g. UA-XXXXXXXXX-X
3. yarn add @nuxtjs/gtm to your project.
4. Add @nuxtjs/gtm to the buildModules section of nuxt.config.js. Add GTM containter id and set the pageTracking option to true.

       export default { 
       	buildModules: [ '@nuxtjs/gtm', ], 
           gtm: { 
           	id: 'GTM-XXXXXXX', 
               pageTracking: true 
           } 
        }   
5. Define a custom event trigger to trigger off the nuxtRoute event which comes from @nuxtjs/gtm.

   ![creating a custom event](/src/assets/content/images/screen_shot_2020-06-10_at_2-41-59_pm.png "creating a custom event")
6. Create two Data Layer Variables for pageUrl and routeName (routeName is optional).  
   ![creating data layer variables](/src/assets/content/images/screen_shot_2020-06-10_at_2-43-26_pm.png "creating data layer variables")
7. Create the tag for Google Analytics in GTM. Select Track Type "Page View". If a Google Analytics Settings variable has not already been created you can quickly select a new one by providing your GA tracking id and leaving all the default settings in place. Click "Enable overriding settings in this tag" and under "More Settings" add two new fields for page and title. Set the value for "page" to the new pageUrl variable and set the value for "title" to the new routeName variable. Under triggering select the nuxtRoute trigger created above. Publish your changes or test in preview mode.  
   ![creating data layer variables](/src/assets/content/images/screen_shot_2020-06-10_at_2-46-04_pm.png "creating a Google analytics tag")
8. Open up your app or site. Go to your GA dashboard and watch your current activity live (with a small lag). You should see the page change when you navigate between pages. (Note: page view reports won't show much of anything until at least one day has passed)  
   ![Activity in Google Analytics dashboard](/src/assets/content/images/screen_shot_2020-06-10_at_2-59-52_pm.png "Activity in Google Analytics dashboard")