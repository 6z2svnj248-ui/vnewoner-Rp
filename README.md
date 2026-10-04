<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ville RP</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: Arial, sans-serif;
    background: #111827;
    color: white;
}

header {
    background: #1f2937;
    padding: 15px;
    text-align: center;
    border-bottom: 2px solid #374151;
}

header h1 {
    color: #60a5fa;
}

#game {
    display: flex;
    min-height: calc(100vh - 70px);
}

/* MENU */
#sidebar {
    width: 260px;
    background: #1f2937;
    padding: 15px;
}

#sidebar h2 {
    margin-bottom: 15px;
}

.stats {
    background: #111827;
    padding: 12px;
    border-radius: 10px;
    margin-bottom: 15px;
}

.stat {
    display: flex;
    justify-content: space-between;
    margin: 8px 0;
}

button {
    width: 100%;
    padding: 11px;
    margin: 5px 0;
    border: none;
    border-radius: 8px;
    background: #2563eb;
    color: white;
    cursor: pointer;
    font-size: 15px;
}

button:hover {
    background: #1d4ed8;
}

/* MISSION */
#missionStatus {
    background: #030712;
    padding: 12px;
    border-radius: 10px;
    margin-bottom: 10px;
    color: #d1d5db;
    text-align: center;
}

/* VILLE */
#city {
    flex: 1;
    padding: 20px;
}

#map {
    position: relative;
    height: 520px;
    max-width: 900px;
    margin: auto;
    background:
        linear-gradient(90deg, transparent 48%, #374151 48%, #374151 52%, transparent 52%),
        linear-gradient(0deg, transparent 48%, #374151 48%, #374151 52%, transparent 52%),
        #166534;
    border: 4px solid #374151;
    border-radius: 15px;
    overflow: hidden;
}

.location {
    position: absolute;
    width: 130px;
    padding: 12px;
    text-align: center;
    background: #1f2937;
    border: 2px solid #60a5fa;
    border-radius: 10px;
    cursor: pointer;
    transition: .2s;
}

.location:hover {
    transform: scale(1.05);
    background: #374151;
}

#mairie {
    top: 30px;
    left: 40px;
}

#police {
    top: 30px;
    right: 40px;
}

#hopital {
    bottom: 30px;
    left: 40px;
}

#garage {
    bottom: 30px;
    right: 40px;
}

#restaurant {
    top: 220px;
    left: 50%;
    transform: translateX(-50%);
}

#player {
    position: absolute;
    width: 25px;
    height: 25px;
    background: #facc15;
    border-radius: 50%;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 15px #facc15;
}

/* JOURNAL */
#log {
    max-width: 900px;
    height: 130px;
    overflow-y: auto;
    margin: 15px auto;
    background: #030712;
    border-radius: 10px;
    padding: 12px;
}

.logLine {
    margin-bottom: 6px;
    color: #d1d5db;
}

/* MODAL */
#modal {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,.7);
    align-items: center;
    justify-content: center;
    z-index: 10;
}

.modalBox {
    width: 90%;
    max-width: 450px;
    background: #1f2937;
    padding: 25px;
    border-radius: 15px;
}

.modalBox input,
.modalBox select {
    width: 100%;
    padding: 12px;
    margin: 8px 0;
    border-radius: 8px;
    border: none;
}

.close {
    background: #dc2626;
}

.close:hover {
    background: #b91c1c;
}

.missionInfo {
    background: #111827;
    padding: 15px;
    border-radius: 10px;
    margin: 10px 0;
    text-align: center;
}

.missionReward {
    color: #4ade80;
    font-weight: bold;
    margin-top: 8px;
}

@media(max-width: 800px) {

    #game {
        flex-direction: column;
    }

    #sidebar {
        width: 100%;
    }

    #map {
        height: 450px;
    }
}
</style>
</head>

<body>

<header>
    <h1>🏙️ VILLE RP</h1>
    <p>Bienvenue dans ta nouvelle vie</p>
</header>

