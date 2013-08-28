---
layout: post
title: "Setting up the Twitter Helper with ASP.NET"
date: 2013-08-26 15:09
comments: true
categories:  asp.net
published: true
---

It is a simple tutorial on how to create your own twitter helpers in ASP.NET website.

<!-- more -->

## Create Buttons with Helpers

### 1. Follow Button

The [Twitter API: Follow Button](https://dev.twitter.com/docs/follow-button) uses Javascript and data attributes to configure the button:

``` html
<a href="https://twitter.com/aspnet" class="twitter-follow-button" data-show-count="false">Follow @aspnet</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>
```

It will be displayed as

<a href="https://twitter.com/aspnet" class="twitter-follow-button" data-show-count="false">Follow @aspnet</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>

We can create a new button with configurations provided by twitter directly at [twitter.com/about/resources/buttons](https://twitter.com/about/resources/buttons). On the other hand, to build our own reusable helper button in ASP.NET website, create `Twitter.cshtml` in the folder `App_Code\`, and add the following to `App_Code\Twitter.cshtml`.

``` html
@using System.Globalization

@* For more about the twitter follow button, please visit https://dev.twitter.com/docs/follow-button *@ 
@helper FollowButton(
            string userName,
            bool showCount = false,
            bool showUserName = true,
            bool largeButton = false,
            bool optOutOfTailoring = false,
            string language = "",
            bool alignLeft= true) {
    HtmlString showCountAttribute = new HtmlString(showCount ? "" : "data-show-count=\"false\"");
    HtmlString showUserNameAttribute = new HtmlString(showUserName ? "" : "data-show-screen-name=\"false\"");
    HtmlString largeButtonAttribute = new HtmlString(largeButton ? "data-size=\"large\"" : "");
    HtmlString optOutOfTailoringAttribute = new HtmlString(optOutOfTailoring ? "data-dnt=\"true\"" : "");
    HtmlString languageAttribute = new HtmlString(!language.IsEmpty() && !language.Equals("en", StringComparison.OrdinalIgnoreCase) ? String.Format(CultureInfo.InvariantCulture, " data-lang=\"" + HttpUtility.HtmlAttributeEncode(language) +"\"") : "");
    HtmlString alignAttribute = new HtmlString(alignLeft ? "" : "data-align=\"right\"");
<a href="https://twitter.com/@HttpUtility.UrlEncode(userName)" class="twitter-follow-button" @showCountAttribute @showUserNameAttribute @largeButtonAttribute @optOutOfTailoringAttribute @languageAttribute @alignAttribute>Follow @("@" + userName)</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>
}
```

After setting up the helper, it can be called in our page with `@Twitter.FollowButton()`. In our example, we should send at least one parameter specifying the `userName` we would like to follow. Assume we need a button following [The ASP.NET Team(@aspnet)](https://twitter.com/aspnet), showing the follower count, with a small size, in the language of spanish ([ISO-639-1 Language Code](http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)):

``` html
@Twitter.FollowButton(userName: "aspnet", showCount: true, largeButton: false, language: "es")
```

It will be displayed as

<a href="https://twitter.com/aspnet" class="twitter-follow-button" data-show-count="false" data-lang="es">Seguir a @aspnet</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>

### 2. Tweet Button

Similarly, we can both utilize the tool provided by [twitter.com/about/resources/buttons#tweet](https://twitter.com/about/resources/buttons#tweet) and create our own helper.

In order to create our own helper, add the following to `App_Code\Twitter.cshtml`.

``` html
@* For more about the tweet button, please visit https://dev.twitter.com/docs/tweet-button *@
@helper TweetButton(string url = "",
            string tweetText = "",
            bool showCount = true,
            string via = "",
            string recommend = "",
            string hashtag = "",
            bool largeButton = false,
            bool optOutOfTailoring = false,
            string language = "") {
    HtmlString urlAttribute = new HtmlString(url.IsEmpty() ? "" : String.Format(CultureInfo.InvariantCulture, " data-url=\"" + HttpUtility.HtmlAttributeEncode(url) + "\""));
    HtmlString tweetTextAttribute = new HtmlString(tweetText.IsEmpty() ? "" : "data-text=\"" + HttpUtility.HtmlAttributeEncode(tweetText) + "\"");
    HtmlString showCountAttribute = new HtmlString(showCount ? "" : "data-show-count=\"false\"");
    HtmlString viaAttribute = new HtmlString(via.IsEmpty() ? "" : "data-via=\"" + HttpUtility.HtmlAttributeEncode(via) + "\"");
    HtmlString recommendAttribute = new HtmlString(recommend.IsEmpty() ? "" : "data-related=\"" + HttpUtility.HtmlAttributeEncode(recommend) + "\"");
    HtmlString hashtagAttribute = new HtmlString(hashtag.IsEmpty() ? "" : "data-hashtags=\"" + HttpUtility.HtmlAttributeEncode(hashtag) + "\"");
    HtmlString largeButtonAttribute = new HtmlString(largeButton ? "data-size=\"large\"" : "");
    HtmlString optOutOfTailoringAttribute = new HtmlString(optOutOfTailoring ? "data-dnt=\"true\"" : "");
    HtmlString languageAttribute = new HtmlString(!language.IsEmpty() && !language.Equals("en", StringComparison.OrdinalIgnoreCase) ? String.Format(CultureInfo.InvariantCulture, " data-lang=\"{0}\"", HttpUtility.HtmlAttributeEncode(language)) : "");
<a href="https://twitter.com/share" class="twitter-share-button" @urlAttribute @tweetTextAttribute @showCountAttribute @viaAttribute @recommendAttribute @hashtagAttribute @largeButtonAttribute @optOutOfTailoringAttribute @languageAttribute>Tweet</a>
<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>
}
```

Assume that we need a tweet button sharing the link to `http://www.asp.net/mvc`, with a large size and a hashtag `awesome`, and in the language of English.

``` html
@Twitter.TweetButton(url: "http://www.asp.net/mvc", largeButton: true, hashtag: "awesome")
```

It will be displayed as

<a href="https://twitter.com/share" class="twitter-share-button" data-url="http://www.asp.net/mvc" data-size="large" data-count="none" data-hashtags="awesome">Tweet</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>

## Create Registered Widgets

In Twitter API v1.1, widgets, including profile, favorites, search and list, are migrated to [Embedded Timelines](https://dev.twitter.com/docs/embedded-timelines). The user should register the widgets at [twitter.com/settins/widgets](https://twitter.com/settings/widgets). Unauthenticated requests are not supported.

### 1. User Timeline (Profile)

Log in to twitter and go to  [twitter.com/settingts/widgets](https://twitter.com/settings/widgets). [Click `Create new`](https://twitter.com/settings/widgets/new), set up our own configuration, and click `Create widget`. Finally, copy and paste the code into the page of our site.

``` html
<a class="twitter-timeline" href="https://twitter.com/aspnet" data-widget-id="369912910400593920">Tweets by @aspnet</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>
```

The code above will be shown as

<a class="twitter-timeline" href="https://twitter.com/aspnet" data-widget-id="369912910400593920">Tweets by @aspnet</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>

### 2. Favorites

Go to [twitter.com/settings/widgets/new/favorites](https://twitter.com/settings/widgets/new/favorites). Set up our own configuration. Click `Create widget`, copy and paste the code into the page of our site.

``` html
<a class="twitter-timeline" href="https://twitter.com/aspnet/favorites" data-widget-id="369909963851714561">Favorite Tweets by @aspnet</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>
```

It will be displayed as:

<a class="twitter-timeline" href="https://twitter.com/aspnet/favorites" data-widget-id="369909963851714561">Favorite Tweets by @aspnet</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>

### 3. List

Go to [twitter.com/settings/widgets/new/list](https://twitter.com/settings/widgets/new/list). Set up our own configuration. Click `Create widget`, copy and paste the code into the page of our website.

``` html
<a class="twitter-timeline" href="https://twitter.com/puncsky/blog" data-widget-id="369913408134467584">Tweets from @puncsky/blog</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>

```

It will be like:

<a class="twitter-timeline" href="https://twitter.com/puncsky/blog" data-widget-id="369913408134467584">Tweets from @puncsky/blog</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>

### 4. Search

Go to [twitter.com/settings/widgets/new/search](https://twitter.com/settings/widgets/new/search). Set up our own configuration. Click `Create widget`, copy and paste the code into the page of our website.

``` html
<a class="twitter-timeline" href="https://twitter.com/search?q=%23asp.net" data-widget-id="369910441469689856">Tweets about "#asp.net"</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>

```

It will be displayed as:

<a class="twitter-timeline" href="https://twitter.com/search?q=%23asp.net" data-widget-id="369910441469689856">Tweets about "#asp.net"</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>

