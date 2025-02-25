---
layout: default
title: The Hangar
---
<script>
  async function loadFeed(url, elementId) {
    try {
      const response = await fetch(`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(url)}`);
      const data = await response.json();

      let html = '';
      data.items.forEach(item => {
        html += `<p><a href="${item.link}">${item.title}</a></p>`;
      });
      document.getElementById(elementId).innerHTML = html;
    } catch (error) {
      document.getElementById(elementId).innerHTML = 'Failed to load feed';
    }
  }

  // Replace with NASA Earthdata RSS feed URL
  const nasaEarthdataFeedURL = 'https://www.earthdata.nasa.gov/rss.xml'; 
  loadFeed(nasaEarthdataFeedURL, 'feedContainer');
</script>
