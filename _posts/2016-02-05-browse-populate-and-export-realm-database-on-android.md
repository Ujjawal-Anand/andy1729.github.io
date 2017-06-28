---
title: "Browse, Populate and Export Realm Database On Android"
date: 2016-02-05 00:13:17
description: "Browse, Populate and Export Realm Database On Android"
---
I remember discovering [Realm](https://realm.io/) library on [qudos.io](http://qudos.io), it was really something magic compared to other database libraries. Realm is written in c++ and describe itself as a SQLite replacement.
According to many benchmarks, Realm provides superior performance and cool queries syntax that really motivated me to give it a try...

<img src="http://zanon.io/images/posts/2015-11-01-benchmark.png" alt="Realm Browser" width="100%">
[Source](http://zanon.io/posts/realm-an-incredible-fast-mobile-db)

Realm really met all my expectations, so I decided to test it on production. One of the issues that actually bothered me, is the lack of a database viewer especially for Linux since I'm switching between Ubuntu and Mac all the time, So I decided to make a quick tutorial on how to browse Realm database on these these environments.

#### For Mac users: 
If you're a Mac user, Realm team already provides [Realm Browser](https://itunes.apple.com/app/realm-browser/id1007457278), please refer to [the next paragraph](#pull-database) to figure out how to extract the database from your device.  

##### <a name="pull-database"></a>How to pull database from the device

Since this task might be repetitive along your development process, I've made a quick bash script that pulls the database from the device to your desktop. The script is very simpl, pull requests are more than welcome.

<script src="https://gist.github.com/medyo/f4b4e3efb5f2299a8a1c.js"></script>  


#### For Linux and Windows users:
Things are different here, Realm doesn't provide any official browser for Linux (Ubuntu …) or Windows. However we're hackers, finding solutions is our hobby :)

We will rely on [Stetho](http://facebook.github.io/stetho/), a Facebook library that works as debugging bridge, we will combine it with [Stetho-Realm](https://github.com/uPhyca/stetho-realm), a module that make Realm database readable by Stetho.

Enough talk, **Let's see that in action** :  
 
1. Install the following dependencies

~~~
repositories {
    maven {
        url 'https://github.com/uPhyca/stetho-realm/raw/master/maven-repo'
    }
}
dependencies {
    compile 'com.facebook.stetho:stetho:1.3.0' 
    compile 'com.uphyca:stetho_realm:0.8.0'
}

~~~

2. initialize stetho on your Application class (onCreate) :  

~~~
public class MyFancyApp extends Application {
@Override
    public void onCreate() {
    	super.onCreate();
        Stetho.initialize(
			Stetho.newInitializerBuilder(this)
			.enableDumpapp(Stetho.defaultDumperPluginsProvider(this))
			.enableWebKitInspector(RealmInspectorModulesProvider.builder(this).build())
		   build());
	}
}
~~~ 

3. Launch your app
4. Once started, Open Chrome Browser, type `chrome://inspect` and choose your running device then navigate to `resources` and finally `Web Sql`.
5. And voila, you should see your database entries there.


<img src="{{ site.baseurl }}assets/images/realm-stetho.png" alt="realm stetho" />