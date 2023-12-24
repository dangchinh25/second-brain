# Overview
- The main distinction between a BFF and a [[Central Aggregating Gateway]] is that a BFF is single purpose in nature => developed for a specific user interface. But it still solves the same [[Central Aggregating Gateway#^07a768 | problem]].

![](https://i.imgur.com/8xgnojb.jpg)
- A given BFF is for a specific user interface, assuming they are owned by the same team => No coupling between multiple teams.
> One user experience, One BFF
- The above remark we should have a specific BFF for each user experience, for example, one for web, one for iOS, one for android, maybe one for table?

# Problem
- One problem is that we may duplicate a lot of the logic between multiple BFF for a same product but different platform/devices/user experience
	- This can be mitigated if we make our core services *logic centric* enough, so the only part that is duplicating in multiple BFFs is the API call to our core services and some thin layer of logic on top of it.