---
layout: default
title: The Hangar
---
<h2>The Hangar</h2>
<div id="feed1"></div>

<script>
  // Wrap in document.ready so jQuery is guaranteed to exist
  $(document).ready(function() {
    console.log("Hangar feed script is running!");

    // Just a minimal test: print the jQuery version
    console.log("jQuery version:", $.fn.jquery);

    // Optional: Show a test message in #feed1
    $("#feed1").text("If you see this text, jQuery worked!");
  });
</script>

