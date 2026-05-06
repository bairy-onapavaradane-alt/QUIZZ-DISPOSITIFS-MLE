import React, { useState } from "react";

// ================= CONFIG =================
const questions = [
  {
    name: "logement",
    label: "Quel est ton logement ?",
    options: [
      { value: "", label: "Choisir" },
      { value: "stable", label: "Stable" },
      { value: "precaire", label: "Précaire / hébergé" },
      { value: "sans", label: "Sans logement" }
    ]
  },
  {
    name: "sante",
    label: "Ta santé est-elle suivie ?",
    options: [
      { value: "", label: "Choisir" },
      { value: "ok", label: "Oui" },
      { value: "non", label: "Non" }
    ]
  },
  {
    name: "mobilite",
    label: "Peux-tu te déplacer facilement ?",
    options: [
      { value: "", label: "Choisir" },
      { value: "ok", label: "Oui" },
      { value: "non", label: "Non" }
    ]
  },
  {
    name: "projet",
    label: "As-tu un projet professionnel ?",
    options: [
      { value: "", label: "Choisir" },
      { value: "clair", label: "Clair" },
      { value: "flou", label: "Flou / aucun" }
    ]
  },
  {
    name: "competences",
    label: "Note tes compétences (0 à 10)",
    type: "number"
  },
  {
    name: "finance",
    label: "Situation financière stable ?",
    options: [
      { value: "", label: "Choisir" },
      { value: "ok", label: "Oui" },
      { value: "non", label: "Non" }
    ]
  }
];

// ================= LOGIQUE =================
const calculProfil = (r) => {
  let score = 0;

  if (r.logement === "stable") score += 2;
  if (r.logement === "sans") score -= 1;

  if (r.sante === "ok") score++;
  if (r.mobilite === "ok") score++;
  if (r.projet === "clair") score += 2;
  if (Number(r.competences) >= 7) score += 2;
  if (r.finance === "ok") score++;

  if (score <= 2) return "fragile";
  if (score <= 6) return "intermediaire";
  return "autonome";
};

const getDispositifs = (profil) => {
  if (profil === "fragile") {
    return [
      "PSAD / Décrochage",
      "TAPAJ",
      "BIP",
      "Aides financières",
      "Santé / psy",
      "Logement d'urgence"
    ];
  }

  if (profil === "intermediaire") {
    return [
      "Accompagnement vers l'emploi",
      "Jeunes et logés",
      "Ateliers CV",
      "Formation",
      "Mobilité"
    ];
  }

  return [
    "Emploi direct",
    "Alternance",
    "Création d'entreprise",
    "Logement autonome",
    "Réseau entreprise"
  ];
};

// ================= APP =================
export default function App() {
  const [step, setStep] = useState(0);
  const [profil, setProfil] = useState(null);

  const [reponses, setReponses] = useState({
    logement: "",
    sante: "",
    mobilite: "",
    projet: "",
    competences: "",
    finance: ""
  });

  const current = questions[step];

  const handleChange = (e) => {
    const { name, value } = e.target;
    setReponses({ ...reponses, [name]: value });
  };

  const next = () => {
    if (!reponses[current.name]) {
      alert("Réponds à la question");
      return;
    }
    setStep(step + 1);
  };

  const prev = () => setStep(step - 1);

  const submit = () => {
    const result = calculProfil(reponses);
    setProfil(result);
  };

  const reset = () => {
    setProfil(null);
    setStep(0);
    setReponses({
      logement: "",
      sante: "",
      mobilite: "",
      projet: "",
      competences: "",
      finance: ""
    });
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-100 p-4">
      <div className="bg-white p-6 rounded-2xl shadow w-full max-w-md">
        <h1 className="text-xl font-bold text-center mb-4">
          Diagnostic Mission Locale
        </h1>

        {!profil ? (
          <>
            <p className="mb-4">{current.label}</p>

            {current.type === "number" ? (
              <input
                type="number"
                name={current.name}
                value={reponses[current.name]}
                min="0"
                max="10"
                onChange={handleChange}
                className="w-full p-2 border rounded"
              />
            ) : (
              <select
                name={current.name}
                value={reponses[current.name]}
                onChange={handleChange}
                className="w-full p-2 border rounded"
              >
                {current.options.map((o, i) => (
                  <option key={i} value={o.value}>
                    {o.label}
                  </option>
                ))}
              </select>
            )}

            <div className="flex justify-between mt-6">
              {step > 0 && (
                <button onClick={prev} className="px-4 py-2 bg-gray-300 rounded">
                  Retour
                </button>
              )}

              {step < questions.length - 1 ? (
                <button onClick={next} className="px-4 py-2 bg-blue-500 text-white rounded ml-auto">
                  Suivant
                </button>
              ) : (
                <button onClick={submit} className="px-4 py-2 bg-green-500 text-white rounded ml-auto">
                  Résultat
                </button>
              )}
            </div>
          </>
        ) : (
          <div>
            <h2 className="text-lg font-bold mb-2">
              Profil : {profil.toUpperCase()}
            </h2>

            <p className="mb-2">Dispositifs conseillés :</p>
            <ul className="list-disc pl-5">
              {getDispositifs(profil).map((d, i) => (
                <li key={i}>{d}</li>
              ))}
            </ul>

            <button
              onClick={reset}
              className="mt-6 w-full bg-black text-white py-2 rounded"
            >
              Recommencer
            </button>
          </div>
        )}
      </div>
    </div>
  );
}