<div id="game">

    <aside id="sidebar">

        <h2>👤 Personnage</h2>

        <div class="stats">

            <div class="stat">
                <span>Nom</span>
                <strong id="name">Inconnu</strong>
            </div>

            <div class="stat">
                <span>💰 Argent</span>
                <strong id="money">500 €</strong>
            </div>

            <div class="stat">
                <span>❤️ Santé</span>
                <strong id="health">100</strong>
            </div>

            <div class="stat">
                <span>🍔 Faim</span>
                <strong id="hunger">100</strong>
            </div>

            <div class="stat">
                <span>💼 Métier</span>
                <strong id="job">Sans emploi</strong>
            </div>

            <div class="stat">
                <span>🚗 Véhicule</span>
                <strong id="vehicle">Aucun</strong>
            </div>

        </div>

        <div id="missionStatus">
            Aucune mission en cours
        </div>

        <button onclick="createCharacter()">
            👤 Personnage
        </button>

        <button onclick="openJobs()">
            💼 Métiers
        </button>

        <button onclick="showMission()">
            📋 Mission
        </button>

        <button onclick="eat()">
            🍔 Manger
        </button>

        <button onclick="saveGame()">
            💾 Sauvegarder
        </button>

        <button onclick="loadGame()">
            📂 Charger
        </button>

    </aside>

    <main id="city">

        <div id="map">

            <div id="player"></div>

            <div class="location"
                 id="mairie"
                 onclick="goLocation('Mairie')">
                🏛️<br>
                Mairie
            </div>

            <div class="location"
                 id="police"
                 onclick="goLocation('Commissariat')">
                👮<br>
                Commissariat
            </div>

            <div class="location"
                 id="hopital"
                 onclick="goLocation('Hôpital')">
                🏥<br>
                Hôpital
            </div>

            <div class="location"
                 id="garage"
                 onclick="goLocation('Garage')">
                🚗<br>
                Garage
            </div>

            <div class="location"
                 id="restaurant"
                 onclick="goLocation('Restaurant')">
                🍔<br>
                Restaurant
            </div>

        </div>

        <div id="log"></div>

    </main>
</div>


<!-- MODAL -->

<div id="modal">

    <div class="modalBox">

        <h2 id="modalTitle">Menu</h2>

        <div id="modalContent"></div>

        <button class="close" onclick="closeModal()">
            Fermer
        </button>

    </div>

</div>


<script>

/* =========================
   DONNÉES DU JOUEUR
========================= */

let player = {
    name: "Inconnu",
    money: 500,
    health: 100,
    hunger: 100,
    job: "Sans emploi",
    vehicle: "Aucun",
    inventory: []
};


/* =========================
   MISSION ACTUELLE
========================= */

let currentMission = null;


/* =========================
   MISSIONS
========================= */

const missions = {

    "Chauffeur": [

        {
            name: "🚕 Transporter un client",
            reward: 80
        },

        {
            name: "🚕 Faire une course en ville",
            reward: 100
        },

        {
            name: "🚕 Déposer un client à la mairie",
            reward: 120
        }

    ],

    "Policier": [

        {
            name: "🚨 Effectuer une patrouille",
            reward: 120
        },

        {
            name: "🚨 Répondre à une intervention",
            reward: 150
        },

        {
            name: "🚨 Sécuriser la mairie",
            reward: 180
        }

    ],

    "Médecin": [

        {
            name: "🏥 Intervenir auprès d'un patient",
            reward: 150
        },

        {
            name: "🚑 Effectuer une intervention médicale",
            reward: 180
        },

        {
            name: "🏥 Soigner un patient",
            reward: 130
        }

    ],

    "Mécanicien": [

        {
            name: "🔧 Réparer un véhicule",
            reward: 100
        },

        {
            name: "🔧 Dépanner un véhicule",
            reward: 130
        },

        {
            name: "🔧 Réparer une voiture en panne",
            reward: 150
        }

    ]

};


/* =========================
   INTERFACE
========================= */

function updateUI() {

    document.getElementById("name").textContent =
        player.name;

    document.getElementById("money").textContent =
        player.money + " €";

    document.getElementById("health").textContent =
        player.health;

    document.getElementById("hunger").textContent =
        player.hunger;

    document.getElementById("job").textContent =
        player.job;

    document.getElementById("vehicle").textContent =
        player.vehicle;

    updateMissionUI();
}


/* =========================
   JOURNAL
========================= */

function log(message) {

    const logBox =
        document.getElementById("log");

    const line =
        document.createElement("div");

    line.className = "logLine";

    line.textContent =
        "▶ " + message;

    logBox.prepend(line);
}


