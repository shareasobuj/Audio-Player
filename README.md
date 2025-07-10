# Audio-Player

<html lang="bn">
<head>
  <meta charset="UTF-8">
  <title>অডিও শেয়ারিং ওয়েবসাইট 🎧</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: fuchsia;
      margin: 0;
      padding: 0;
    }
    header {
      background: #4CAF50;
      color: white;
      text-align: center;
      padding: 20px;
      font-size: 28px;
    }
    .upload, .search {
      background: white;
      padding: 20px;
      margin: 10px;
      border-radius: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    input, button {
      width: 100%;
      padding: 10px;
      margin-top: 10px;
      font-size: 16px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button {
      background: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background: #45a049;
    }
    .audio-list {
      display: flex;
      flex-wrap: wrap;
      gap: 15px;
      padding: 20px;
    }
    .audio-card {
      background: white;
      padding: 15px;
      border-radius: 10px;
      width: 300px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    audio {
      width: 100%;
      margin-top: 10px;
    }
  </style>
</head>
<body>

  <header>🎵 আমার অডিও ওয়েবসাইট</header>

  <div class="upload">
    <h3>🎙️ অডিও আপলোড করুন</h3>
    <input type="text" id="audioTitle" placeholder="অডিওর শিরোনাম দিন">
    <input type="file" id="audioFile" accept="audio/*">
    <button onclick="uploadAudio()">আপলোড করুন</button>
  </div>

  <div class="search">
    <input type="text" id="searchInput" placeholder="🔍 অডিও খুঁজুন..." oninput="renderAudios()">
  </div>

  <div class="audio-list" id="audioContainer"></div>

  <script>
    let audios = JSON.parse(localStorage.getItem("myAudios")) || [];

    function uploadAudio() {
      const title = document.getElementById("audioTitle").value.trim();
      const file = document.getElementById("audioFile").files[0];

      if (!title || !file) {
        alert("অনুগ্রহ করে শিরোনাম ও অডিও ফাইল দিন!");
        return;
      }

      const reader = new FileReader();
      reader.onload = function(e) {
        audios.push({ title: title, src: e.target.result });
        localStorage.setItem("myAudios", JSON.stringify(audios));
        document.getElementById("audioTitle").value = "";
        document.getElementById("audioFile").value = "";
        renderAudios();
      };
      reader.readAsDataURL(file);
    }

    function renderAudios() {
      const container = document.getElementById("audioContainer");
      const search = document.getElementById("searchInput").value.toLowerCase();
      container.innerHTML = "";

      const filtered = audios.filter(a => a.title.toLowerCase().includes(search));
      if (filtered.length === 0) {
        container.innerHTML = "<p>⚠️ কোনো অডিও পাওয়া যায়নি!</p>";
        return;
      }

      filtered.forEach(audio => {
        const card = document.createElement("div");
        card.className = "audio-card";
        card.innerHTML = `
          <strong>${audio.title}</strong>
          <audio controls src="${audio.src}"></audio>
        `;
        container.appendChild(card);
      });
    }

    window.onload = renderAudios;
  </script>

</body>
</html>
