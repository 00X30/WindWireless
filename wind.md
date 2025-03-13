---
layout: default
title: Wind - Weather & Climate Trends
---

<h1 style="text-align: center; color: #5D3FD3;">Wind: Weather & Climate Trends</h1>

<!-- 📡 Live Weather Alerts -->
<div class="section">
    <h2>📡 Live Weather Alerts</h2>
    <div id="weather-carousel" class="carousel-container">
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
.carousel-container {
    position: relative;
    width: 100%;
    height: auto;
    overflow: hidden;
    text-align: center;
}

.carousel-item {
    display: none;
    opacity: 0;
    transition: opacity 1s ease-in-out;
}

.carousel-item.active {
    display: block;
    opacity: 1;
}

.news-item {
    margin-bottom: 10px;
    padding: 10px;
    background: rgba(255, 255, 255, 0.2);
    border-radius: 5px;
}
</style>

<script>
async function fetchWeatherRSS() {
    const rssFeeds = [
        "https://earthobservatory.nasa.gov/rss/eo.rss", // NASA Earth Observatory
        "https://climate.nasa.gov/feed/", // NASA Climate
        "https://www.nasa.gov/rss/dyn/earth.rss", // NASA Earth Science
        "https://www.nhc.noaa.gov/index-at.xml", // NOAA National Hurricane Center
        "https://www.tsunami.gov/feeds.xml" // NOAA Tsunami Warnings
    ];

    let allArticles = [];

    for (const feedUrl of rssFeeds) {
        try {
            const response = await fetch(`https://rss2json.com/api.json?rss_url=${encodeURIComponent(feedUrl)}`);
            const data = await response.json();

            if (data.items) {
                data.items.slice(0, 2).forEach(item => {
                    allArticles.push({
                        title: item.title,
                        link: item.link,
                        source: new URL(feedUrl).hostname,
                        date: new Date(item.pubDate).getTime() // Sort by latest
                    });
                });
            }
        } catch (error) {
            console.error(`Failed to fetch ${feedUrl}:`, error);
        }
    }

    // Sort by date (latest first) and keep only top 5
    allArticles = allArticles.sort((a, b) => b.date - a.date).slice(0, 5);

    displayWeatherCarousel(allArticles);
}

function displayWeatherCarousel(articles) {
    const carouselContainer = document.getElementById("weather-carousel");
    carouselContainer.innerHTML = ""; // Clear previous entries

    if (articles.length === 0) {
        carouselContainer.innerHTML = "<p style='color: red;'>No weather alerts available.</p>";
        return;
    }

    articles.forEach((article, index) => {
        const slide = document.createElement("div");
        slide.classList.add("carousel-item");
        if (index === 0) slide.classList.add("active"); // First one is visible

        slide.innerHTML = `
            <p class="news-item">
                <strong><a href="${article.link}" target="_blank" style="color: #5D3FD3;">
                    ${article.title}
                </a></strong> <br>
                <small style="color: #ccc;">${article.source}</small>
            </p>
        `;
        carouselContainer.appendChild(slide);
    });

    startCarousel();
}

function startCarousel() {
    let index = 0;
    const slides = document.querySelectorAll(".carousel-item");
    if (slides.length === 0) return;

    setInterval(() => {
        slides.forEach(slide => slide.classList.remove("active"));
        slides[index].classList.add("active");
        index = (index + 1) % slides.length;
    }, 5000); // Rotate every 5 seconds
}

// Run the fetch function
fetchWeatherRSS();
</script>
