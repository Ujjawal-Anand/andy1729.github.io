---
title:  "Different Android app resources (icon, title ...) according to gradle build type"
date:   2015-11-28 10:13:00
description: Useful trick to keep a clean android development workspace
---

In the usual Android development process, coding, releasing and testing, is a repetitive flux that involves generating new `apk` each time a new build is done. I know, At this point everything is ordinary.

The issue is trigged when you want to have many versions of the same app, on the same device without the need to erase and reinstall the new app. 

For example keeping the production `release` app while testing the beta `build` version, or testing the `trial` version without losing the `paid` or `staging` versions.

- my.package.name
- my.package.name.**trial**
- my.package.name.**paid**
- my.package.name.**staging**


~~~
// app/build.gradle
android {
	buildTypes {
		release{}
		trial{
			applicationIdSuffix ".trial"
		}
		paid{
			applicationIdSuffix ".paid"
		}
		staging{
			applicationIdSuffix ".staging"
		}
	}
}
~~~

The *manipulation* is really easy; it consists on adding a custom suffix to the app package name so we could have unique names for each build type. At this step, we've successfully separated package names, let's set custom names and icons for each of these items.
We will create multi directories inside the `src` folder, having the same name as build types defined above on your `gradle.build` file.

~~~
app
| - src
	| - debug
	| - trial
	| - staging
	| - main (default one)

~~~
These new added folders are acting like the default `main` folder, they get called according to the choosed build type, do whatever changes you want.

<img src="http://www.awesomescreenshot.com/upload/162377/167439/45118ccc-4286-4d7b-5dfc-cf8245537139.png" alt="Elbotola build types" width="150">

In this example ([Elbotola App](https://play.google.com/store/apps/details?id=com.mobiacube.elbotola)), I've changed the title and added an orange badge to the app icon to differentiate between `release` and `beta` versions

<img src="http://www.awesomescreenshot.com/upload/162377/167439/b91c3bd2-32c4-4173-5c42-884ecfc76984.png" alt="Build types hierarchy" width="240">

That trick is really useful to keep a clean workspace, hope that improves your development workflow.

Happy coding
