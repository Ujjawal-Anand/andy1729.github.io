---
title: "Trello for Mobile developers"
date: 2015-12-09 00:31:47
description: "An efficient solution that relies on using Trello vertically with custom labels"
---
Trello, as everyone knows, is a great project management tool, it's really easy and extremely flexible to fit many needs from digital work to daily life tasks.

Today, I want to expand that flexibility in the way to make it more gratefully for mobile developers Android and iOS.
The normal use case consists of using 3 default lists: `to-do`, `doing`, `done` which are correct and let us manage our tasks gratefully. 

<img src="https://d2k1ftgv7pobq7.cloudfront.net/meta/u/res/images/d1e04e4b8e9f93a9671be5dc719b4f79/guide-board.png" alt="Trello" width="100%"/>

However on the mobile development field, doing this same manipulation will make us lose tracking of changes between versions. I mean how do you keep track of what you're working on in this version, what was already shipped and finally what it's planned for later.  
 
For these needs, I've opted for an **alternative solution** that relies on using Trello vertically and it's resumed in 3 processes:  

- Creating new lists as many as our app versions  
- Renaming these lists according to a specific convention 
- Applying labels for each card according to the work status or type (working on, done, UI, bug ...)  


Let's see it in pictures:

<img src="{{ site.baseurl }}assets/images/trello_for_mobiledev.png" alt="Trello for mobile developers" />

As you saw, each list is versioned, named according to app version, respecting the following convention: `version name` - `version code` - `Shared + date of sharing`  
eg: `Beta - v0.4 (Shared on 01/01/2016)`  
The last `Shared on` item expresses if the app was shared with the client or even deployed on the store with the specified date of this action.
That's a basic convention that I'd recommend, but of course, you could tweak it as you like to match your needs.

Let's see labels: 
 
<img src="{{ site.baseurl }}assets/images/trello_labels_for_mobiledev.png" alt="Trello Labels" width="200"/>  
As you noticed before, we're no longer using default lists to indicate the status of our work since their role have changed to handle tasks relating to app versions.  
We'll substitute this lack by using labels which go perfectly with our case. Mandatory ones that should be available in any project are :  

- Doing
- Done  

These, are basic, add new labels as many as you need `UI`, `UX` or even `Bug` ...

### Bonus  

One more great thing, that's this new way of managing my Trello saved me time to mark which changes are made in each version, so no more worries about Changelogs on Google Play or AppStore. Here is an example taken from *Clean Master* app.

<img src="{{ site.baseurl }}assets/images/cleanmaster_changelog.png" alt="Clean Master" width="300"/>  


Hope this technique could improve your development workflow.
Let me know what's your favorite way of managing such projects ?