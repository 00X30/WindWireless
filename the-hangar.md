---
layout: default
title: The Hangar
---
<h2>News Feeds</h2>

<div id="feed1"></div>
<div id="feed2"></div>
<div id="feed3"></div>

<script>
 console.log("Loading feed1...");
$("#feed1").rss(proxy + encodeURIComponent("https://rss.nytimes.com/services/xml/rss/nyt/Technology.xml"), {
  limit: 3,
  layoutTemplate: "<ul>{entries}</ul>",
  entryTemplate: "<li><a href='{url}' target='_blank'>{title}</a></li>",
  // A callback to confirm it's done:
  onData: function() {
    console.log("feed1 loaded successfully");
  },
  onError: function() {
    console.error("feed1 failed to load");
  }
});

</script>
