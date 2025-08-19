<html lang="en">
<head>
<meta charset="UTF-8">
  <meta name="google-site-verification" content="j8wJCiLTFIvrqtx81YIfZVDvnhBW1qAJKeBwV2fFPUo" />
<title>ESL Placement Test</title>
<style>
  footer, .site-footer, .footer {
  display: none !important;
}
 body {
  font-family: Arial, sans-serif;
  background: url("image.jpg") no-repeat center center fixed;
  background-size: cover;
  color: #333;
  margin: 0;
  padding: 0;
}
  #container {
    max-width: 800px;
    margin: 40px auto;
    background: #fff;
    border-radius: 15px;
    box-shadow: 0 10px 20px rgba(0,0,0,0.2);
    overflow: hidden;
  }
  #info, #quiz-box, #result {
    padding: 30px;
    text-align: center;
  }
  h2 { margin-bottom: 20px; }
  .option {
    display: block;
    background: #f0f0f0;
    padding: 12px;
    margin: 10px 0;
    border-radius: 10px;
    cursor: pointer;
    transition: 0.3s;
  }
  .option:hover { background: #e0e0e0; }
  .correct { background: #4CAF50; color: #fff; }
  .wrong { background: #f44336; color: #fff; }
  #progress-container {
    width: 90%;
    background: #ddd;
    height: 20px;
    border-radius: 10px;
    margin: 20px auto;
  }
  #progress-bar {
    height: 100%;
    width: 0%;
    background: #4CAF50;
    border-radius: 10px;
    transition: width 0.3s;
  }
  button { 
    padding: 12px 25px; 
    font-size: 16px; 
    cursor: pointer; 
    border-radius: 8px; 
    border: none; 
    background: #2193b0; 
    color: white; 
    transition: 0.3s; 
    margin-top: 10px;
  }
  button:hover { background: #6dd5ed; color: #000; }
</style>
</head>
<body>

<div id="container">
  <div id="info">
    <h2>Welcome to the ESL Placement Test</h2>
    <p>You will be presented with 100 multiple-choice questions to determine your English level (A1–C2).<br>
    You can now choose to answer a question or skip it.<br>
    Skipping will move you forward, but unanswered questions count toward your progress and will affect your final score.<br>
    Correct answers will turn green, incorrect answers will turn red.<br>
    Your progress will be shown below as a progress bar.<br>
    Click "Start Quiz" when you are ready.</p>
    <button onclick="startQuiz()">Start Quiz</button>
  </div>

  <div id="quiz-box" style="display:none;">
    <div id="question-container"></div>
    <button id="skipBtn" onclick="skipQuestion()">Skip Question</button>
    <div id="progress-container"><div id="progress-bar"></div></div>
  </div>

  <div id="result" style="display:none;"></div>
</div>
<script>
const quizData = [
  // A1 (Beginner) 1-15
  {question: "What’s your name?", options: ["I name John","My name John","My name is John","I’m name John","Name is John"], correct:2},
  {question: "I ___ a student.", options: ["is","are","am","be","being"], correct:2},
  {question: "They ___ from Spain.", options: ["is","are","am","be","was"], correct:1},
  {question: "She ___ to school every day.", options: ["go","goes","going","goed","is go"], correct:1},
  {question: "Which one is a color?", options: ["Chair","Blue","Run","Book","Sleep"], correct:1},
  {question: "We ___ TV now.", options: ["watch","watches","are watching","watching","is watching"], correct:2},
  {question: "The opposite of 'hot' is ___", options: ["tall","cold","thin","big","short"], correct:1},
  {question: "Choose the correct sentence: He don’t like pizza, ___?", options: ["He don’t like pizza.","He doesn’t like pizza.","He isn’t likes pizza.","He not like pizza.","He no like pizza."], correct:1},
  {question: "___ is your favorite food?", options: ["Who","What","Where","How","Why"], correct:1},
  {question: "We have two ___.", options: ["childs","children","childs’","childrens","child"], correct:1},
  {question: "I usually ___ at 7 a.m.", options: ["get up","gets up","getting up","got up","is get up"], correct:0},
  {question: "Excuse me, ___ you help me?", options: ["can","must","should","may to","can to"], correct:0},
  {question: "Which is correct? She is more tall than him.", options: ["She is more tall than him.","She is tallest than him.","She is taller than him.","She is tall than him.","She tall than him."], correct:2},
  {question: "He ___ in Paris last year.", options: ["live","lives","living","lived","is living"], correct:3},
  {question: "Where ___ yesterday?", options: ["you go","did you go","do you went","you did go","did you went"], correct:1},

  // A2 (Elementary) 16-30
  {question: "If it ___ tomorrow, we’ll stay home.", options:["rains","rained","raining","rain","will rain"], correct:0},
  {question: "She ___ to London twice.", options:["has been","was","is","had been","goes"], correct:0},
  {question: "They ___ dinner when I called.", options:["have","having","were having","has","are have"], correct:2},
  {question: "Which one is correct? I have much friends.", options:["I have much friends.","I have many friends.","I have a lots of friends.","I have lot friends.","I have plenty friend."], correct:1},
  {question: "The film was really ___.", options:["bored","boring","bore","to bore","boredom"], correct:1},
  {question: "Choose the correct question tag: You like coffee, ___?", options:["do you","don’t you","didn’t you","are you","isn’t it"], correct:1},
  {question: "I ___ go to the gym on Mondays.", options:["usually","usual","use","using","is usually"], correct:0},
  {question: "The letter was written ___ Maria.", options:["by","with","to","from","of"], correct:0},
  {question: "I ___ him since we were children.", options:["know","knew","known","have known","has known"], correct:3},
  {question: "Which is a synonym of 'happy'?", options:["Sad","Angry","Glad","Tired","Sick"], correct:2},
  {question: "My house is ___ than yours.", options:["big","bigger","biggest","more big","most big"], correct:1},
  {question: "Choose the correct future form: We ___ at 8 tomorrow.", options:["meet","will meet","are meet","will meeting","meets"], correct:1},
  {question: "I’ve lived here ___ 2010.", options:["for","ago","since","until","during"], correct:2},
  {question: "___ you ever been to Canada?", options:["Do","Did","Have","Has","Had"], correct:2},
  {question: "He speaks English ___ than his brother.", options:["good","best","better","well","more good"], correct:2},

  // B1 (Intermediate) 31-45
  {question: "If I ___ rich, I’d travel the world.", options:["am","was","were","be","been"], correct:2},
  {question: "By the time she arrived, we ___ eating.", options:["finish","have finished","had finished","finished","finishing"], correct:2},
  {question: "I don’t mind ___ late tonight.", options:["working","work","to work","worked","works"], correct:0},
  {question: "They said they ___ the project before the deadline.", options:["finish","finished","will finish","would finish","are finishing"], correct:3},
  {question: "She asked me if I ___ help her.", options:["can","could","may","will","shall"], correct:1},
  {question: "Which sentence is correct? I look forward to meeting you.", options:["I look forward to meet you.","I look forward meeting you.","I look forward to meeting you.","I look forward meet you.","I look forwarded to meet you."], correct:2},
  {question: "The book ___ by millions of people.", options:["reads","is read","read","is reading","was readed"], correct:1},
  {question: "Choose the correct phrasal verb: She ___ the lights before leaving.", options:["turned off","turned out","turned up","turned in","turned over"], correct:0},
  {question: "The test was difficult, but he ___ to pass it.", options:["managed","could","can","succeed","successed"], correct:0},
  {question: "Which is correct? Although tired, he kept working.", options:["Despite he was tired, he kept working.","Despite of being tired, he kept working.","Although tired, he kept working.","Although of tired, he kept working.","Despite tired, he kept working."], correct:2},
  {question: "If you ___ earlier, we wouldn’t have missed the train.", options:["arrive","arrived","had arrived","have arrived","arriving"], correct:2},
  {question: "Choose the correct sentence: She suggested going.", options:["She suggested me to go.","She suggested to go.","She suggested going.","She suggested that I go.","Both C and D"], correct:4},
  {question: "I’ll call you as soon as I ___.", options:["arrive","arrived","will arrive","arrives","arriving"], correct:0},
  {question: "The man ___ wallet was stolen went to the police.", options:["which","who","whose","whom","that"], correct:2},
  {question: "The manager made us ___ longer hours.", options:["work","working","to work","worked","works"], correct:0},

  // B2 (Upper-Intermediate) 46-65
  {question: "If he ___ earlier, he wouldn't have missed the bus.", options:["left","leaves","had left","has left","leaving"], correct:2},
  {question: "She advised me ___ more careful.", options:["be","being","to be","been","was"], correct:2},
  {question: "Despite ___ tired, he kept working.", options:["being","be","been","is","was"], correct:0},
  {question: "I would have helped you if I ___ about it.", options:["know","knew","had known","knows","known"], correct:2},
  {question: "They insisted that he ___ present at the meeting.", options:["be","is","was","being","been"], correct:0},
  {question: "This is the first time I ___ sushi.", options:["eat","ate","have eaten","had eaten","eating"], correct:2},
  {question: "She acts as if she ___ the boss.", options:["is","was","were","be","been"], correct:2},
  {question: "I’m looking forward to ___ you next week.", options:["see","seeing","to see","saw","seen"], correct:1},
  {question: "It’s high time you ___ your homework.", options:["do","did","done","does","doing"], correct:1},
  {question: "He demanded that the documents ___ immediately.", options:["send","sent","be sent","is sent","was sent"], correct:2},
  {question: "The more you practice, the ___ you become.", options:["good","better","best","well","more better"], correct:1},
  {question: "She made me ___ the report.", options:["finish","finished","finishing","to finish","finishes"], correct:0},
  {question: "Had I known, I ___ you earlier.", options:["call","called","would have called","have called","calls"], correct:2},
  {question: "I prefer tea ___ coffee.", options:["than","to","over","with","more"], correct:1},
  {question: "No sooner had we arrived ___ it started raining.", options:["than","when","as","then","and"], correct:0},
  {question: "He is used to ___ up early.", options:["get","got","getting","gets","to get"], correct:2},
  {question: "I wish I ___ taller.", options:["am","was","were","be","being"], correct:2},
  {question: "It’s essential that everyone ___ on time.", options:["arrive","arrives","arrived","arriving","is arrive"], correct:0},
  {question: "She spoke as though she ___ everything.", options:["know","knew","has known","had known","known"], correct:1},
  {question: "By next year, I ___ my degree.", options:["will complete","will have completed","completed","have completed","completing"], correct:1},

  // C1 (Advanced) 66-85
  {question: "The new policy requires that all employees ___ a training.", options:["attend","attends","attended","attending","to attend"], correct:0},
  {question: "He would have gone if he ___ invited.", options:["was","were","had been","is","being"], correct:2},
  {question: "Hardly ___ the meeting started when the fire alarm rang.", options:["had","have","has","having","was"], correct:0},
  {question: "I would rather you ___ here tomorrow.", options:["come","came","comes","coming","to come"], correct:1},
  {question: "It’s high time the project ___ completed.", options:["is","was","be","were","been"], correct:3},
  {question: "She talks as if she ___ the whole world.", options:["knows","knew","known","had known","was knowing"], correct:1},
  {question: "I cannot help ___ when I hear such stories.", options:["laugh","laughing","to laugh","laughed","laughs"], correct:1},
  {question: "No one else but John ___ responsible for the mistake.", options:["is","was","be","being","has"], correct:1},
  {question: "Rarely ___ I seen such dedication.", options:["have","had","has","having","was"], correct:0},
  {question: "He suggested that she ___ the contract carefully.", options:["read","reads","readed","reading","to read"], correct:0},
  {question: "The teacher demanded that the students ___ quiet.", options:["be","is","are","was","been"], correct:0},
  {question: "I would have gone if it ___ not raining.", options:["is","was","had","has","were"], correct:2},
  {question: "She acted as though she ___ offended.", options:["was","were","is","be","been"], correct:1},
  {question: "It’s imperative that everyone ___ the rules.", options:["follows","follow","followed","following","to follow"], correct:1},
  {question: "Had he tried harder, he ___ succeeded.", options:["would","will","would have","has","had"], correct:2},
  {question: "The committee recommends that the policy ___ reviewed.", options:["be","is","was","being","has been"], correct:0},
  {question: "I would rather you ___ me the truth.", options:["tell","told","tells","telling","to tell"], correct:1},
  {question: "She behaves as if she ___ the owner.", options:["is","was","were","be","been"], correct:2},
  {question: "No sooner ___ the plane taken off than it started to rain.", options:["had","have","has","was","having"], correct:0},
  {question: "I wish I ___ spoken to him earlier.", options:["had","have","has","haved","having"], correct:0},

  // C2 (Proficient) 86-100
   {question: "The manager demanded that the report ___ immediately.", options:["be submitted","is submitted","submitted","submits","was submitted"], correct:0}
];

let currentQuestion = 0;
let score = 0;

const infoBox = document.getElementById("info");
const quizBox = document.getElementById("quiz-box");
const questionContainer = document.getElementById("question-container");
const progressBar = document.getElementById("progress-bar");
const resultBox = document.getElementById("result");
const skipBtn = document.getElementById("skipBtn");

function startQuiz() {
  infoBox.style.display = "none";
  quizBox.style.display = "block";
  loadQuestion();
}

function loadQuestion() {
  if (currentQuestion >= quizData.length) {
    quizBox.style.display = "none";
    resultBox.style.display = "block";
    showLevel();
    return;
  }

  let q = quizData[currentQuestion];
  questionContainer.innerHTML = `<div class="question">${currentQuestion+1}. ${q.question}</div>
    ${q.options.map((opt,i)=>`<div class="option" onclick="checkAnswer(${i})">${opt}</div>`).join("")}`;
}

function checkAnswer(selected) {
  let q = quizData[currentQuestion];
  let options = document.querySelectorAll(".option");
  options.forEach((btn, idx) => {
    btn.style.pointerEvents = "none";
    if(idx === q.correct) btn.classList.add("correct");
    else if(idx === selected) btn.classList.add("wrong");
  });
  if(selected === q.correct) score++;
  currentQuestion++;
  progressBar.style.width = (currentQuestion/quizData.length*100) + "%";
  setTimeout(loadQuestion, 800);
}

function skipQuestion() {
  currentQuestion++;
  progressBar.style.width = (currentQuestion/quizData.length*100) + "%";
  loadQuestion();
}

function showLevel() {
  let percent = (score/quizData.length) * 100;
  let level;
  if(percent <= 20) level = "A1 (Beginner)";
  else if(percent <= 40) level = "A2 (Elementary)";
  else if(percent <= 60) level = "B1 (Intermediate)";
  else if(percent <= 80) level = "B2 (Upper-Intermediate)";
  else if(percent <= 90) level = "C1 (Advanced)";
  else level = "C2 (Proficient)";
  resultBox.innerHTML = `🎉 Your English level is: <strong>${level}</strong> (${score}/${quizData.length})`;
}
</script>
</body>
</html>
