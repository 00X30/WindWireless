<!DOCTYPE html>
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
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
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
        }
    </style>
</head>
<body>
    <h2>The Hangar</h2>
   <!-- 🎥 YouTube Widget -->
    <div class="section">
        <h3 style="text-align: center; color: #20B2AA;">Wind & Wireless</h3>
        <script type="text/javascript" src="https://feed.mikle.com/js/fw-loader.js" 
            preloader-text="Loading" 
            data-fw-param="171544/"></script>
    </div>

    <!-- ✈️ Word from the Tower: RSS-powered Aviation News -->
    <div class="section">
        <h3>✈️ Word from the Tower</h3>
        <div id="rss-feed" class="news-item">Loading latest aviation news...</div>
    </div>

    <script>
    async function fetchRSS() {
        const rssFeedUrl = "https://theaviationist.com/feeds/";
        try {
            console.log("Fetching RSS from:", rssFeedUrl);
            const response = await fetch(rssFeedUrl);
            const data = await response.text();
            console.log("Fetched RSS Data:", data); // Debugging output
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
            displayArticles(latestArticles);
        } catch (error) {
            console.error("Error fetching RSS:", error);
            document.getElementById("rss-feed").innerHTML = "<p style='color: red;'>Failed to load articles.</p>";
        }
    }

    function displayArticles(articles) {
        const feedContainer = document.getElementById("rss-feed");
        feedContainer.innerHTML = "";
        if (articles.length === 0) {
            feedContainer.innerHTML = "<p style='color: red;'>No articles available.</p>";
            return;
        }
        articles.forEach(article => {
            const articleElement = document.createElement("div");
            articleElement.innerHTML = `
                <p class="news-item">
                    <strong><a href="${article.link}" target="_blank" style="color: #5D3FD3; text-decoration: none;">${article.title}</a></strong><br>
                    <small style="color: #ccc;">${new Date(article.date).toLocaleDateString()}</small>
                </p>
            `;
            feedContainer.appendChild(articleElement);
        });
    }
    fetchRSS();
    </script>
</body>
</html>

