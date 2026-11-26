
<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>My Creations</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: Arial, sans-serif;
    background: #f4f6f8;
    color: #222;
}

header {
    background: #111827;
    color: white;
    padding: 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 15px;
}

nav a {
    color: white;
    text-decoration: none;
    margin: 8px;
}

.hero {
    text-align: center;
    padding: 85px 20px;
    background: white;
}

.hero h2 {
    font-size: 38px;
    margin-bottom: 15px;
}

.hero p {
    margin-bottom: 25px;
}

.heroButton {
    display: inline-block;
    padding: 12px 20px;
    background: #2563eb;
    color: white;
    border-radius: 8px;
    text-decoration: none;
}

section {
    max-width: 1100px;
    margin: auto;
    padding: 55px 20px;
}

section h2 {
    margin-bottom: 25px;
}

.cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
    gap: 20px;
}

.card {
    background: white;
    padding: 25px;
    border-radius: 12px;
    box-shadow: 0 3px 10px rgba(0,0,0,0.08);
}

.card h3 {
    margin-bottom: 15px;
}

.card p {
    margin-bottom: 12px;
}

button {
    border: none;
    border-radius: 7px;
    padding: 11px 16px;
    cursor: pointer;
    font-size: 15px;
    color: white;
}

.visitButton, .playButton {
    background: #2563eb;
}

.visitButton:hover, .playButton:hover {
    background: #1d4ed8;
}

.deleteButton {
    background: #dc2626;
    margin-left: 5px;
    margin-top: 10px;
}

.addButton {
    background: #16a34a;
    margin-top: 25px;
    padding: 13px 18px;
}

.free {
    color: #16a34a;
    font-weight: bold;
}

.paid {
    color: #dc2626;
    font-weight: bold;
}

.about {
    text-align: center;
    background: white;
    max-width: none;
}

footer {
    background: #111827;
    color: white;
    text-align: center;
    padding: 25px;
}

.modal {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.6);
    justify-content: center;
    align-items: center;
    z-index: 1000;
}

.modalBox {
    background: white;
    padding: 30px;
    border-radius: 12px;
    width: 90%;
    max-width: 400px;
}

.modalBox h2 {
    margin-bottom: 20px;
}

.modalBox input {
    width: 100%;
    padding: 12px;
    margin-bottom: 12px;
    border: 1px solid #ccc;
    border-radius: 6px;
}

.cancel {
    background: #666;
}

@media(max-width: 600px) {
    header {
        flex-direction: column;
    }

    .hero h2 {
        font-size: 30px;
    }
}
</style>
</head>

<body>

<header>
    <h1>My Creations 🚀</h1>

    <nav>
        <a href="#home">Home</a>
        <a href="#websites">My Websites</a>
        <a href="#games">My Games</a>
        <a href="#about">About</a>
    </nav>
</header>

<main>

<section id="home" class="hero">
    <h2>Welcome to My Creations</h2>
    <p>Explore my inventions, websites and games.</p>
    <a href="#websites" class="heroButton">Explore</a>
</section>

<section id="websites">
    <h2>My Websites 🌐</h2>
    <div id="websiteList" class="cards"></div>

    <button class="addButton" onclick="openWebsiteForm()">
        + Add My Website
    </button>
</section>

<section id="games">
    <h2>My Games 🎮</h2>
    <div id="gameList" class="cards"></div>
</section>

<section id="about" class="about">
    <h2>About</h2>
    <p>This is my personal collection of creations.</p>
</section>

</main>

<!-- ADD WEBSITE POPUP -->

<div id="websiteForm" class="modal">
    <div class="modalBox">

        <h2>Add My Website</h2>

        <input
            type="text"
            id="websiteName"
            placeholder="Website name"
        >

        <input
            type="text"
            id="websiteURL"
            placeholder="https://example.com"
        >

        <button onclick="addWebsite()">Add Website</button>

        <button class="cancel" onclick="closeWebsiteForm()">
            Cancel
        </button>

    </div>
</div>

<footer>
    <p>© 2026 My Creations</p>
</footer>

<script>

/* =================================
   LOAD SAVED INFORMATION
================================= */

function loadData(key, fallback) {
    try {
        const data = JSON.parse(localStorage.getItem(key));
        return data === null ? fallback : data;
    } catch {
        return fallback;
    }
}

let myWebsites = loadData("myWebsites", []);
let accessCounts = loadData("accessCounts", {});

if (!Array.isArray(myWebsites)) myWebsites = [];

if (
    !accessCounts ||
    typeof accessCounts !== "object" ||
    Array.isArray(accessCounts)
) {
    accessCounts = {};
}


/* =================================
   ADD YOUR GAMES HERE
================================= */

const myGames = [

    /*
    EXAMPLE:

    {
        id: "game1",
        name: "My Racing Game",
        url: "https://your-game-link.com"
    },

    */

];


/* =================================
   WEBSITE URL CHECK
================================= */

function getValidURL(value) {
    try {
        let url = value.trim();

        if (!/^https?:\/\//i.test(url)) {
            url = "https://" + url;
        }

        const parsed = new URL(url);

        if (
            parsed.protocol !== "https:" &&
            parsed.protocol !== "http:"
        ) {
            return null;
        }

        return parsed.href;
    } catch {
        return null;
    }
}


/* =================================
   SAVE VISIT COUNTS
================================= */

