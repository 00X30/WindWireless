---
layout: default
title: The Hangar
---

<h1 style="text-align: left; color: #5D3FD3;">Aviation: Flight News & Trends</h1>

<!-- 🎥 YouTube Widget -->
<div>
    <h3>Wind & Wireless</h3>
    <script type="text/javascript" src="https://feed.mikle.com/js/fw-loader.js" 
        preloader-text="Loading" 
        data-fw-param="171544/">
    </script>
</div>

<!-- ✈️ Live Aviation Alerts -->
<div class="section">
    <h2 style="text-align: left;">✈️ Live Aviation Alerts</h2>
    <div id="aviation-carousel" class="carousel-container">
        <p style="text-align: left;">Loading latest aviation news...</p>
    </div>
</div>

<!-- 📜 Historic Aviation Milestones -->
<div class="section">
    <h2 style="text-align: left;">📜 Historic Aviation Milestones</h2>
    <p style="text-align: left;">1903: Wright Brothers’ first powered flight at Kitty Hawk.</p>
    <p style="text-align: left;">1927: Charles Lindbergh completes first solo Atlantic crossing.</p>
    <p style="text-align: left;">1981: Space Shuttle Columbia launches STS-1 mission.</p>
</div>

<!-- 🛫 Major Recent Aviation News -->
<div class="section">
    <h2 style="text-align: left;">🛫 Major Recent Aviation News</h2>
    <div class="news-item" style="text-align: left;">New supersonic jet concept aims to cut transatlantic time in half.</div>
    <div class="news-item" style="text-align: left;">FAA proposes new drone regulations for beyond-visual-line-of-sight flights.</div>
    <div class="news-item" style="text-align: left;">Airline orders 50 next-gen hybrid-electric planes.</div>
</div>

<!-- 🎥 Video Reports & Analysis -->
<div class="section">
    <h2 style="text-align: left;">🎥 Video Reports & Analysis</h2>
    <div class="video-list" style="text-align: left;">
        <div><a href="#">Electric VTOL test flights: Are we there yet?</a></div>
        <div><a href="#">Next-generation supersonic passenger jets</a></div>
        <div><a href="#">Air traffic control modernization: The next big leap</a></div>
    </div>
</div>

<!-- 🔮 Aviation Industry Forecasts -->
<div class="section">
    <h2 style="text-align: left;">🔮 Aviation Industry Forecasts</h2>
    <p style="text-align: left;">Global air traffic expected to double by 2035.</p>
    <p style="text-align: left;">Urban air mobility could be a $1.5 trillion market by 2040.</p>
    <p style="text-align: left;">Sustainable aviation fuels gain traction worldwide.</p>
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
    background: rgba(0, 0, 0, 0.6);
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

/* Styling for YouTube Mikle Plugin video items */
#youtube-mikle-container .video-item {
    flex: 0 0 auto;
    width: 300px;
    height: 169px;
    border-radius: 5px;
    overflow: hidden;
}
</style>

<script>
async function fetchAviationRSS() {
    const rssFeeds = [
        "https://www.aopa.org/news-and-media/news/rss",
        "https://www.flightglobal.com/feeds/allnews",
        "https://aviationweek.com/rss.xml",
        "https://www.faa.gov/newsroom/rss",
        "https://www.nasa.gov/rss/dyn/aeronautics.rss"
    ];

    let allArticles = [];

    for (const feedUrl of rssFeeds) {
        try {
            console.log(`Fetching: ${feedUrl}`);
            const response = await fetch(`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(feedUrl)}`);
            const data = await response.json();

            if (data.items) {
                data.items.slice(0, 2).forEach(item => {
                    allArticles.push({
                        title: item.title,
                        link: item.link,
                        source: new URL(feedUrl).hostname,
                        date: new Date(item.pubDate).getTime()
                    });
                });
            }
        } catch (error) {
            console.error(`Failed to fetch ${feedUrl}:`, error);
        }
    }

    allArticles = allArticles.sort((a, b) => b.date - a.date).slice(0, 5);

    displayAviationCarousel(allArticles);
}

function displayAviationCarousel(articles) {
    const carouselContainer = document.getElementById("aviation-carousel");
    carouselContainer.innerHTML = "";

    if (articles.length === 0) {
        carouselContainer.innerHTML = "<p style='color: red;'>No aviation alerts available.</p>";
        return;
    }

    articles.forEach((article, index) => {
        const slide = document.createElement("div");
        slide.classList.add("carousel-item");
        if (index === 0) slide.classList.add("active");

        slide.innerHTML = `
            <p class="news-item">
                <strong><a href="${article.link}" target="_blank" style="color: #5D3FD3;">
                    ${article.title}
                </a></strong><br>
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
    }, 5000);
}

document.addEventListener('DOMContentLoaded', function() {
    // Initialize Aviation RSS Fetch
    fetchAviationRSS();

    // Initialize YouTube Mikle Plugin for top videos
    if (typeof MikleYouTube !== 'undefined') {
        new MikleYouTube({
            container: '#youtube-mikle-container',
            videoCount: 5  // fetch 5 videos
            // additional config options can be added here as needed
        });
    } else {
        console.error("MikleYouTube plugin not loaded.");
    }
});
</script>
