---
layout: default
title: The Hangar
---

<h2>The Hangar</h2>

<!-- 🎥 YouTube Widget -->
<div>
    <h3>Wind & Wireless</h3>
    <script type="text/javascript" src="https://feed.mikle.com/js/fw-loader.js" 
        preloader-text="Loading" 
        data-fw-param="171544/">
    </script>
</div>

<!-- 📝 Wispers on the Wind: Blog Section -->
<h3><span style="font-family: cursive; font-size: 2.5rem;">Wispers on the Wind</span>: The Wind & Wireless Blog ✈️</h3>

<p>
    <a href="https://medium.com/@ekwedar/wind-wireless-what-happens-when-you-remove-restrictions-you-soar-4f27f8a516f0">
        Wind & Wireless – What Happens When You Remove Restrictions? You Soar.
    </a>  
</p>
<p>✍️ <strong>Read the full story on Medium!</strong> Click the link above to see how I built Wind & Wireless using open-source and free tools!</p>

<!-- ✈️ Word from the Tower: RSS-powered Aviation News -->
<h3>Word from the Tower</h3>
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
