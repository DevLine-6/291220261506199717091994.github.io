<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>IA ou pas IA ?</title>
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    body {
      background: linear-gradient(135deg, #0f172a, #1e293b);
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      transition: background 0.3s;
    }

    .container {
      text-align: center;
      background: rgba(255,255,255,0.05);
      backdrop-filter: blur(10px);
      padding: 30px;
      border-radius: 20px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.3);
      max-width: 400px;
      width: 90%;
    }

    h1 {
      margin-bottom: 10px;
      font-size: 24px;
    }

    #score {
      margin-bottom: 20px;
      opacity: 0.8;
    }

    img {
      width: 100%;
      border-radius: 15px;
      margin-bottom: 20px;
      transition: transform 0.3s;
    }

    img:hover {
      transform: scale(1.02);
    }

    .buttons {
      display: flex;
      gap: 10px;
    }

    button {
      flex: 1;
      padding: 12px;
      border: none;
      border-radius: 10px;
      font-size: 16px;
      cursor: pointer;
      transition: 0.2s;
    }

    .ia {
      background: #22c55e;
      color: white;
    }

    .notia {
      background: #3b82f6;
      color: white;
    }

    button:hover {
      transform: translateY(-2px);
      opacity: 0.9;
    }

    .wrong {
      background: #7f1d1d !important;
    }

    .flash {
      animation: flashRed 0.3s;
    }

    @keyframes flashRed {
      0% { background: #7f1d1d; }
      100% { background: linear-gradient(135deg, #0f172a, #1e293b); }
    }
  </style>
</head>

<body>

<div class="container">
  <h1>IA ou pas IA ?</h1>
  <div id="score">Score : 0</div>
  <img id="image" src="">
  <div class="buttons">
    <button class="ia" onclick="answer(true)">IA</button>
    <button class="notia" onclick="answer(false)">Pas IA</button>
  </div>
</div>

<script>
const images = [
  {
    src: "https://wp.en.aleteia.org/wp-content/uploads/sites/2/2023/03/Pope-francis-artificial-intelligence-generated-image-white-coat.jpg?resize=620,350&q=75",
    isAI: true
  },
  {
    src: "https://file.aitubo.ai/assets/doc/2024/12/aitubo-54-.jpg!w1280",
    isAI: true
  },
  {
    src: "https://upload.chatsdumonde.com/img_global/25-cousins-du-chat/guepard/1285_13441-un-guepard-couche-sur-un-rocher.jpg",
    isAI: false
  },
  {
    src: "images/photo4.jpg",
    isAI: false
  }
];

let current = 0;
let score = 0;

const imageEl = document.getElementById("image");
const scoreEl = document.getElementById("score");
const container = document.querySelector(".container");

function loadImage() {
  imageEl.src = images[current].src;
}

function showEndScreen() {
  container.innerHTML = `
    <h1>🎉 Jeu terminé</h1>
    <p style="font-size:18px; margin-top:15px;">
      Prêts à être encore grands parents ?
    </p>
    <p style="margin-top:10px;">Score final : ${score} / ${images.length}</p>
  `;
}

function answer(choice) {
  const correct = images[current].isAI;

  if (choice === correct) {
    score++;
    scoreEl.innerText = "Score : " + score;

    confetti({
      particleCount: 100,
      spread: 70,
      origin: { y: 0.6 }
    });
  } else {
    document.body.classList.add("wrong");
    setTimeout(() => document.body.classList.remove("wrong"), 300);
  }

  current++;

  // 👉 FIN DU JEU
  if (current >= images.length) {
    setTimeout(showEndScreen, 600);
    return;
  }

  loadImage();
}

loadImage();
