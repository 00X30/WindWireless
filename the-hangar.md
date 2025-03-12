---
layout: default
title: The Hangar
---

<h2>The Hangar</h2>
<!-- Word from the Tower -->
<p><strong>Word from the Tower.</strong></p>

<div id="rss-feed">
    <p>Loading latest aviation news...</p>
</div>
<script>
<div id="rss-feed">
    <p>Loading latest aviation news...</p>
</div>

<script>
async function fetchRSS() {
    const rssFeedUrl = "https://theaviationist.com/feeds/";
    
    try {
        // Fetch the RSS feed and parse it
        const response = await fetch(`https://api.allorigins.win/get?url=${encodeURIComponent(rssFeedUrl)}`);
        const data = await response.json();
        const parser = new DOMParser();
        const xmlDoc = parser.parseFromString(data.contents, "text/xml");

        // Extract items (articles)
        const items = xmlDoc.querySelectorAll("item");
        let latestArticles = [];
        
        items.forEach((item, index) => {
            if (index < 10) { // Store the latest 10 articles
                latestArticles.push({
                    title: item.querySelector("title").textContent,
                    link: item.querySelector("link").textContent,
                    date: item.querySelector("pubDate").textContent
                });
            }
        });

        // Store in Local Storage for daily caching
        localStorage.setItem("cachedArticles", JSON.stringify(latestArticles));
        localStorage.setItem("lastUpdate", new Date().toISOString());

        // Display the latest 3
        displayArticles(latestArticles.slice(0, 3));

    } catch (error) {
        console.error("Error fetching RSS:", error);
        document.getElementById("rss-feed").innerHTML = "<p>Failed to load articles.</p>";
    }
}

function displayArticles(articles) {
    const feedContainer = document.getElementById("rss-feed");
    feedContainer.innerHTML = ""; // Clear placeholder text

    articles.forEach(article => {
        const articleElement = document.createElement("div");
        articleElement.innerHTML = `
            <p>
                <strong><a href="${article.link}" target="_blank">${article.title}</a></strong> <br>
                <small>${new Date(article.date).toLocaleDateString()}</small>
            </p>
            <hr>
        `;
        feedContainer.appendChild(articleElement);
    });
}

// Check if cached data is available & fresh
const lastUpdate = localStorage.getItem("lastUpdate");
const cachedArticles = localStorage.getItem("cachedArticles");

if (cachedArticles && lastUpdate) {
    const lastUpdateDate = new Date(lastUpdate);
    const now = new Date();

    // Refresh the feed if it's a new day
    if (now.toDateString() === lastUpdateDate.toDateString()) {
        displayArticles(JSON.parse(cachedArticles).slice(0, 3));
    } else {
        fetchRSS();
    }
} else {
    fetchRSS();
}
</script>
