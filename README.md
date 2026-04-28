<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Diagnostic Mission Locale</title>

<style>
body {
  margin:0;
  font-family: Arial;
  background:#0f172a;
  color:white;
}

.container {
  max-width:420px;
  margin:auto;
  padding:20px;
}

.card {
  background:#1e293b;
  padding:20px;
  border-radius:20px;
}

h2 { text-align:center; }

button {
  width:100%;
  padding:14px;
  margin:8px 0;
  border:none;
  border-radius:12px;
  font-size:16px;
  cursor:pointer;
  background:#334155;
  color:white;
}

button:hover { opacity:0.9; }

.red { background:#ef4444; }
.blue { background:#3b82f6; }
.green { background:#22c55e; }
.yellow { background:#eab308; color:black; }

.start { background:#22c55e; }

.result-box {
  background:#334155;
  padding:15px;
  border-radius:10px;
  margin-top:10px;
}
</style>
</head>

<body>

<div class="container">
<div class="card">

<h2>🎯 Diagnostic rapide</h2>

<div id="start">
<p>Réponds aux questions</p>
<button class="start" onclick="startQuiz()">Commencer</button>
</div>

<div id="quiz" style="display:none;">
<p id="question"></p>
<div id="answers"></div>
</div>

<div id="result"></div>

</div>
</div>

<script>

const quiz = [
{
q:"Situation actuelle ?",
a:[
{t:"Aucune activité", type:"psad", w:3, c:"red"},
{t:"Recherche d'emploi", type:"emploi", w:2, c:"blue"},
{t:"Grande difficulté", type:"difficulte", w:4, c:"green"}
]
},
{
q:"Projet professionnel ?",
a:[
{t:"Clair", type:"emploi", w:3, c:"blue"},
{t:"Flou", type:"psad", w:2, c:"red"},
{t:"Aucun", type:"psad", w:3, c:"red"}
]
},
{
q:"Logement ?",
a:[
{t:"Stable", type:"none", w:0, c:"green"},
{t:"Précaire", type:"logement", w:3, c:"yellow"},
{t:"Sans logement", type:"logement", w:5, c:"yellow"}
]
}
];

let i = 0;
let score = {psad:0, emploi:0, difficulte:0, logement:0};

function startQuiz(){
document.getElementById("start").style.display="none";
document.getElementById("quiz").style.display="block";
showQuestion();
}

function showQuestion(){

if(i >= quiz.length){
finishQuiz();
return;
}

let q = quiz[i];
document.getElementById("question").innerText = q.q;

let answers = document.getElementById("answers");
answers.innerHTML = "";

q.a.forEach(rep => {
let btn = document.createElement("button");
btn.textContent = rep.t;
btn.className = rep.c;

btn.onclick = () => {
if(rep.type !== "none"){
score[rep.type] += rep.w;
}
i++;
showQuestion();
};

answers.appendChild(btn);
});
}

function finishQuiz(){

let sorted = Object.entries(score).sort((a,b)=>b[1]-a[1]);
let top = sorted[0][0];

let data = {
psad:"🔄 Remobilisation",
emploi:"💼 Emploi / Formation",
difficulte:"🧠 Accompagnement renforcé",
logement:"🏠 Logement"
};

document.getElementById("quiz").style.display="none";

document.getElementById("result").innerHTML = `
<h3>Résultat :</h3>
<div class="result-box">
${data[top]}
</div>
<button class="start" onclick="location.reload()">Recommencer</button>
`;
}

</script>

</body>
</html>
}

</script>

</body>
</html>
