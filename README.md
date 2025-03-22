<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planning Sécurité</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f4f4f4;
        }
        .container {
            max-width: 800px;
            margin: 50px auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
        }
        input, button, select {
            margin: 10px;
            padding: 10px;
            width: 80%;
        }
        .hidden {
            display: none;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ccc;
            padding: 10px;
            text-align: center;
        }
        th {
            background: #bbb;
        }
        .notification {
            background-color: #4CAF50;
            color: white;
            padding: 10px;
            margin-top: 20px;
            border-radius: 5px;
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Page de connexion -->
        <div id="loginPage">
            <h2>Connexion</h2>
            <input type="text" id="loginInput" placeholder="Code d'accès ou mot de passe admin">
            <button onclick="login()">Se connecter</button>
        </div>

        <!-- Page du planning pour les agents (cachée par défaut) -->
        <div id="planningPage" class="hidden">
            <h2>Planning Sécurité</h2>
            <div id="notification" class="notification">Le planning a été mis à jour !</div>
            <table>
                <tr>
                    <th>Lundi</th>
                    <th>Mardi</th>
                    <th>Mercredi</th>
                    <th>Jeudi</th>
                    <th>Vendredi</th>
                    <th>Samedi</th>
                </tr>
                <tr>
                    <td id="monTime"></td>
                    <td id="tueTime"></td>
                    <td id="wedTime"></td>
                    <td id="thuTime"></td>
                    <td id="friTime"></td>
                    <td id="satTime"></td>
                </tr>
                <tr>
                    <td id="monSec"></td>
                    <td id="tueSec"></td>
                    <td id="wedSec"></td>
                    <td id="thuSec"></td>
                    <td id="friSec"></td>
                    <td id="satSec"></td>
                </tr>
            </table>
            <button onclick="logout()">Se déconnecter</button>
        </div>

        <!-- Page admin pour modifier le planning (cachée par défaut) -->
        <div id="adminPage" class="hidden">
            <h2>Modifier le Planning</h2>
            <select id="daySelect">
                <option value="mon">Lundi</option>
                <option value="tue">Mardi</option>
                <option value="wed">Mercredi</option>
                <option value="thu">Jeudi</option>
                <option value="fri">Vendredi</option>
                <option value="sat">Samedi</option>
            </select>
            <input type="text" id="timeInput" placeholder="Heure (ex: 18h-22h)">
            <input type="text" id="nameInput" placeholder="Nom de l'agent">
            <button onclick="updatePlanning()">Mettre à jour</button>
            <button onclick="logout()">Se déconnecter</button>
        </div>
    </div>

    <script>
        // Codes d'accès
        const agentAccessCode = "STL22"; // Code pour les agents
        const adminPassword = "Nykho11&-"; // Mot de passe admin

        // Charger les données du planning depuis localStorage
        let planningData = JSON.parse(localStorage.getItem("planningData")) || {
            monTime: "18h-22h",
            tueTime: "19h-23h",
            wedTime: "20h-00h",
            thuTime: "18h-22h",
            friTime: "17h-21h",
            satTime: "16h-20h",
            monSec: "Jean Dupont",
            tueSec: "Marie Curie",
            wedSec: "Paul Martin",
            thuSec: "Lucie Bernard",
            friSec: "Pierre Durand",
            satSec: "Sophie Leroy"
        };

        // Fonction de connexion
        function login() {
            let enteredCode = document.getElementById("loginInput").value;
            if (enteredCode === agentAccessCode) {
                // Connexion en tant qu'agent
                document.getElementById("loginPage").classList.add("hidden");
                document.getElementById("planningPage").classList.remove("hidden");
                loadPlanning();
            } else if (enteredCode === adminPassword) {
                // Connexion en tant qu'admin
                document.getElementById("loginPage").classList.add("hidden");
                document.getElementById("adminPage").classList.remove("hidden");
            } else {
                alert("Code ou mot de passe incorrect");
            }
        }

        // Fonction pour charger le planning
        function loadPlanning() {
            for (const key in planningData) {
                document.getElementById(key).textContent = planningData[key];
            }
        }

        // Fonction pour mettre à jour le planning (admin seulement)
        function updatePlanning() {
            let selectedDay = document.getElementById("daySelect").value;
            let time = document.getElementById("timeInput").value;
            let name = document.getElementById("nameInput").value;

            // Mettre à jour les données
            planningData[selectedDay + "Time"] = time;
            planningData[selectedDay + "Sec"] = name;

            // Sauvegarder dans localStorage
            localStorage.setItem("planningData", JSON.stringify(planningData));

            // Afficher une notification
            showNotification("Le planning a été mis à jour !");

            // Recharger le planning
            loadPlanning();

            // Réinitialiser les champs
            document.getElementById("timeInput").value = "";
            document.getElementById("nameInput").value = "";
        }

        // Fonction pour afficher une notification
        function showNotification(message) {
            let notification = document.getElementById("notification");
            notification.textContent = message;
            notification.style.display = "block";
            setTimeout(() => {
                notification.style.display = "none";
            }, 3000); // La notification disparaît après 3 secondes
        }

        // Fonction de déconnexion
        function logout() {
            document.getElementById("loginPage").classList.remove("hidden");
            document.getElementById("planningPage").classList.add("hidden");
            document.getElementById("adminPage").classList.add("hidden");
            document.getElementById("loginInput").value = "";
        }

        // Charger le planning au démarrage (si déjà connecté)
        if (localStorage.getItem("planningData")) {
            loadPlanning();
        }
    </script>
</body>
</html>
