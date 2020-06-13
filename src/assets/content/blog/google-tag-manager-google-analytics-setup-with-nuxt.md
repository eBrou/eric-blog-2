---
title: Google Tag Manager / Google Analytics Setup with Nuxt
author: Eric Broucek
date: 2020-06-13T07:00:00Z
hero_image: ''

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
6. 