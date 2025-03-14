---
layout: default
title: The Hangar
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Hangar</title>
    <style>
        body {
            background: #1a1a2e;
            color: #5D3FD3;
            font-family: Arial, sans-serif;
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
            text-align: center;
        }

        .news-item {
            padding: 10px;
            background: rgba(32, 178, 170, 0.2);
            border-radius: 5px;
            margin-bottom: 10px;
        }
    </style>
</head>

<body>

    <h2>The Hangar</h2>

    <!-- 🎥 YouTube Widget -->
    <div class="section">
        <h3 style="text-align: center;">Wind & Wireless</h3>
        <script type="text/javascript" src="https://feed.mikle.com/js/fw-loader.js" preloader-text="Loading" data-fw-param="171544/"></script>
    </div>

    <!-- 📝 Blog Section -->
    <div class="section">
        <h3>Wispers on the Wind ✈️</h3>
        <p><a href="https://medium.com/@ekwedar/wind-wireless-what-happens-when-you-remove-restrictions-you-soar-4f27f8a516f0" 
           style="color: #5D3FD3; font-weight: bold; text-decoration: underline;">
            Wind & Wireless – What Happens When You Remove Restrictions? You Soar.
        </a></p>
        <p>✍️ <strong>Read the full story on Medium!</strong></p>
    </div>

    <!-- ✈️ Word from the Tower: RSS-powered Aviation News -->
    <div class="section">
        <h3>Word from the Tower ✈️</h3>
        <div id="aviation-rss">
            <p>Loading latest aviation news...</p>
        </div>
    </div>

    <script>
    async function fetchAviationRSS() {
        const rssFeedUrl = "https://theaviationist.com/feeds/";

        try {
            console.log("Fetching RSS from:", rssFeedUrl);
            const response = await fetch(`https://corsproxy.io/?${encodeURIComponent(rssFeedUrl)}`);
            const data = await response.text();
            const parser = new DOMParser();
            const xmlDoc = parser.parseFromString(data, "text/xml");

            const items = xmlDoc.querySelectorAll("item");
            let latestArticles = [];

            items.forEach((item, index) => {
                if (index < 5) { 
                    latestArticles.push({
                        title: item.querySelector("title").textContent,
                        link: item.querySelector("link").textContent,
                        date: item.querySelector("pubDate").textContent
                    });
                }
            });

            localStorage.setItem("cachedAviationArticles", JSON.stringify(latestArticles));
            localStorage.setItem("lastAviationUpdate", new Date().toISOString());

            displayAviationArticles(latestArticles);

        } catch (error) {
            console.error("Error fetching RSS:", error);
            document.getElementById("aviation-rss").innerHTML = "<p style='color: red;'>Failed to load articles.</p>";
        }
    }

    function displayAviationArticles(articles) {
        const feedContainer = document.getElementById("aviation-rss");
        feedContainer.innerHTML = "";

        articles.forEach(article => {
            const articleElement = document.createElement("div");
            articleElement.classList.add("news-item");
            articleElement.innerHTML = `
                <p>
                    <strong><a href="${article.link}" target="_blank" style="color: #5D3FD3;">${article.title}</a></strong>
                    <br>
                    <small style="color: #ddd;">${new Date(article.date).toLocaleDateString()}</small>
                </p>
            `;
            feedContainer.appendChild(articleElement);
        });

        console.log("✅ Aviation news updated!");
    }

    // ✅ Check Cache & Refresh if Needed
    const lastAviationUpdate = localStorage.getItem("lastAviationUpdate");
    const cachedAviationArticles = localStorage.getItem("cachedAviationArticles");

    if (cachedAviationArticles && lastAviationUpdate) {
        const lastUpdateDate = new Date(lastAviationUpdate);
        const now = new Date();

        if (now.toDateString() === lastUpdateDate.toDateString()) {
            displayAviationArticles(JSON.parse(cachedAviationArticles));
        } else {
            fetchAviationRSS();
        }
    } else {
        fetchAviationRSS();
    }
    </script>

</body>
</html>
