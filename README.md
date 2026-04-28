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
  color:white;
  background:#334155; /* 👈 CORRECTION visibilité */
}

button:hover {
  opacity:0.9;
}

/* couleurs réponses */
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
<p>Réponds aux questions pour être orienté</p>
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
q:"Quelle est ta situation actuelle ?",
a:[
{t:"Aucune activité (ni école, ni emploi)", type:"psad", w:3, c:"red"},
{t:"Je cherche un emploi ou une formation", type:"emploi", w:2, c:"blue"},
{t:"Situation difficile (isolement, santé…)", type:"difficulte", w:4, c:"green"}
]
},

{
q:"Ton projet professionnel :",
a:[
{t:"Clair", type:"emploi", w:3, c:"blue"},
{t:"En réflexion", type:"psad", w:2, c:"red"},
{t:"Aucun", type:"psad", w:3, c:"red"}
]
},

{
q:"Ton logement :",
a:[
{t:"Stable", type:"none", w:0, c:"green"},
{t:"Précaire", type:"logement", w:3, c:"yellow"},
{t:"Sans logement", type:"logement", w:5, c:"yellow"}
]
},

{
q:"Ta situation financière :",
a:[
{t:"Très difficile", type:"argent", w:4, c:"yellow"},
{t:"Un peu difficile", type:"argent", w:2, c:"yellow"},
{t:"Stable", type:"none", w:0, c:"green"}
]
},

{
q:"Accompagnement actuel :",
a:[
{t:"Oui suffisant", type:"none", w:0, c:"green"},
{t:"Oui mais insuffisant", type:"psad", w:2, c:"red"},
{t:"Aucun", type:"psad", w:3, c:"red"}
]
},

{
q:"Ta priorité aujourd’hui :",
a:[
{t:"Travailler", type:"emploi", w:4, c:"blue"},
{t:"Stabiliser ma vie", type:"difficulte", w:5, c:"green"},
{t:"Trouver un logement", type:"logement", w:5, c:"yellow"},
{t:"Avoir de l’argent", type:"argent", w:5, c:"yellow"}
]
}

];

let i = 0;

let score = {
psad:0,
emploi:0,
difficulte:0,
logement:0,
argent:0
};

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
btn.className = rep.c; // 👈 IMPORTANT (couleurs visibles)

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

let urgence = sorted.find(x =>
["logement","argent","difficulte"].includes(x[0]) && x[1] >= 4
);

let principal = urgence ? urgence[0] : sorted[0][0];

let secondaires = sorted
.filter(x => x[0] !== principal && x[1] > 0)
.slice(0,2);

let data = {
psad:["🔄","Remobilisation","Retrouver un rythme et construire un projet."],
emploi:["💼","Insertion professionnelle","Accès emploi ou formation."],
difficulte:["🧠","Accompagnement renforcé","Soutien social et personnel."],
logement:["🏠","Accès logement","Stabiliser ta situation."],
argent:["💰","Aides financières","Répondre à une urgence."]
};

let html = "<h3>🎯 Ton orientation</h3>";

let p = data[principal];

html += `
<div class="result-box">
<h4>👉 Priorité : ${p[0]} ${p[1]}</h4>
<p>${p[2]}</p>
</div>
`;

if(secondaires.length){
html += "<h4>🔎 Ensuite :</h4>";

secondaires.forEach(s=>{
let d = data[s[0]];
html += `
<div class="result-box">
<h4>${d[0]} ${d[1]}</h4>
<p>${d[2]}</p>
</div>
`;
});
}

html += `<button class="start" onclick="location.reload()">Recommencer</button>`;

document.getElementById("quiz").style.display="none";
document.getElementById("result").innerHTML = html;

}

</script>

</body>
</html>