function saveCounts() {
    localStorage.setItem(
        "accessCounts",
        JSON.stringify(accessCounts)
    );
}


/* =================================
   GET VISITS FOR A LINK
================================= */

function getVisits(url) {
    return Math.max(0, Number(accessCounts[url]) || 0);
}


/* =================================
   OPEN WEBSITE FORM
================================= */

function openWebsiteForm() {
    document.getElementById("websiteForm").style.display = "flex";
}

function closeWebsiteForm() {
    document.getElementById("websiteForm").style.display = "none";
}


/* =================================
   ADD WEBSITE
================================= */

function addWebsite() {

    const name =
        document.getElementById("websiteName").value.trim();

    const enteredURL =
        document.getElementById("websiteURL").value.trim();

    if (!name || !enteredURL) {
        alert("Please enter the website name and URL.");
        return;
    }

    const url = getValidURL(enteredURL);

    if (!url) {
        alert("Please enter a valid website URL.");
        return;
    }

    myWebsites.push({
        name: name,
        url: url
    });

    localStorage.setItem(
        "myWebsites",
        JSON.stringify(myWebsites)
    );

    displayWebsites();

    document.getElementById("websiteName").value = "";
    document.getElementById("websiteURL").value = "";

    closeWebsiteForm();

    alert("Website added successfully! 🎉");
}


/* =================================
   COMMON OPEN SYSTEM

   First 2 opens = FREE
   After that = ₹2 required
================================= */

function openProtectedLink(url, itemName) {

    const validURL = getValidURL(url);

    if (!validURL) {
        alert("This link is invalid.");
        return;
    }

    const visits = getVisits(validURL);

    // FIRST AND SECOND VISIT ARE FREE

    if (visits < 2) {

        // Save the visit before leaving this page.
        accessCounts[validURL] = visits + 1;

        saveCounts();

        // Open in the SAME tab.
        window.location.assign(validURL);

        return;
    }

    // THIRD VISIT AND ONWARDS

    alert(
        "You have used your 2 free visits for " +
        itemName + "!\n\n" +
        "₹2 is required for another visit.\n\n" +
        "Real payment is not connected yet. " +
        "No money has been charged."
    );
}


/* =================================
   DISPLAY WEBSITES
================================= */

function displayWebsites() {

    const list = document.getElementById("websiteList");

    list.innerHTML = "";

    if (myWebsites.length === 0) {
        const message = document.createElement("p");
        message.textContent = "You haven't added any websites yet.";
        list.appendChild(message);
        return;
    }

    myWebsites.forEach(function(website, index) {

        const card = document.createElement("div");
        card.className = "card";

        const title = document.createElement("h3");
        title.textContent = "🌐 " + website.name;

        const visits = getVisits(website.url);
        const remaining = Math.max(0, 2 - visits);

        const status = document.createElement("p");

        if (remaining > 0) {
            status.className = "free";
            status.textContent = remaining + " free visit(s) left";
        } else {
            status.className = "paid";
            status.textContent = "₹2 required per visit";
        }

        const visitButton = document.createElement("button");

        visitButton.className = "visitButton";
        visitButton.textContent =
            remaining > 0 ? "Visit Website" : "₹2 to Visit";

        visitButton.onclick = function() {
            openProtectedLink(website.url, website.name);
        };

        const deleteButton = document.createElement("button");

        deleteButton.className = "deleteButton";
        deleteButton.textContent = "Delete";

        deleteButton.onclick = function() {
            deleteWebsite(index);
        };

        card.appendChild(title);
        card.appendChild(status);
        card.appendChild(visitButton);
        card.appendChild(deleteButton);

        list.appendChild(card);
    });
}


/* =================================
   DELETE WEBSITE
================================= */

function deleteWebsite(index) {

    if (confirm("Delete this website?")) {

        myWebsites.splice(index, 1);

        localStorage.setItem(
            "myWebsites",
            JSON.stringify(myWebsites)
        );

        displayWebsites();
    }
}


/* =================================
   DISPLAY GAMES
================================= */

function displayGames() {

    const list = document.getElementById("gameList");

    list.innerHTML = "";

    if (myGames.length === 0) {
        const message = document.createElement("p");
        message.textContent = "Your games will appear here when you add them.";
        list.appendChild(message);
        return;
    }

    myGames.forEach(function(game) {

        const card = document.createElement("div");
        card.className = "card";

        const title = document.createElement("h3");
        title.textContent = "🎮 " + game.name;

        const url = getValidURL(game.url);
        const visits = url ? getVisits(url) : 0;
        const remaining = Math.max(0, 2 - visits);

        const status = document.createElement("p");

        if (remaining > 0) {
            status.className = "free";
            status.textContent = remaining + " free play(s) left";
        } else {
            status.className = "paid";
            status.textContent = "₹2 required per play";
        }

        const playButton = document.createElement("button");

        playButton.className = "playButton";

        playButton.textContent =
            remaining > 0 ? "▶ Play Free" : "💰 Pay ₹2 to Play";

        playButton.onclick = function() {
            openProtectedLink(game.url, game.name);
        };

        card.appendChild(title);
        card.appendChild(status);
        card.appendChild(playButton);

        list.appendChild(card);
    });
}


/* =================================
   START WEBSITE
================================= */

displayWebsites();
displayGames();

</script>

</body>
</html>
