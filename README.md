<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Kaun Banega Crorepati</title>
  <style>
    body {
  font-family: Arial, sans-serif;
  background-color: #0f3057;
  color: #ffffff;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

.container {
  text-align: center;
  max-width: 600px;
  padding: 20px;
  background-color: #00587a;
  border-radius: 10px;
}

h1 {
  margin-bottom: 20px;
}

#question {
  font-size: 1.2em;
  margin-bottom: 20px;
}

.options {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}

.option-btn {
  padding: 10px;
  background-color: #00adb5;
  border: none;
  border-radius: 5px;
  color: white;
  font-weight: bold;
  cursor: pointer;
}

.option-btn:disabled {
  background-color: #444;
  cursor: not-allowed;
}

.lifeline-box {
  margin-top: 20px;
}

#result {
  margin-top: 20px;
  font-size: 1.1em;
}

#prize {
  margin-bottom: 10px;
  font-size: 1.1em;
  font-weight: bold;
}

  </style>
</head>
<body>
  <div class="container">
    <h1>🪙 Kaun Banega Crorepati 🪙</h1>
    <div id="prize">Prize: ₹0</div>
    <div id="question-box">
      <h2 id="question">Loading...</h2>
      <div class="options">
        <button class="option-btn" onclick="selectAnswer(0)">A</button>
        <button class="option-btn" onclick="selectAnswer(1)">B</button>
        <button class="option-btn" onclick="selectAnswer(2)">C</button>
        <button class="option-btn" onclick="selectAnswer(3)">D</button>
      </div>
    </div>
    <div class="lifeline-box">
      <button id="lifeline-btn" onclick="useLifeline()">Use 50:50 Lifeline</button>
      <button onclick="quitGame()">Quit</button>
    </div>
    <div id="result"></div>
  </div>

  <script>
   const levels = [
  {
    level: 1,
    questions: [
      {
        question: "Which planet is known as the Red Planet?",
        options: ["Earth", "Venus", "Mars", "Jupiter"],
        answer: 2,
      },
      {
        question: "Who wrote the Indian National Anthem?",
        options: ["Bankim Chandra Chatterjee", "Rabindranath Tagore", "Subhash Chandra Bose", "Jawaharlal Nehru"],
        answer: 1,
      },
    ]
  },
  {
    level: 2,
    questions: [
      {
        question: "What is the capital of Australia?",
        options: ["Sydney", "Melbourne", "Perth", "Canberra"],
        answer: 3,
      },
      {
        question: "Which is the largest ocean?",
        options: ["Atlantic", "Indian", "Pacific", "Arctic"],
        answer: 2,
      },
    ]
  },
  {
    level: 3,
    questions: [
      {
        question: "What is the speed of light?",
        options: ["299 792 458 m / s", "299 612 595 m / s", "299 911 458 m / s", "297 792 696 m / s"],
        answer: 0,
      },
      {
        question: "Strongest man in the world?",
        options: ["Tarun Gill", "Tom Stoltman", "Dwayne Johnson", "Nick Best"],
        answer: 1,
      },
    ]
  },
];

const prizeLevels = [1000, 5000, 10000, 50000, 100000, 250000, 500000, 1000000, 2500000, 5000000, 10000000];
let currentQuestion = 0;
let currentPrize = 0;
let lifelineUsed = false;

const questionEl = document.getElementById("question");
const prizeEl = document.getElementById("prize");
const resultEl = document.getElementById("result");
const optionBtns = document.querySelectorAll(".option-btn");
const lifelineBtn = document.getElementById("lifeline-btn");

let currentLevel = 0;
let currentQuestion = 0;
let currentPrize = 0;
const prizePerLevel = [5000, 20000, 100000]; // You can expand this
let lifelineUsed = false;

function loadQuestion() {
  const levelData = levels[currentLevel];
  const q = levelData.questions[currentQuestion];
  questionEl.textContent = `Level ${levelData.level}: ${q.question}`;
  optionBtns.forEach((btn, index) => {
    btn.textContent = `${String.fromCharCode(65 + index)}. ${q.options[index]}`;
    btn.disabled = false;
    btn.style.visibility = "visible";
  });
  resultEl.textContent = "";
  prizeEl.textContent = `Prize: ₹${currentPrize}`;
}

function selectAnswer(index) {
  const correct = questions[currentQuestion].answer;
  if (index === correct) {
    currentPrize = prizeLevels[currentQuestion];
    resultEl.textContent = `✅ Correct! You won ₹${currentPrize}`;
    currentQuestion++;
    if (currentQuestion < questions.length) {
      setTimeout(loadQuestion, 1500);
    } else {
      resultEl.textContent = `🏆 Congratulations! You are a Crorepati! Total: ₹${currentPrize}`;
      disableAllOptions();
    }
  } else {
    resultEl.textContent = `❌ Wrong! Correct answer was: ${questions[currentQuestion].options[correct]}`;
    disableAllOptions();
  }
}

function disableAllOptions() {
  optionBtns.forEach((btn) => (btn.disabled = true));
}

function useLifeline() {
  if (lifelineUsed) return;
  lifelineUsed = true;
  lifelineBtn.disabled = true;

  const correct = questions[currentQuestion].answer;
  const hideCount = 2;
  let hidden = 0;
  while (hidden < hideCount) {
    const rand = Math.floor(Math.random() * 4);
    if (rand !== correct && optionBtns[rand].style.visibility !== "hidden") {
      optionBtns[rand].style.visibility = "hidden";
      hidden++;
    }
  }
}

function quitGame() {
  resultEl.textContent = `🏁 You quit! Total winnings: ₹${currentPrize}`;
  disableAllOptions();
  lifelineBtn.disabled = true;
}

loadQuestion();

  </script>
</body>
</html>
