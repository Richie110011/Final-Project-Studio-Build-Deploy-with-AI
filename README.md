<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mini Music App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #121212;
      color: #fff;
      margin: 0;
      padding: 20px;
      text-align: center;
    }
    h1 {
      margin-bottom: 20px;
    }
    #searchBar {
      padding: 10px;
      width: 300px;
      border-radius: 8px;
      border: none;
      outline: none;
    }
    #results {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 20px;
      margin-top: 30px;
    }
    .card {
      background: #1e1e1e;
      border-radius: 12px;
      padding: 15px;
      box-shadow: 0 4px 6px rgba(0,0,0,0.5);
    }
    .card img {
      width: 100%;
      border-radius: 8px;
    }
    .card h3 {
      margin: 10px 0 5px;
      font-size: 16px;
    }
    .card p {
      font-size: 14px;
      color: #aaa;
    }
    .card button {
      margin-top: 10px;
      padding: 8px 12px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      background: #ff3d6e;
      color: #fff;
    }
  </style>
</head>
<body>
  <h1>🎶 Mini Music App</h1>
  <input type="text" id="searchBar" placeholder="Search for a song or artist...">
  <div id="results"></div>

  <script>
    const searchBar = document.getElementById("searchBar");
    const results = document.getElementById("results");
    let audio = new Audio();

    searchBar.addEventListener("keyup", function(e) {
      if (e.key === "Enter") {
        fetchMusic(searchBar.value);
      }
    });

    async function fetchMusic(query) {
      results.innerHTML = "<p>Loading...</p>";
      const res = await fetch(`https://cors-anywhere.herokuapp.com/https://api.deezer.com/search?q=${query}`);
      const data = await res.json();
      displayResults(data.data);
    }

    function displayResults(tracks) {
      results.innerHTML = "";
      tracks.forEach(track => {
        const card = document.createElement("div");
        card.classList.add("card");
        card.innerHTML = `
          <img src="${track.album.cover_medium}" alt="${track.title}">
          <h3>${track.title}</h3>
          <p>${track.artist.name}</p>
          <button onclick="playPreview('${track.preview}')">▶ Play</button>
        `;
        results.appendChild(card);
      });
    }

    function playPreview(previewUrl) {
      if (!previewUrl) return alert("No preview available.");
      audio.pause();
      audio = new Audio(previewUrl);
      audio.play();
    }
  </script>
</body>
</html>
