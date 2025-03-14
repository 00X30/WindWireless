---
layout: default
title: Wind - Weather & Climate Trends
---

<h1 style="text-align: left; color: #5D3FD3;">Wind: Weather & Climate Trends</h1>

<!-- 📡 Live Weather Alerts -->
<div class="section">
    <h2 style="text-align: left;">📡 Live Weather Alerts</h2>
    <div id="weather-carousel" class="carousel-container">
        <p style="text-align: left;">Loading latest weather news...</p>
    </div>
</div>

<!-- 📊 Historical Weather Trends -->
<div class="section">
    <h2 style="text-align: left;">📊 Historical Weather Trends</h2>
    <p style="text-align: left;">2006–2010: VA/NC saw unexpected winter storms.</p>
    <p style="text-align: left;">2013–2018: TX experiences massive hail events.</p>
    <p style="text-align: left;">2024: El Niño causes record snowpack in the West.</p>
</div>

<!-- 🌪️ Major Recent Weather Events -->
<div class="section">
    <h2 style="text-align: left;">🌪️ Major Recent Weather Events</h2>
    <div class="news-item" style="text-align: left;">Hurricane Lambda devastates Gulf Coast – $20B in damage.</div>
    <div class="news-item" style="text-align: left;">Super Tornado Outbreak – 150 tornadoes in 3 days!</div>
    <div class="news-item" style="text-align: left;">NYC Flooding – Record-breaking rainfall in Central Park.</div>
</div>

<!-- 🎥 Video Reports & Analysis -->
<div class="section">
    <h2 style="text-align: left;">🎥 Video Reports & Analysis</h2>
    <div class="video-list" style="text-align: left;">
        <div><a href="#">Tracking shifting snowfall patterns (2005–2025)</a></div>
        <div><a href="#">How tornado alley is moving east</a></div>
        <div><a href="#">What’s up with El Niño in 2024?</a></div>
    </div>
</div>

<!-- 🔮 Climate Shift Projections -->
<div class="section">
    <h2 style="text-align: left;">🔮 Climate Shift Projections</h2>
    <p style="text-align: left;">Model predicts Gulf hurricanes will be 15% stronger by 2035.</p>
    <p style="text-align: left;">Antarctica’s ice melt is 2x faster than expected.</p>
    <p style="text-align: left;">Jet stream disruptions: What we know so far.</p>
</div>

<style>
.carousel-container {
    position: relative;
    width: 100%;
    height: auto;
    overflow: hidden;
    text-align: left;
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

.section {
    background: rgba(0, 0, 0, 0.6); /* Increased contrast for readability */
    padding: 20px;
    margin-bottom: 20px;
    border-radius: 8px;
    box-shadow: 0px 0px 10px rgba(0, 255, 255, 0.2);
}

h2 {
    color: #20B2AA;
    text-shadow: 2px 2px 5px rgba(32, 178, 170, 0.8);
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
        "https://www.nhc.noaa.gov/index-at.xml", // NOAA National Hurricane Center Atlantic
        "https://www.nhc.noaa.gov/index-ep.xml", // NOAA National Hurricane Center Eastern Pacific
        "https://www.weather.gov/rss/warning/USZ076.xml", // NOAA Severe Weather Warnings
        "https://www.tsunami.gov/feeds.xml", // NOAA Tsunami Warnings
        "https://earthobservatory.nasa.gov/subscribe/feeds", // NASA Earth Observatory
    ];

    let allArticles = [];

    for (const feedUrl of rssFeeds) {
        try {
            const response = await fetch(`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(feedUrl)}`);
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

//
::contentReference[oaicite:0]{index=0}
<script>
async function fetchWeatherRSS() {
    const rssFeeds = [
        "https://www.nhc.noaa.gov/index-at.xml", // NOAA National Hurricane Center Atlantic
        "https://www.nhc.noaa.gov/index-ep.xml", // NOAA National Hurricane Center Eastern Pacific
        "https://www.weather.gov/rss/warning/USZ076.xml", // NOAA Severe Weather Warnings
        "https://www.tsunami.gov/feeds.xml", // NOAA Tsunami Warnings
        "https://earthobservatory.nasa.gov/subscribe/feeds" // NASA Earth Observatory
    ];

    let allArticles = [];

    for (const feedUrl of rssFeeds) {
        try {
            console.log(`Fetching: ${feedUrl}`); // Debugging
            const response = await fetch(`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(feedUrl)}`);
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

    displayWeatherAlerts(allArticles);
}

function displayWeatherAlerts(articles) {
    const alertContainer = document.getElementById("weather-carousel");
    alertContainer.innerHTML = ""; // Clear previous content

    if (articles.length === 0) {
        alertContainer.innerHTML = "<p style='color: red;'>No weather alerts available.</p>";
        return;
    }

    articles.forEach(article => {
        const alertItem = document.createElement("p");
        alertItem.classList.add("news-item");
        alertItem.innerHTML = `
            <strong><a href="${article.link}" target="_blank" style="color: #5D3FD3;">
                ${article.title}
            </a></strong> <br>
            <small style="color: #ccc;">${article.source}</small>
        `;
        alertContainer.appendChild(alertItem);
    });

    console.log("✅ Weather alerts updated!");
}

// Run RSS Fetching
fetchWeatherRSS();
</script>

</body>
</html> 
