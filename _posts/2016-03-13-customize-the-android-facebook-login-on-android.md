---
layout: post
title: "Customize Facebook Login Button On Android"
permalink: customize-facebook-login-button-on-android
date: 2016-03-13 13:26:26
description: "Real solution, maintainable and easy to implement"
redirect_from: "/customize-the-android-facebook-login-on-android/"
---
If you do a quick lookup on stackoverflow, you'll find many answers (hacks) trying to solve this issue by editing the login button view to make it more suitable to the user need.

Being in the same position, I was looking for a real solution, maintainable and easy to implement and for such task, I could'nt find better than [Fancybuttons](https://github.com/medyo/Fancybuttons) library. Building buttons with this is like a lego, it supports icons, borders, hover color...

![Facebook connect]({{ site.baseurl }}assets/images/facebook_connect_demo.gif)

**Before starting**, I'll assume that you've already added Facebook and Fancybuttons librarires and setup all the necessary things ( if not yet check the full source code on the bottom of this page)

Let's create our new Facebook button then :


	<mehdi.sakout.fancybuttons.FancyButton
		android:id="@+id/facebook_login"
		android:layout_width="wrap_content"
		android:layout_height="45dp"
		android:paddingLeft="10dp"
		android:paddingRight="10dp"
		app:fb_radius="2dp"
		app:fb_iconPosition="left"
		app:fb_fontIconSize="20sp"
		app:fb_iconPaddingRight="10dp"
		app:fb_textSize="16sp"
		app:fb_text="Facebook Connect"
		app:fb_textColor="#ffffff"
		app:fb_defaultColor="#39579B"
		app:fb_focusColor="#6183d2"
		app:fb_fontIconResource="&#xf230;"
		android:layout_centerVertical="true"
		android:layout_centerHorizontal="true" />
	
	
You should have something like this :

<img src="{{ site.baseurl }}assets/images/facebook_connect_button.png" alt="Facebook Connect button" width="40%">

Let's implement the onClickListener and define our actions:   


	FacebookLogin.setOnClickListener(new View.OnClickListener() {
		@Override
		public void onClick(View view) {
			if (AccessToken.getCurrentAccessToken() != null){
				mLoginManager.logOut();
			} else {
				mAccessTokenTracker.startTracking();
				mLoginManager.logInWithReadPermissions(MainActivity.this, 				Arrays.asList("public_profile"));
			}
		}
	});
 


You could find the full source code on github : [https://github.com/medyo/custom-facebook-login-button](https://github.com/medyo/custom-facebook-login-button)


Good coding
