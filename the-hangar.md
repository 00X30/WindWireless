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
    console.log("The Hangar feed script is running!");

    // Use a proxy to avoid CORS issues:
    const proxy = "https://api.allorigins.hexocode.repl.co/get?disableCache=true&url=";

    // Example feeds:
    $("#feed1").rss(proxy + encodeURIComponent("https://rss.nytimes.com/services/xml/rss/nyt/Technology.xml"), {
      limit: 3
    });
$("#feed1").rss(proxy + encodeURIComponent("https://rss.nytimes.com/services/xml/rss/nyt/Technology.xml"), {
  limit: 3
});

    $("#feed2").rss(proxy + encodeURIComponent("https://feeds.bbci.co.uk/news/technology/rss.xml"), {
      limit: 3
    });

    $("#feed3").rss(proxy + encodeURIComponent("https://www.wired.com/feed/category/gear/latest/rss"), {
      limit: 3
    });
  });
</script>
