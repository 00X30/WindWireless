---
layout: default
title: The Hangar
---
<h2>The Hangar</h2>
---
layout: default
title: The Hangar
---
<h2>The Hangar</h2>

<div id="feed1"></div>
<div id="feed2"></div>
<div id="feed3"></div>

<script>
  async function loadFeed(url, elementId) {
    try {
      // The "rss2json" service automatically converts an RSS URL into JSON
      const response = await fetch(`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(url)}`);
      const data = await response.json();
      
      let html = '';
      data.items.forEach(item => {
        html += `<p><a href="${item.link}">${item.title}</a></p>`;
      });

      document.getElementById(elementId).innerHTML = html;
    } catch (error) {
      console.error(error);
      document.getElementById(elementId).innerHTML = 'Failed to load feed';
    }
  }

  // Call your function 3 times for 3 feeds
  loadFeed('https://example.com/feed1.rss', 'feed1');
  loadFeed('https://example.com/feed2.rss', 'feed2');
  loadFeed('https://example.com/feed3.rss', 'feed3');
</script>
