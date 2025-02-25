---
layout: default
title: The Hangar
---
<h2>News Feeds</h2>

<div id="feed1"></div>
<div id="feed2"></div>
<div id="feed3"></div>

<script>
  // Wrap everything in document.ready so jQuery is definitely loaded
  $(document).ready(function() {
    console.log("Hangar feed script is running!");

    // Define the proxy variable (otherwise 'proxy' is undefined)
    var proxy = "https://api.allorigins.win/get?url=";

    console.log("Loading feed1...");
    $("#feed1").rss(proxy + encodeURIComponent("https://rss.nytimes.com/services/xml/rss/nyt/Technology.xml"), {
      limit: 3,
      layoutTemplate: "<ul>{entries}</ul>",
      entryTemplate: "<li><a href='{url}' target='_blank'>{title}</a></li>",
      onData: function() {
        console.log("feed1 loaded successfully");
      },
      onError: function() {
        console.error("feed1 failed to load");
      }
    });
  });
</script>
