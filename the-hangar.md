---
layout: default
title: The Hangar
---

<h2 style="text-align: center; font-size: 3rem; color: #5D3FD3; text-shadow: 3px 3px 8px rgba(93, 63, 211, 0.8);">
    The Hangar
</h2>

<!-- 🎥 YouTube Widget -->
<div style="display: flex; justify-content: center; align-items: center; flex-direction: column; margin-bottom: 2rem;">
    <h3 style="font-size: 2rem; text-align: center; color: #20B2AA; text-shadow: 1px 1px 5px rgba(32, 178, 170, 0.8);">
        Wind & Wireless
    </h3>
    <script type="text/javascript" src="https://feed.mikle.com/js/fw-loader.js" 
        preloader-text="Loading" 
        data-fw-param="171544/">
    </script>
</div>

<!-- 📝 Wispers on the Wind: Blog Section -->
<div style="background: rgba(32, 178, 170, 0.15); padding: 1.5rem; border-radius: 12px; box-shadow: 0px 0px 12px rgba(93, 63, 211, 0.4); margin-bottom: 2rem;">
    <h3 style="font-family: cursive; font-size: 2.5rem; color: #5D3FD3; text-align: center; text-shadow: 2px 2px 6px rgba(93, 63, 211, 0.6);">
        Wispers on the Wind ✈️
    </h3>
    <p style="text-align: center;">
        <a href="https://medium.com/@ekwedar/wind-wireless-what-happens-when-you-remove-restrictions-you-soar-4f27f8a516f0" 
   style="color: #5D3FD3; font-weight: bold; text-decoration: underline;">
            Wind & Wireless – What Happens When You Remove Restrictions? You Soar.
        </a>
    </p>
    <p style="text-align: center; color: #ddd;">
        ✍️ <strong>Read the full story on Medium!</strong> Click the link above to see how I built Wind & Wireless using open-source and free tools!
    </p>
</div>

<!-- ✈️ Word from the Tower: RSS-powered Aviation News -->
<div style="background: rgba(93, 63, 211, 0.15); padding: 1.5rem; border-radius: 12px; box-shadow: 0px 0px 12px rgba(32, 178, 170, 0.4);">
    <h3 style="font-size: 2rem; text-align: center; color: #20B2AA; text-shadow: 2px 2px 5px rgba(32, 178, 170, 0.8);">
        Word from the Tower
    </h3>
    <div id="rss-feed" style="padding: 1rem; text-align: center;">
        <p style="color: #ddd;">Loading latest aviation news...</p>
    </div>
</div>

<script>
async function fetchRSS() {
    const rssFeedUrl = "https://theaviationist.com/feeds/";

    try {
        const response = await fetch(`https://corsproxy.io/?${encodeURIComponent(rssFeedUrl)}`);
        const data = await response.text();
        const parser = new DOMParser();
        const xmlDoc = parser.parseFromString(data, "text/xml");

        const items = xmlDoc.querySelectorAll("item");
        let latestArticles = [];

        items.forEach((item, index) => {
            if (index < 10) {
                latestArticles.push({
                    title: item.querySelector("title").textContent,
                    link: item.querySelector("link").textContent,
                    date: item.querySelector("pubDate").textContent
                });
            }
        });

        localStorage.setItem("cachedArticles", JSON.stringify(latestArticles));
        localStorage.setItem("lastUpdate", new Date().toISOString());

        displayArticles(latestArticles.slice(0, 3));

    } catch (error) {
        console.error("Error fetching RSS:", error);
        document.getElementById("rss-feed").innerHTML = "<p style='color: red;'>Failed to load articles.</p>";
    }
}

function displayArticles(articles) {
    const feedContainer = document.getElementById("rss-feed");
    feedContainer.innerHTML = "";

    articles.forEach(article => {
        const articleElement = document.createElement("div");
        articleElement.innerHTML = `
            <p style="padding: 10px; background: rgba(32, 178, 170, 0.2); border-radius: 5px; margin-bottom: 10px;">
                <strong>
                    <a href="${article.link}" target="_blank" style="color: #5D3FD3; text-decoration: none;">
                        ${article.title}
                    </a>
                </strong>
                <br>
                <small style="color: #ddd;">${new Date(article.date).toLocaleDateString()}</small>
            </p>
        `;
        feedContainer.appendChild(articleElement);
    });
}

// ✅ Logic for Cached Data and Refresh
const lastUpdate = localStorage.getItem("lastUpdate");
const cachedArticles = localStorage.getItem("cachedArticles");

if (cachedArticles && lastUpdate) {
    const lastUpdateDate = new Date(lastUpdate);
    const now = new Date();

    if (now.toDateString() === lastUpdateDate.toDateString()) {
        displayArticles(JSON.parse(cachedArticles).slice(0, 3));
    } else {
        fetchRSS();
    }
} else {
    fetchRSS();
}
</script>

