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
  // Function to load an RSS feed. You might need a CORS proxy if necessary.
  function loadFeed(selector, feedUrl) {
    $(selector).rss(feedUrl, {
      // If your feed URL has CORS issues, prepend a proxy URL:
      // feedUrl: "https://api.allorigins.hexocode.repl.co/get?disableCache=true&url=" + encodeURIComponent(feedUrl),
      limit: 5,
      layoutTemplate: "<ul>{entries}</ul>",
      entryTemplate: '<li><a href="{url}" target="_blank">{title}</a></li>',
      // Optionally, customize how the feed is rendered.
    });
  }

  // Replace the feed URLs below with your desired RSS feed URLs.
  loadFeed("#feed1", "https://rss.nytimes.com/services/xml/rss/nyt/Technology.xml");
  loadFeed("#feed2", "https://feeds.bbci.co.uk/news/technology/rss.xml");
  loadFeed("#feed3", "https://www.wired.com/feed/category/gear/latest/rss");
});
</script>
