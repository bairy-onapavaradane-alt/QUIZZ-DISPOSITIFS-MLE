<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Quiz Mission Locale</title>

<style>
body {
  margin:0;
  font-family: 'Segoe UI', sans-serif;
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
  box-shadow:0 10px 25px rgba(0,0,0,0.3);
}

h2 { text-align:center; }

.progress {
  height:6px;
  background:#334155;
  border-radius:10px;
  overflow:hidden;
  margin:15px 0;
}

.progress-fill {
  height:100%;
  width:0%;
  background:#38bdf8;
  transition:0.3s;
}

button {
  width:100%;
  padding:14px;
  margin:8px 0;
  border:none;
  border-radius:12px;
  font-size:16px;
  cursor:pointer;
  color:white;
  transition:0.2s;
}

button:hover { transform:scale(1.04); }

.red { background:#ef4444; }
.blue { background:#3b82f6; }
.green { background:#22c55e; }
.yellow { background:#eab308; color:black; }

.start { background:#22c55e; }

.result {
  text-align:center;
}

.result h3 {
  font-size:24px;
  margin-bottom:10px;
}

.badge {
  font-size:40px;
}

.fade {
  animation:fade 0.4s;
}

@keyframes fade {
  from {opacity:0; transform:translateY(10px);}
  to {opacity:1;}
}
</style>
</head>

<body>

<div class="container">
<div class="card">

<h2>🎯 Ton orientation</h2>

<div id="start">
<p>Commence le quiz</p>
<button class="start" onclick="startQuiz()">Démarrer</button>
</div>

<div id="quiz" style="display:none;">
<div class="progress"><div id="bar" class="progress-fill"></div></div>
<p id="q"></p>
<div id="a"></div>
</div>

<div id="result"></div>

</div>
</div>

<script>

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
  {q:"Logement stable ?", a:[
    {t:"Oui", v:"none", c:"green"},
    {t:"Non", v:"logement", c:"yellow"}
  ]},
  {q:"Difficultés financières ?", a:[
    {t:"Oui", v:"argent", c:"yellow"},
    {t:"Non", v:"none", c:"green"}
  ]}
];

var i = 0;
var score = JSON.parse(localStorage.getItem("score")) || {psad:0, emploi:0, difficulte:0, logement:0, argent:0};

function startQuiz(){
  document.getElementById("start").style.display="none";
  document.getElementById("quiz").style.display="block";
  show();
}

function show(){
  if(i >= quiz.length){
    finish();
    return;
  }

  document.getElementById("bar").style.width = (i/quiz.length)*100 + "%";

  var q = quiz[i];
  document.getElementById("q").innerHTML = q.q;
  var aDiv = document.getElementById("a");
  aDiv.innerHTML="";

  q.a.forEach(rep=>{
    var b = document.createElement("button");
    b.textContent = rep.t;
    b.className = rep.c + " fade";

    b.onclick = ()=>{
      if(rep.v !== "none") score[rep.v]++;
      localStorage.setItem("score", JSON.stringify(score));
      i++;
      show();
    };

    aDiv.appendChild(b);
  });
}

function finish(){

  var max = Object.keys(score).reduce((a,b)=>score[a]>score[b]?a:b);

  var data = {
    psad:["🔄","PSAD","Remise en parcours"],
    emploi:["💼","Emploi","Insertion pro"],
    difficulte:["🧠","TAPAJ / BIP","Accompagnement renforcé"],
    logement:["🏠","Logement","Accès logement"],
    argent:["💰","Aides","Soutien financier"]
  };

  var r = data[max];

  document.getElementById("quiz").style.display="none";

  document.getElementById("result").innerHTML = `
    <div class="result fade">
      <div class="badge">${r[0]}</div>
      <h3>${r[1]}</h3>
      <p>${r[2]}</p>

      <button class="start" onclick="restart()">Recommencer</button>
    </div>
  `;

  document.getElementById("bar").style.width = "100%";
}

function restart(){
  localStorage.clear();
  location.reload();
}

</script>

</body>
</html>

});
</script>

</body>
</html>
