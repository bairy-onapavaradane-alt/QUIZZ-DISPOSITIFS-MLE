function finish(){

// tri des scores
var sorted = Object.entries(score).sort((a,b)=>b[1]-a[1]);

// priorité = urgence sociale
var urgence = sorted.find(x => ["logement","argent","difficulte"].includes(x[0]) && x[1] >= 4);

// dispositif principal
var principal = urgence ? urgence[0] : sorted[0][0];

// secondaires
var secondaires = sorted.filter(x => x[0] !== principal && x[1] > 0).slice(0,2);

// base données dispositifs
var data = {
psad:["🔄","Remobilisation (PSAD)","Retrouver un rythme et construire un projet."],
emploi:["💼","Insertion professionnelle","Accès emploi ou formation."],
difficulte:["🧠","Accompagnement renforcé","Soutien global (social, santé)."],
logement:["🏠","Accès logement","Stabiliser ta situation de logement."],
argent:["💰","Aides financières","Répondre à une urgence financière."]
};

var html = "<h3>🎯 Ton orientation personnalisée</h3>";

// principal
var p = data[principal];
html += `<div class="result-box">
<h4>👉 Priorité : ${p[0]} ${p[1]}</h4>
<p>${p[2]}</p>
</div>`;

// secondaires
if(secondaires.length){
html += "<h4>🔎 À travailler ensuite :</h4>";
secondaires.forEach(s=>{
var d = data[s[0]];
html += `<div class="result-box">
<h4>${d[0]} ${d[1]}</h4>
<p>${d[2]}</p>
</div>`;
});
}

html += `<button class="start" onclick="location.reload()">Recommencer</button>`;

document.getElementById("quiz").style.display="none";
document.getElementById("result").innerHTML = html;
}
