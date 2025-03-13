---
layout: default
title: Wind - Weather & Climate Trends
---

<h1 style="text-align: center; color: #5D3FD3;">Wind: Weather & Climate Trends</h1>

<!-- 📡 Live Weather Alerts -->
<div class="section">
    <h2>📡 Live Weather Alerts</h2>
    <div id="weather-feed">
        <p>Loading latest weather news...</p>
    </div>
</div>

<!-- 📊 Historical Weather Trends -->
<div class="section">
    <h2>📊 Historical Weather Trends</h2>
    <p>2006–2010: VA/NC saw unexpected winter storms.</p>
    <p>2013–2018: TX experiences massive hail events.</p>
    <p>2024: El Niño causes record snowpack in the West.</p>
</div>

<!-- 🌪️ Major Recent Weather Events -->
<div class="section">
    <h2>🌪️ Major Recent Weather Events</h2>
    <div class="news-item">Hurricane Lambda devastates Gulf Coast – $20B in damage.</div>
    <div class="news-item">Super Tornado Outbreak – 150 tornadoes in 3 days!</div>
    <div class="news-item">NYC Flooding – Record-breaking rainfall in Central Park.</div>
</div>

<!-- 🎥 Video Reports & Analysis -->
<div class="section">
    <h2>🎥 Video Reports & Analysis</h2>
    <div class="video-list">
        <a href="#">Tracking shifting snowfall patterns (2005–2025)</a>
        <a href="#">How tornado alley is moving east</a>
        <a href="#">What’s up with El Niño in 2024?</a>
    </div>
</div>

<!-- 🔮 Climate Shift Projections -->
<div class="section">
    <h2>🔮 Climate Shift Projections</h2>
    <p>Model predicts Gulf hurricanes will be 15% stronger by 2035.</p>
    <p>Antarctica’s ice melt is 2x faster than expected.</p>
    <p>Jet stream disruptions: What we know so far.</p>
</div>

<style>
.section {
    background: rgba(255, 255, 255, 0.1);
    padding: 20px;
    margin-bottom: 20px;
    border-radius: 8px;
    box-shadow: 0px 0px 10px rgba(0, 255, 255, 0.2);
}
h2 {
    color: #20B2AA;
    text-shadow: 2px 2px 5px rgba(32, 178, 170, 0.8);
}
.news-item {
    margin-bottom: 10px;
    padding: 10px;
    background: rgba(255, 255, 255, 0.2);
    border-radius: 5px;
}
.video-list a {
    color: #5D3FD3;
    text-decoration: none;
    display: block;
    margin-bottom: 10px;
}
.video-list a:hover {
    color: #20B2AA;
}
</style>

<script>
async function fetchWeatherRSS() {
    const rssFeeds = [
        "https://alerts.weather.gov/cap/us.php?x=1", // NOAA
        "https://earthobservatory.nasa.gov/rss/eo.rss", // NASA
        "https://feeds.bbci.co.uk/news/science_and_environment/rss.xml", // BBC
        "https://www.aljazeera.com/xml/rss/all.xml", // Al Jazeera
        "https://www.eumetsat.int/rss.xml" // EUMETSAT
    ];

    let allArticles = [];

    for (const feedUrl of rssFeeds) {
        try {
            const response = await fetch(`https://corsproxy.io/?${encodeURIComponent(feedUrl)}`);
            const data = await response.text();
            const parser = new DOMParser();
            const xmlDoc = parser.parseFromString(data, "text/xml");

            const items = xmlDoc.querySelectorAll("item");
            items.forEach((item, index) => {
                if (index < 2) { // Get only top 2 from each feed (total 5 max)
                    allArticles.push({
                        title: item.querySelector("title").textContent,
                        link: item.querySelector("link").textContent,
                        source: new URL(feedUrl).hostname
                    });
                }
            });

        } catch (error) {
            console.error(`Failed to fetch ${feedUrl}:`, error);
        }
    }

    // Sort articles by latest if we can extract dates later
    allArticles = allArticles.slice(0, 5); // Keep only latest 5

    displayWeatherAlerts(allArticles);
}

function displayWeatherAlerts(articles) {
    const feedContainer = document.getElementById("weather-feed");
    feedContainer.innerHTML = "";

    if (articles.length === 0) {
        feedContainer.innerHTML = "<p style='color: red;'>No weather alerts available.</p>";
        return;
    }

    articles.forEach(article => {
        const articleElement = document.createElement("div");
        articleElement.innerHTML = `
            <p class="news-item">
                <strong><a href="${article.link}" target="_blank" style="color: #5D3FD3;">
                    ${article.title}
                </a></strong> <br>
                <small style="color: #ccc;">${article.source}</small>
            </p>
        `;
        feedContainer.appendChild(articleElement);
    });
}

// Run the fetch function
fetchWeatherRSS();
</script>
