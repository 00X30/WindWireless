---
layout: default
title: The Hangar
---
<h2>News Feeds</h2>

<div id="feed1"></div>
<div id="feed2"></div>
<div id="feed3"></div>

<script>
$(document).ready(function() {
  // Helper function to load an RSS feed with a CORS proxy
  function loadFeed(selector, feedUrl) {
    // Using a free CORS proxy (AllOrigins) - note: sometimes proxies have rate limits.
    var proxiedUrl = "https://api.allorigins.hexocode.repl.co/get?disableCache=true&url=" + encodeURIComponent(feedUrl);
    $(selector).rss(proxiedUrl, {
      limit: 5,
      layoutTemplate: "<ul>{entries}</ul>",
      entryTemplate: '<li><a href="{url}" target="_blank">{title}</a></li>'
    });
  }

  // Replace these feed URLs with your desired RSS feeds.
  loadFeed("#feed1", "https://rss.nytimes.com/services/xml/rss/nyt/Technology.xml");
  loadFeed("#feed2", "https://feeds.bbci.co.uk/news/technology/rss.xml");
  loadFeed("#feed3", "https://www.wired.com/feed/category/gear/latest/rss");
});
</script>
