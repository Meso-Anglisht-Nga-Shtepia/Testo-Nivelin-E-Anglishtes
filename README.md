<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ESL Placement Test</title>
<style>
  body {
    font-family: Arial, sans-serif;
    background: #f4f7fb;
    margin: 0;
    padding: 0;
  }
  .container {
    max-width: 800px;
    margin: auto;
    padding: 20px;
    background: #fff;
    border-radius: 12px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  }
  h1, h2, h3 {
    text-align: center;
  }
  .question {
    margin: 20px 0;
    font-size: 18px;
  }
  .options button {
    display: block;
    margin: 8px 0;
    padding: 12px;
    width: 100%;
    border: 1px solid #ddd;
    border-radius: 8px;
    background: #f9f9f9;
    cursor: pointer;
    transition: background 0.3s;
  }
  .options button:hover {
    background: #eef;
  }
  .correct {
    background: #c8f7c5 !important;
  }
  .wrong {
    background: #f7c5c5 !important;
  }
  #progressBar {
    width: 100%;
    background: #eee;
    border-radius: 10px;
    margin: 20px 0;
  }
  #progress {
    height: 20px;
    width: 0;
    background: #4caf50;
    border-radius: 10px;
    transition: width 0.3s;
  }
  .hidden { display: none; }
  .intro, .quiz, .result { text-align: center; }
  .skip-btn {
    margin-top: 10px;
    background: #ddd;
    border: none;
    padding: 10px 20px;
    border-radius: 8px;
    cursor: pointer;
  }
</style>
</head>
<body>
<div class="container">
  <div class="intro" id="intro">
    <h1>Welcome to the ESL Placement Test</h1>
    <p>This test has <strong>100 multiple-choice questions</strong>, starting from A1 level (beginner) to C2 level (advanced).<br>
       Each question has 5 possible answers. Select the correct one.<br>
       You can choose to <strong>skip a question</strong> if you are unsure, but you cannot go back.<br>
       Your English level will be shown at the end based on your score.<br>
       Click below to start.</p>
    <button onclick="startQuiz()">Start Test</button>
  </div>

  <div class="quiz hidden" id="quiz">
    <h2 id="questionNumber"></h2>
    <div class="question" id="question"></div>
    <div class="options" id="options"></div>
    <button class="skip-btn" onclick="skipQuestion()">Skip</button>
    <div id="progressBar"><div id="progress"></div></div>
  </div>

  <div class="result hidden" id="result">
    <h2>Test Completed</h2>
    <p id="scoreText"></p>
  </div>
</div>

<script>
const questions = [
  // ----------------- A1 -----------------
  {q: "1. What is your name?", options:["I am fine","My name is John","I am 20 years","Yes, it is","Good morning"], answer:1},
  {q: "2. How are you?", options:["I am fine","I am John","It is red","On the table","Yes, it does"], answer:0},
  {q: "3. Choose the correct sentence.", options:["She go to school","She goes to school","She going school","She go school","She to school"], answer:1},
  {q: "4. What is the plural of 'book'?", options:["Books","Bookes","Bookies","Book","Booken"], answer:0},
  {q: "5. Where ___ you from?", options:["is","am","are","be","was"], answer:2},
  // ... add up progressively to C2

  // ----------------- C2 -----------------
  {q: "100. Which sentence best demonstrates advanced academic style?", options:[
    "The results are good and show we did it right.",
    "The findings adequately substantiate the initial hypothesis.",
    "We found out that it works okay.",
    "It is nice and probably correct.",
    "The study was kind of useful."
  ], answer:1},
];

let currentQuestion = 0;
let score = 0;

function startQuiz(){
  document.getElementById('intro').classList.add('hidden');
  document.getElementById('quiz').classList.remove('hidden');
  showQuestion();
}

function showQuestion(){
  if(currentQuestion >= questions.length){
    endQuiz();
    return;
  }
  let q = questions[currentQuestion];
  document.getElementById('questionNumber').innerText = `Question ${currentQuestion+1} of ${questions.length}`;
  document.getElementById('question').innerText = q.q;
  let optionsHtml = '';
  q.options.forEach((opt,i)=>{
    optionsHtml += `<button onclick=\"selectAnswer(${i})\">${opt}</button>`;
  });
  document.getElementById('options').innerHTML = optionsHtml;
}

function selectAnswer(choice){
  let q = questions[currentQuestion];
  let buttons = document.querySelectorAll('#options button');
  if(choice === q.answer){
    buttons[choice].classList.add('correct');
    score++;
  } else {
    buttons[choice].classList.add('wrong');
    buttons[q.answer].classList.add('correct');
  }
  setTimeout(()=>{
    currentQuestion++;
    updateProgress();
    showQuestion();
  },1000);
}

function skipQuestion(){
  currentQuestion++;
  updateProgress();
  showQuestion();
}

function updateProgress(){
  let percent = ((currentQuestion) / questions.length) * 100;
  document.getElementById('progress').style.width = percent + '%';
}

function endQuiz(){
  document.getElementById('quiz').classList.add('hidden');
  document.getElementById('result').classList.remove('hidden');

  let level = '';
  if(score < 20) level = 'A1 (Beginner)';
  else if(score < 40) level = 'A2 (Elementary)';
  else if(score < 60) level = 'B1 (Intermediate)';
  else if(score < 75) level = 'B2 (Upper Intermediate)';
  else if(score < 90) level = 'C1 (Advanced)';
  else level = 'C2 (Proficient)';

  document.getElementById('scoreText').innerText = `Your level of English is: ${level}`;
}
</script>
</body>
</html>
