# "Project #1 - MTA Turnstile Data Analysis"
---

Our first project at the Metis Data Science Bootcamp involved analyzing turnstile entry/exit data for the NYC subway system to optimize canvasser placement in order to promote an upcoming event.

## Prompt
Our client WomenTechWomenYes (WTWY), a fictional non-profit in New York dedicated to increasing the participation of women in the technology sector, is looking to promote their annual tech gala. WTWY employs 'street teams' of canvassers who approach people at entrances to NYC subways in an effort to collect their email addresses and send them free tickets to the gala. Importantly, WTWY is looking for people who are passionate about tech, and/or those who are able to donate to support the efforts of the organization.

## Approach and Assumptions
Before looking at the available data, we first set out to identify the potential stumbling blocks and better define the problem we were being asked to solve.

### General Assumptions
The project prompt was fairly open-ended, and we were instructed to make reasonable assumptions as though we had corresponded directly with WomenTechWomenYes via email. We assumed the following:
1. WTWY is employing a small, but variable number of canvassing 'street teams' depending on the outcome of the analysis
2. The tech gala for 2017 would take place in two weeks, so canvassing would be done right away

### Problem 1: Volunteer Constraints
There are 472 stops in the NYC subway system, and a yet-to-be-determined number of street teams will allocate limited time to canvassing potential gala attendees/future donors. Because of the scope of the problem, we knew we would have to pick a small number of high-traffic stops to maximize the number of people who would at least *see* the canvassers. More importantly, we wanted to optimize the time the street teams are canvassing to reach the right demographic, which leads to...

### Problem 2: Targeting the Right People
While rush hour means more people will get the message and canvassers will have more of an ability to approach people, we wanted to ensure that the teams would be working areas with a high number of people who find the WTWY cause interesting and relevant, and possibly those subway riders who are more likely to donate. We decided to locate stops in areas with the most tech employers, and pick times where these employees would be leaving work (and thus have more time to talk).

### Consideration: Busy Subway Stops = Impersonal Crowds
WomenTechWomenYes mentioned in the prompt that canvassing teams in prior years would stand at subway entrances. Given that we are choosing those subway stops with maximum passenger volume during rush hour, we suggested the WTWY place canvassers *on the subway platforms*. This would create something of a 'captive audience' for canvassing, and would ensure that riders are not able to simply breeze past the turnstiles (and away from the street teams).

## The Solution:
1. Locate the top tech companies in NYC, and constrain the list of potential subway stops based on proximity to these employers.
2. Determine historical ridership over the late spring of the past three years to get an accurate seasonal picture.
3. Rank stops within the filtered map area by ridership; WomenTechWomenYes will allocate resources by priority given that they did not specify the number of available canvassers.

### Step 1: Gathering Company Data
We used *Built in NYC*'s [list of the top 50 NYC startups](http://www.builtinnyc.com/2017/01/06/50-new-york-startups-watch-2017) as a proxy for the top tech employers in New York, starting by locating the latitude and longitude for each based on their addresses.

### Step 2: Mapping Companies & Constraining Search Area
##### Top 50 Startups, 2017 #####
##### Built in NYC ####
###### via gmaps #####

![Built in NYC - Top 50 Startups, 2017](https://github.com/reichsmarc/reichsmarc.github.io/blob/master/images/companymap.png?raw=true)

Our team used the *gmaps* module in Python to plot the tech employers on a map so we could determine appropriate geographical boundaries for our subway stop search. As you can see in the graphic, most of these employers are located in the area between 14th and 42nd Streets as would be expected given that the Flatiron District is the original hub of the tech scene in New York.

### Step 3: Mapping Subway Stops
##### Stations Between 14th and 42nd Streets vs. Built in NYC's Top 50 Startups ##### 
###### via gmaps ######  
![Stations Between 14th and 42nd Street (green) vs. Built in NYC's Top 50 Startups 2017 (blue)](https://github.com/reichsmarc/reichsmarc.github.io/blob/master/images/subwaycompanymap.png?raw=true)

We used the same approach to map the subway stops within our search area, shown in green above with employers in blue.

### Step 4: Ranking Subway Stops by Ridership
We used 18 weeks of historical MTA turnstile data (available [here](http://web.mta.info/developers/turnstile.html)) to find the stations with the highest ridership. Upon further analysis of the data, a few quirks stood out to us:
1. 98% of the data (for all 472 stations) is segmented into four-hour time blocks, e.g. 12AM to 4AM, 4AM to 8AM, 8AM to 12PM and so on.
1. Many records had non-uniform timestamps (e.g. 12:00:04 AM instead of 12:00:00 AM) or different time blocks altogether, making summary analysis more difficult.
1. An even larger number of records had turnstiles which were counting backwards, or registering zero riders over a given time period.
1. More records, still, had very low day-over-day rider counts which would defy intuition (for example, a turnstile at Grand Central with 11 entries in a single day).

Fortunately, filtering out stations not in the relevant geographical area significantly reduced the number of occurences of these issues in the data. We set a maximum and minimum ridership bound and manually classified further outliers based on the standard test of record +/- 1.5 x [interquartile range].

With the data cleaned, we bucketed records into overlapping time blocks of *six* hours which normalized most of the timestamp issues mentioned in /#2 above. This allowed us to arrive at the below ranking of station/time block combinations.

##### Ranked Ridership by Time Block and Subway Station #####
![Ranked Ridership by Time Block and Subway Station](https://github.com/reichsmarc/reichsmarc.github.io/blob/master/images/relativeridership.png?raw=true)

With this chart, WomenTechWomenYes will be able to efficiently allocate their street teams by time and location to maximize exposure and the potential of reaching the right audience with their canvassing efforts.