/* =========================
   PERSONNAGE
========================= */

function createCharacter() {

    openModal(
        "Créer ton personnage",
        `
        <input
            id="characterName"
            placeholder="Prénom et nom">

        <button onclick="confirmCharacter()">
            Créer
        </button>
        `
    );
}


function confirmCharacter() {

    const name =
        document.getElementById("characterName").value;

    if (!name.trim()) {

        alert("Entre un nom.");

        return;
    }

    player.name = name;

    updateUI();

    closeModal();

    log(
        "Bienvenue " +
        player.name +
        " !"
    );
}


/* =========================
   MÉTIERS
========================= */

function openJobs() {

    openModal(
        "Choisir un métier",
        `
        <button onclick="chooseJob('Chauffeur')">
            🚕 Chauffeur
        </button>

        <button onclick="chooseJob('Policier')">
            👮 Policier
        </button>

        <button onclick="chooseJob('Médecin')">
            🏥 Médecin
        </button>

        <button onclick="chooseJob('Mécanicien')">
            🔧 Mécanicien
        </button>
        `
    );
}


function chooseJob(job) {

    if (currentMission) {

        log(
            "⚠️ Termine ta mission avant de changer de métier."
        );

        closeModal();

        return;
    }

    player.job = job;

    updateUI();

    closeModal();

    log(
        "💼 Tu es maintenant " +
        job +
        "."
    );

    log(
        "📋 Une mission est disponible."
    );
}


/* =========================
   SYSTÈME DE MISSIONS
========================= */

function showMission() {

    if (player.job === "Sans emploi") {

        openModal(
            "📋 Missions",
            `
            <div class="missionInfo">
                <p>
                    Tu dois avoir un métier
                    pour effectuer une mission.
                </p>
            </div>

            <button onclick="openJobs()">
                💼 Choisir un métier
            </button>
            `
        );

        return;
    }


    if (currentMission) {

        openModal(
            "📋 Mission en cours",
            `
            <div class="missionInfo">

                <p>
                    <strong>
                        ${currentMission.name}
                    </strong>
                </p>

                <p class="missionReward">
                    💰 Récompense :
                    ${currentMission.reward} €
                </p>

                <p style="margin-top:10px;">
                    ⏳ Mission en cours...
                </p>

            </div>
            `
        );

        return;
    }


    openModal(
        "📋 Nouvelle mission",
        `
        <div class="missionInfo">

            <p>
                💼 Métier :
                <strong>${player.job}</strong>
            </p>

            <p style="margin-top:10px;">
                Une mission va t'être attribuée.
            </p>

        </div>

        <button onclick="startMission()">
            🚀 Commencer la mission
        </button>
        `
    );
}


function startMission() {

    if (player.job === "Sans emploi") {

        log(
            "❌ Tu dois avoir un métier."
        );

        return;
    }


    if (currentMission) {

        log(
            "⚠️ Tu as déjà une mission en cours."
        );

        return;
    }


    const jobMissions =
        missions[player.job];


    const mission =
        jobMissions[
            Math.floor(
                Math.random() *
                jobMissions.length
            )
        ];


    currentMission = {

        name: mission.name,

        reward: mission.reward

    };


    closeModal();

    updateMissionUI();

    log(
        "📋 Mission commencée : " +
        currentMission.name
    );


    setTimeout(() => {

        if (!currentMission)
            return;


        player.money +=
            currentMission.reward;


        log(
            "✅ Mission terminée ! +" +
            currentMission.reward +
            " €"
        );


        currentMission = null;


        updateUI();

        updateMissionUI();

    }, 10000);
}


/* =========================
   AFFICHAGE MISSION
========================= */

function updateMissionUI() {

    const missionBox =
        document.getElementById(
            "missionStatus"
        );

    if (!missionBox)
        return;


    if (currentMission) {

        missionBox.innerHTML =
            "📋 " +
            currentMission.name;

    } else {

        missionBox.innerHTML =
            "Aucune mission en cours";
    }
}


/* =========================
   RESTAURANT
========================= */

function eat() {

    if (player.money < 15) {

        log(
            "Tu n'as pas assez d'argent."
        );

        return;
    }


    if (player.hunger >= 100) {

        log(
            "Tu n'as pas faim."
        );

        return;
    }


    player.money -= 15;

    player.hunger += 30;


    if (player.hunger > 100)
        player.hunger = 100;


    updateUI();

    log(
        "🍔 Tu as mangé un repas. -15 €"
    );
}


