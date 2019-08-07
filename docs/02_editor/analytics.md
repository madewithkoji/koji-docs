# Analytics

## Overview

There are two types of analytics built into Koji. You don't need to do any 
additional configuration in order to use these tools.

## App stats

You can see the number of views and remixes for your app by finding it in your 
"My Projects" list. The number of views is the number of times your app has been 
viewed on WithKoji.com or via an embedded player. It does not count any views 
directly to the app's domain.

## Server logs

You can see some additional basic analytics for apps you've deployed by opening 
"Analytics" from the "Tools" section of your project.

These analytics are generated based on the access logs for files in your app 
and count unique viewers by IP address. KojiCDN also parse the logs in order to 
aggregate referrers, and map IPs to coarse geolocation in order to aggregate 
the locations of your users.

While access logs can yield useful high-level information, these logs provide 
only the most basic information about what files are requested and the (general) 
geographic area where the request originated. If you want more access to more
detailed analytics in order to understand the behavior of users on your app,
you have a few options:

- The [Google Analytics](https://analytics.google.com) plugin can be enabled in the plugins section of your app. Simply sign up for Google Analytics, create a new property, and paste the ID in the plugin configuration to automatically enable Google Analytics in your app
- You can embed any third party analytics tool in your app at the code level
