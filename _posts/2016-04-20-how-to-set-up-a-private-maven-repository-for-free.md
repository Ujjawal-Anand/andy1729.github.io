---
layout: post
title: "How to Set Up a Private Maven Repository For Free"
permalink: how-to-set-up-a-private-maven-repository-for-free
date: 2016-04-20 22:19:19
comments: true
description: "Easy Peasy Solution to Share custom android dependencies privately between teamates"
keywords: "maven, free, repository, android, gradle, java"
---

Being a fan of factorisation, I recently decided to extract some redundant modules from my released Android Apps and make them standalone to be used in future projects. I ended up having easy to use dependencies like this `compile {libraryName}`, seems cool ? nah ?   

This guide applies only for **private projects** that you'd like to keep in secret, or share solely with agency teammates... if you'd like to go public, check Bintray, Maven Central or Jitpack.

>At the end of this guide, you'll be able to have something like :   `compile "my.company.library:version"`

<br/>
Ready ? **Let's get Go**   

My current configuration is :

* Gradle 2.11  
* Android Plugin for Gradle 2.1.0-beta1 
* Android Studio 2.1
* MacOS X 10.10.5

---
<img src="{{ site.baseurl }}assets/images/mymavenrepo.png" alt="MyMavenRepo" width="40%" align="center">

Instead of building a private maven server repository which costs time and money, I've opted for a self hosted version called named [MyMavenRepo.com](https://mymavenrepo.com/), **At the time of writing this article**, the service remains free and offers 250mb of data plus supports maven and gradle build types.

#### 1 - Setup an Account
Okay, start by creating a new account, you'll receive the password in your inbox.

#### 2 - Setup Dev Environment
Once on your dashboard, you'll identify two different URLs: 

`Read URL : https://mymavenrepo.com/repo/myUniqueID/`  
`Write URL : https://mymavenrepo.com/repo/myUniqueID/`

These urls are used to read/write data to our repository, they come with a unique ID to identify your repository, make sure to keep them private, if you don't set any permissions, anyone could access your content.

On the `HTTP Basic Auth` Section, click on `enable HTTP Basic Auth` to be able to restrict access by username and password.

<img src="{{ site.baseurl }}assets/images/mymavenrepo_urls.png" alt="MyMavenRepo Dashboard"/>

Once done, you'll have to setup the authentification details, Click on <span style="padding:5px;background-color:#5DBD8E;color:white">Add User</span> and define your default user for both READ and WRITE urls, I won't cover this part, it's really up to you.  

Now, it's time to export all these variables to our `gradle.properties`

	$ vim .gradle/gradle.properties

Change with your password and repo url:

	myMavenRepoReadPassword=myPassword
	myMavenRepoWritePassword= myPassword
	myMavenRepoReadUrl=https://mymavenrepo.com/repo/uniqueID/
	myMavenRepoWriteUrl=https://mymavenrepo.com/repo/uniqueID/


#### 3 - Update your dependency project and Push Content
Upgrade your library `build.gradle` file like below

~~~
apply plugin: 'maven-publish'
	
group = 'my.company'
archivesBaseName = 'android-library'
version = '0.1.4-SNAPSHOT'
	
publishing {
    repositories {
        maven {
            url myMavenRepoWriteUrl
            credentials {
                username 'myMavenRepo'
                password myMavenRepoReadPassword
            }
        }
    }
	
    publications {
        aar(MavenPublication) {
            groupId group
            artifactId archivesBaseName
            version project.version
			
			// Optional
            pom.withXml {
                asNode().appendNode('name','Dependency Name')
                asNode().appendNode('description','Dependency Description')
                asNode().appendNode('url','http://medyo.github.com')
            }

            artifact("$buildDir/outputs/aar/${archivesBaseName}-release.aar")        }
    }
}
~~~

Ready ? Go

	./gradlew clean build publish
	
If things go like expected, you should see in log the remote path of your dependency

<img src="{{ site.baseurl }}assets/images/mymavenrepo_upload.png" alt="MyMavenRepo Upload Gradle"/>


#### 4 - Distribution

Hooray ! You're done, your library is ready to be shared with your teammates, Just Ask them to upgrade their gradle files

Add this to your top `build.gradle` file:

	maven {
		url "https://mymavenrepo.com/repo/UniqueID/"
		credentials {
			username = 'myMavenRepo'
			password = '********'
		}
	}


And integrate your dependency like you you've always done :

	compile 'my.company:android-library:0.1.4-SNAPSHOT'
	
For any suggestions or questions, please post your comment below.
Thanks
