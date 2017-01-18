---
layout: post
title:  "Ruby CLI Gem Project: RA Events"
date:   2017-01-18 14:11:03 -0500
---

Originally, I decided that I wanted to create a gem that scraped darksky.net for the weather at whatever location you specified around the world. I had finished that when I realized it didn't meet the basic requirement of this assignment, which was to have the scraper create many objects, not just one. I promise to read more carefully next time. 
After that project, I decided to something more standard. The gem I created scrapes the event listings on ResidentAdvisor.net which lists electronic music events around the world. Since there doesn’t seem to be any page on RA that lists the URL parameters for the top cities with events, I had to track them down one by one and hardcode them into the program. I would’ve preferred to scrape them, but I decided to live with hardcoding and move on to scraping the events. 

>     @cli_city = {"London, UK" => 1, "Berlin, DE" => 2, "Paris, FR" => 3,
>                 "Switzerland" => 4, "New York, US" => 5, "Amsterdam, NL" => 6,
>                 "Tokyo, JP" => 7, "Ibiza, ES" => 8, "Barcelona, ES" => 9,
>                 "South + East, UK" => 10}

I created a class hash for these url parameters in the scraper class.

```
@@cities = {13 => 1, 34 => 2, 44 => 3, 60 => 4, 8 => 5, 29 => 6, 27 => 7, 25 => 8, 20 => 9, 15 => 10}
```

There are two things needed in order for the scraper to create the URL to pass to Nokogiri so that the appropriate page can be retrieved: The city parameter above and the date. 

![](http://http://i.imgur.com/szppoth.jpg)

After asking the user for one of the top ten cities in the world for RA events, the gem lists two weeks’ worth of dates starting from today. When the user chooses one of the dates, the date gets split into year, month, and day and sent alone with the city parameter to the Scraper class. From there, the Scraper creates the URL and scrapes the selected day in the selected city. 

![](http://http://imgur.com/9oGpd56)

The Scraper class, in turn, creates the Event objects from each event on the event listing pages for that day in the given city. Finding the xpath selectors for each piece of information was straightforward, I used the Chrome extension [Xpath Helper](https://chrome.google.com/webstore/detail/xpath-helper/hgimnogjllphhhkhlmebbmlgjoejdpjl?hl=en) and inspect to track down the correct ones. 
The price method needed the most work since the information scraped from the price area on each event needed a fair amount of parsing. I found some code on Stack Exchange for a method that performs multiple gsubs on the same string. 
The CLI, after the scraper has completed creating the Event objects, lists the events for the given day and city. 

![](http://http://imgur.com/UU4NBi7)

Selecting the number of the event displays the event detail. 

![](http://http://imgur.com/l6OkQMC)

Then, you can return to the list, select a new date, return to the start to select a new city, or exit. 
The thing that took the longest on this project was the CLI. It’s fairly complicated and I’m sure could be simplified for readability. 