/* =========================
   LOCATIONS
========================= */

function goLocation(location) {

    log(
        "Tu te rends au " +
        location +
        "."
    );


    if (location === "Hôpital") {

        if (player.money >= 50) {

            player.money -= 50;

            player.health = 100;

            log(
                "🏥 Le médecin t'a soigné. -50 €"
            );

        } else {

            log(
                "Tu n'as pas assez d'argent."
            );
        }
    }


    if (location === "Garage") {

        if (player.vehicle === "Aucun") {

            if (player.money >= 300) {

                player.money -= 300;

                player.vehicle =
                    "Petite voiture";

                log(
                    "🚗 Tu as acheté une voiture !"
                );

            } else {

                log(
                    "La voiture coûte 300 €."
                );
            }

        } else {

            log(
                "🚗 Ton véhicule : " +
                player.vehicle
            );
        }
    }


    if (location === "Commissariat") {

        if (player.job === "Policier") {

            log(
                "👮 Tu commences ton service de policier."
            );

        } else {

            log(
                "Le policier te regarde entrer."
            );
        }
    }


    if (location === "Mairie") {

        log(
            "🏛️ Bienvenue à la mairie de la ville."
        );
    }


    if (location === "Restaurant") {

        eat();
    }


    updateUI();
}


/* =========================
   SAUVEGARDE
========================= */

function saveGame() {

    const saveData = {

        player: player,

        currentMission: currentMission

    };


    localStorage.setItem(
        "villeRP",
        JSON.stringify(saveData)
    );


    log(
        "💾 Partie sauvegardée."
    );
}


/* =========================
   CHARGEMENT
========================= */

function loadGame() {

    const saved =
        localStorage.getItem("villeRP");


    if (!saved) {

        log(
            "❌ Aucune sauvegarde trouvée."
        );

        return;
    }


    try {

        const data =
            JSON.parse(saved);


        if (data.player) {

            player =
                data.player;

            currentMission =
                data.currentMission || null;

        } else {

            player = data;
        }


        updateUI();


        log(
            "📂 Partie chargée."
        );

    } catch {

        log(
            "❌ Impossible de charger la sauvegarde."
        );
    }
}


/* =========================
   MODAL
========================= */

function openModal(title, content) {

    document.getElementById(
        "modalTitle"
    ).textContent = title;


    document.getElementById(
        "modalContent"
    ).innerHTML = content;


    document.getElementById(
        "modal"
    ).style.display = "flex";
}


function closeModal() {

    document.getElementById(
        "modal"
    ).style.display = "none";
}


/* =========================
   SYSTÈME DE FAIM
========================= */

setInterval(() => {

    player.hunger -= 1;


    if (player.hunger < 0)
        player.hunger = 0;


    if (player.hunger === 0) {

        player.health -= 2;


        if (player.health < 0)
            player.health = 0;


        log(
            "🍔 Tu as très faim ! Ta santé diminue."
        );
    }


    updateUI();

}, 10000);


/* =========================
   DÉPLACEMENT
========================= */

document.addEventListener(
    "keydown",
    event => {

        const playerElement =
            document.getElementById("player");


        let left =
            parseFloat(
                playerElement.style.left
            ) || 50;


        let top =
            parseFloat(
                playerElement.style.top
            ) || 50;


        const speed = 2;


        if (event.key === "ArrowLeft")
            left -= speed;


        if (event.key === "ArrowRight")
            left += speed;


        if (event.key === "ArrowUp")
            top -= speed;


        if (event.key === "ArrowDown")
            top += speed;


        left =
            Math.max(
                2,
                Math.min(98, left)
            );


        top =
            Math.max(
                2,
                Math.min(98, top)
            );


        playerElement.style.left =
            left + "%";


        playerElement.style.top =
            top + "%";

    }
);


/* =========================
   DÉMARRAGE
========================= */

updateUI();


log(
    "🏙️ La ville est ouverte."
);


log(
    "🎮 Utilise les flèches du clavier pour te déplacer."
);


log(
    "💼 Choisis un métier et commence une mission !"
);

</script>

</body>
</html>
