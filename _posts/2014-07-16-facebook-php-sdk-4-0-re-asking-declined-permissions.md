---
title: "Facebook PHP SDK 4.0 - Re-asking declined permissions"
date: 2014-07-16
layout: single
classes: wide
excerpt: "How to re-request declined Facebook Login permissions with the PHP SDK 4.0 helper method."
author_profile: false
read_time: true
categories:
  - web-dev
  - php
  - facebook
tags:
  - php
  - facebook
  - sdk
  - permissions
  - oauth
header:
  image: /assets/images/posts/2014-07-16-facebook-php-sdk.svg
  teaser: /assets/images/posts/2014-07-16-facebook-php-sdk.svg
---
**UPDATE:**

Facebook PHP SDK now uses the *getReRequestUrl()* method of the *FacebookRedirectLoginHelper* class to generate a URL to re-request denied permissions from a user.

```
public string getReRequestUrl(string $redirectUrl, array $scope = [], string $separator = '&')
```

Read the documentation [here](https://developers.facebook.com/docs/php/FacebookRedirectLoginHelper/5.0.0#get-re-request-url).

---

So I was testing Facebook Login integration on the website [www.treasherlocked.com](http://www.treasherlocked.com/) that I have been developing for a while. Permissions like *email* and *user_location* were required by the web app. So, it was programmed to re-ask the denied permissions in the Login Dialog if a user denies any.

```
$facebook = new Facebook(APP_ID, APP_SECRET, REDIRECT_URI);
if ( $facebook->IsAuthenticated() ) {
// Verify if all of the scopes have been granted
if ( !$facebook->verifyScopes( unserialize(SCOPES) ) ) {
header( "Location: " . $facebook->getLoginURL( $facebook->denied_scopes) );
exit;
}
...
}
```

*Note: $facebook is a custom class that I built and not a part of Facebook PHP SDK.*

But it wasn't showing the Login Dialog again when permissions were denied. Instead Facebook was redirecting to *Redirect URI*, creating a redirect loop. I even [asked on Stack Overflow](http://stackoverflow.com/questions/24716168/facebook-php-sdk-4-0-cannot-re-ask-read-permissions-once-denied) but got no answer. As I was approaching the deadline, I couldn't afford waiting much and kept on looking for solutions. After hours of googling, I landed on a [Facebook Login API doc](https://developers.facebook.com/docs/facebook-login/login-flow-for-web/v2.0#re-asking-declined-permissions) page that actually addresses this issue. All that needs to be done is append a *rerequest = true* parameter to the login URL's query string. But this feature was not yet implemented in Facebook PHP SDK 4.0. There was a [proposal on GitHub](https://github.com/facebook/facebook-php-sdk-v4/issues/146) for this feature though. So I took the liberty of forking the project and made a small change in the *getLoginURL()* method's prototype and definition in the FacebookRedirectLoginHelper class. The *getLoginURL()* prototype then looked like

```
public function getLoginUrl($redirectUrl, $scope = array(), $rerequest = false, $version = null)
```

I sent a pull request to the project repo which was quickly merged. ([Read this article](https://www.sammyk.me/how-to-contribute-to-an-open-source-project-on-github) if you want to know how to contribute to an open source project if you aren't contributing already.)

So, if you need to re-ask declined permissions, all you have to do is pass *true* to the third parameter. The code on the callback script will look something like the following.

```
<?php
...
$helper = new FacebookRedirectLoginHelper();
if ($permissions_were_declined) {
header("Location: " . $helper->getLoginUrl( $redirect_uri, $declined_scopes, true );
exit;
}
...
?>
```
