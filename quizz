<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Quiz Mission Locale</title>

<style>
body {
  font-family: Arial;
  background: linear-gradient(135deg, #74ebd5, #9face6);
  padding: 20px;
  margin: 0;
}

.container {
  max-width: 500px;
  margin: auto;
  background: white;
  padding: 25px;
  border-radius: 15px;
  text-align: center;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
}

h2 {
  margin-bottom: 10px;
}

#progress {
  font-size: 14px;
  color: #666;
  margin-bottom: 10px;
}

.bar {
  height: 8px;
  background: #eee;
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 20px;
}

.bar-fill {
  height: 100%;
  width: 0%;
  background: #3498db;
  transition: width 0.3s;
}

button {
  display: block;
  width: 100%;
  padding: 14px;
  margin: 8px 0;
  border: none;
  border-radius: 10px;
  font-size: 16px;
  color: white;
  cursor: pointer;
  transition: transform 0.2s, opacity 0.2s;
}

button:hover {
  transform: scale(1.05);
  opacity: 0.9;
}

.start { background:#28a745; }
.red { background:#e74c3c; }
.blue { background:#3498db; }
.green { background:#2ecc71; }
.yellow { background:#f1c40f; color:black; }

.fade {
  animation: fadeIn 0.4s ease;
}

@keyframes fadeIn {
  from {opacity: 0; transform: translateY(10px);}
  to {opacity: 1; transform: translateY(0);}
}

.result-box {
  padding: 15px;
  border-radius: 10px;
  background: #f8f9fa;
  margin-top: 15px;
}
</style>
</head>

<body>

<div class="container">

<h2>🎯 Quel dispositif est fait pour toi ?</h2>

<div id="startScreen">
<p>Clique pour commencer le quiz</p>
<button class="start" id="startBtn">Commencer</button>
</div>

<p id="progress"></p>
<div class="bar"><div class="bar-fill" id="barFill"></div></div>

<p id="question"></p>
<div id="answers"></div>
<div id="result"></div>

</div>

<script>
document.addEventListener("DOMContentLoaded", function(){

var quiz = [
  {q:"Que fais-tu actuellement ?", a:[
    {t:"Rien", v:"psad", c:"red"},
    {t:"Je cherche un emploi", v:"emploi", c:"blue"},
    {t:"J'ai des difficultés", v:"difficulte", c:"green"}
  ]},
  {q:"As-tu un projet professionnel ?", a:[
    {t:"Oui", v:"emploi", c:"blue"},
    {t:"Non", v:"psad", c:"red"}
  ]},
  {q:"As-tu un logement stable ?", a:[
    {t:"Oui", v:"none", c:"green"},
    {t:"Non", v:"logement", c:"yellow"}
  ]},
  {q:"As-tu des difficultés financières ?", a:[
    {t:"Oui", v:"argent", c:"yellow"},
    {t:"Non", v:"none", c:"green"}
  ]}
];

var i = 0;
var score = {psad:0, emploi:0, difficulte:0, logement:0, argent:0};

var startBtn = document.getElementById("startBtn");
var startScreen = document.getElementById("startScreen");
var questionEl = document.getElementById("question");
var answersEl = document.getElementById("answers");
var resultEl = document.getElementById("result");
var progressEl = document.getElementById("progress");
var barFill = document.getElementById("barFill");

startBtn.addEventListener("click", function(){
  startScreen.style.display = "none";
  show();
});

function updateProgress(){
  progressEl.innerHTML = "Question " + (i+1) + " / " + quiz.length;
  barFill.style.width = ((i)/quiz.length)*100 + "%";
}

function show(){
  if(i >= quiz.length){
    showResult();
    return;
  }

  updateProgress();

  questionEl.className = "fade";
  questionEl.innerHTML = quiz[i].q;
  answersEl.innerHTML = "";

  quiz[i].a.forEach(function(rep){
    var btn = document.createElement("button");
    btn.textContent = rep.t;
    btn.className = rep.c + " fade";

    btn.addEventListener("click", function(){
      answersEl.innerHTML = "";
      if(rep.v !== "none") score[rep.v]++;
      i++;
      setTimeout(show, 300);
    });

    answersEl.appendChild(btn);
  });
}

function showResult(){

  var max = null;
  var maxValue = -1;

  for(var k in score){
    if(score[k] > maxValue){
      maxValue = score[k];
      max = k;
    }
  }

  var res = "";
  var desc = "";

  if(max == "psad"){
    res = "PSAD";
    desc = "Remise en parcours, accompagnement pour repartir sur de bonnes bases.";
  }
  if(max == "emploi"){
    res = "Accompagnement vers l'emploi";
    desc = "Aide pour trouver un travail ou une formation.";
  }
  if(max == "difficulte"){
    res = "TAPAJ / BIP";
    desc = "Dispositifs pour jeunes en difficulté avec accompagnement renforcé.";
  }
  if(max == "logement"){
    res = "Jeunes et Logés";
    desc = "Aide pour accéder à un logement stable.";
  }
  if(max == "argent"){
    res = "FDAJ / FUI";
    desc = "Aides financières pour soutenir ton projet.";
  }

  questionEl.innerHTML = "🎉 Ton résultat :";
  answersEl.innerHTML = "";

  resultEl.innerHTML = `
    <div class="result-box fade">
      <h3>${res}</h3>
      <p>${desc}</p>
      <button class="start" onclick="location.reload()">Recommencer</button>
    </div>
  `;

  barFill.style.width = "100%";
}

});
</script>

</body>
</html>
