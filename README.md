<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TipShare - Calculateur de Pourboires</title>
    <style>
        /* Styles CSS */
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f4f7f6;
            margin: 0;
            padding: 20px;
            color: #333;
        }
        .container {
            position: relative;
            max-width: 700px;
            margin: auto;
            background: #ffffff;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        h1 {
            color: #4CAF50;
            text-align: center;
            margin-bottom: 25px;
        }
        .section {
            border: 1px solid #ddd;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 8px;
            background-color: #fafafa;
        }
        .admin-controls-container input:disabled,
        .admin-controls-container select:disabled,
        .admin-controls-container button:disabled:not(.remove-btn) {
            background-color: #e9ecef;
            cursor: not-allowed;
            border-color: #ced4da;
            opacity: 0.8;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input[type="number"],
        input[type="text"],
        input[type="password"],
        input[type="email"],
        select {
            width: calc(100% - 12px);
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 12px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
            transition: background-color 0.3s;
            margin-top: 5px;
        }
        button:hover {
            background-color: #45a049;
        }
        #header-status {
            position: absolute;
            top: 20px;
            right: 20px;
            text-align: right;
            font-size: 0.95em;
            padding: 5px 10px;
            background-color: #e8f5e9;
            border-radius: 5px;
            border: 1px solid #4CAF50;
            z-index: 10;
        }
        #header-status strong {
            color: #4CAF50;
            font-size: 1.1em;
            margin-right: 10px;
        }
        #header-status .logout-button {
            background-color: #dc3545;
            width: auto;
            padding: 4px 8px;
            font-size: 0.8em;
            margin: 0;
            margin-left: 10px;
            display: inline-block;
            vertical-align: middle;
        }
        #header-status .logout-button:hover {
            background-color: #c82333;
        }
        #admin-status {
            font-weight: bold;
            padding: 3px 6px;
            border-radius: 3px;
            display: inline-block;
            background-color: #28a745;
            color: white;
            margin-right: 10px;
        }
        .tips-input-detail {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 10px;
            padding: 10px;
            border: 1px dashed #4CAF50;
            border-radius: 5px;
            background-color: #f0fff0;
        }
        .tips-input-detail label {
            font-weight: normal;
        }
        .tips-input-detail input {
            margin-bottom: 0;
            width: 90%;
        }
        #employee-list {
            margin-top: 15px;
            border-top: 1px solid #eee;
            padding-top: 10px;
        }
        .employee-input-group {
            display: grid;
            grid-template-columns: 2fr 1fr 120px;
            gap: 10px;
            margin-bottom: 10px;
            align-items: center;
        }
        .employee-input-group input {
            margin-bottom: 0;
        }
        .remove-btn {
            background-color: #f44336;
            width: 35px;
            padding: 8px 0;
            font-size: 14px;
            margin-left: 5px;
        }
        .remove-btn:hover {
            background-color: #d32f2f;
        }
        #login-section,
        #employee-input-section {
            border: 1px solid #ccc;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 8px;
        }
        #results {
            margin-top: 25px;
            padding: 20px;
            border-top: 2px solid #4CAF50;
            background-color: #e8f5e9;
            border-radius: 8px;
        }
        #results h2 {
            color: #388e3c;
            margin-top: 0;
        }
        .result-item {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px dotted #ccc;
        }
        .result-item:last-child {
            border-bottom: none;
        }
        .highlight {
            font-weight: bold;
            color: #000;
        }
        .history-entry {
            border: 1px solid #ddd;
            padding: 10px;
            margin-bottom: 10px;
            border-radius: 5px;
            background-color: #fff;
            position: relative;
        }
        .history-header {
            font-weight: bold;
            color: #4CAF50;
            display: flex;
            justify-content: space-between;
        }
        .history-details {
            font-size: 0.9em;
            color: #555;
            margin-top: 5px;
        }
        .history-delete-btn {
            background-color: #f44336;
            color: white;
            padding: 5px 10px;
            border: none;
            border-radius: 3px;
            cursor: pointer;
            font-size: 0.8em;
            width: auto;
            margin: 0;
        }
        .share-item-paid {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding-right: 5px;
            margin-top: 3px;
        }
        .share-item-paid.is-paid {
            text-decoration: line-through;
            color: #888;
        }
        .share-item-paid label {
            margin-left: 5px;
            font-size: 0.95em;
        }
    </style>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-auth.js"></script>
    <script src="firebase-config.js"></script>
</head>
<body onload="loadDataAndInitialize()">
    <div class="container">
        <div id="header-status"></div>
        <h1>💰 TipShare : Calculateur de Pourboires par Heure</h1>
        <div id="login-section">
            <h2>👤 Connexion Employé</h2>
            <div id="login-form">
                <label for="login-email">Adresse e-mail :</label>
                <input type="email" id="login-email" placeholder="Votre e-mail" required>
                <label for="login-password">Mot de passe :</label>
                <input type="password" id="login-password" placeholder="Mot de passe" required>
                <button onclick="loginEmployee()">Se Connecter</button>
            </div>
        </div>
        <div id="employee-input-section" class="section" style="display:none;">
            <h2>2. ⏰ Mes Heures Travaillées</h2>
            <label for="user-hours-input">Heures travaillées pour cette période :</label>
            <input type="number" id="user-hours-input" min="0" step="0.5" oninput="saveUserHours()">
        </div>
        <hr>
        <fieldset id="admin-controls-container" class="admin-controls-container" disabled>
            <div class="section">
                <h2>1. 💶 Saisie du Montant Total</h2>
                <label for="tips-period">Période de pourboires :</label>
                <select id="tips-period" onchange="saveTotalTips(); updateInputFields()">
                    <option value="jour">Jour</option>
                    <option value="semaine">Semaine</option>
                    <option value="mois">Mois</option>
                </select>
                <div id="simple-tips-input">
                    <label for="total-tips">
                        <span id="total-tips-label">Montant total des pourboires (Jour) (€) :</span>
                    </label>
                    <input type="number" id="total-tips" value="100" min="0" step="0.01" oninput="saveTotalTips()" required>
                </div>
                <div id="tips-daily-inputs" style="display:none;" class="tips-input-detail"></div>
                <div id="tips-weekly-inputs" style="display:none;" class="tips-input-detail"></div>
                <p style="text-align: center; font-size: 0.9em; margin-top: 10px;">
                    Total consolidé : <strong id="consolidated-total">0.00 €</strong>
                </p>
            </div>
            <hr>
            <div class="section">
                <h2>3. 🧑‍🤝‍🧑 Liste des Employés Enregistrés</h2>
                <div id="employee-list"></div>
                <button type="button" id="add-employee-button" onclick="addEmployeeField()">➕ Ajouter un Employé</button>
                <button type="button" id="save-employee-list-button" onclick="alert('La gestion des employés se fait maintenant dans la console Firebase.')" style="background-color: #007bff; margin-top: 10px;">
                    💾 Enregistrer les modifications
                </button>
            </div>
            <button type="button" id="calculate-button" onclick="calculateTips()">Calculer la Répartition (Responsable)</button>
        </fieldset>
        <hr>
        <div id="results" style="display:none;">
            <h2>4. Résultats de la Répartition</h2>
            <div class="result-item">
                <span id="period-label">Total des Heures Travaillées :</span>
                <span id="total-hours-output" class="highlight">0</span>
            </div>
            <div class="result-item">
                <span>Taux de Pourboire par Heure :</span>
                <span id="tip-rate-output" class="highlight">0.00 €</span>
            </div>
            <hr style="margin: 10px 0;">
            <div id="individual-shares"></div>
            <hr style="margin: 10px 0;">
            <div class="result-item">
                <span>Total Distribué :</span>
                <span id="distributed-total" class="highlight">0.00 €</span>
            </div>
        </div>
        <div class="section">
            <h2>5. Historique des Répartitions 🕰️</h2>
            <div id="history-list">
                <p id="no-history-message">Aucun calcul n'a été enregistré.</p>
            </div>
            <button id="clear-history-btn" type="button" onclick="clearHistory()" style="background-color: #f44336; display: none;">Effacer l'Historique</button>
        </div>
    </div>
    <script>
        // Clés de stockage local
        const EMPLOYEE_HOURS_KEY = 'tipshare_employee_hours';
        const TIPS_STORAGE_KEY = 'tipshare_total_tips';
        const DAILY_TIPS_STORAGE_KEY = 'tipshare_daily_tips';
        const WEEKLY_TIPS_STORAGE_KEY = 'tipshare_weekly_tips';
        const PERIOD_STORAGE_KEY = 'tipshare_period';
        const HISTORY_STORAGE_KEY = 'tipshare_history';
        const ADMIN_STATE_KEY = 'tipshare_admin_active';

        const daysOfWeek = ["Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi", "Dimanche"];
        const weeksOfMonth = ["Semaine 1", "Semaine 2", "Semaine 3", "Semaine 4", "Semaine 5"];

        // --- Initialisation de Firebase et de l'authentification ---
        const auth = firebase.auth();
        let currentUser = null;

        // INITIALISATION
        function loadDataAndInitialize() {
            auth.onAuthStateChanged(user => {
                currentUser = user;
                if (user) {
                    user.getIdTokenResult().then(idTokenResult => {
                        const isAdmin = !!idTokenResult.claims.admin;
                        localStorage.setItem(ADMIN_STATE_KEY, isAdmin);
                        updateUIForUser(user.email, isAdmin);
                    });
                } else {
                    localStorage.setItem(ADMIN_STATE_KEY, 'false');
                    updateUIForUser(null, false);
                }
                loadTotalTips();
                loadHistory();
                updateInputFields();
            });
        }

        // GESTION DE L'AUTHENTIFICATION
        function loginEmployee() {
            const email = document.getElementById('login-email').value.trim();
            const password = document.getElementById('login-password').value;
            if (!email || !password) {
                alert("Veuillez remplir l'e-mail et le mot de passe.");
                return;
            }
            auth.signInWithEmailAndPassword(email, password).catch(error => {
                alert("Erreur de connexion : " + error.message);
                document.getElementById('login-password').value = '';
            });
        }

        function logoutEmployee() {
            auth.signOut();
        }
        
        // MISE À JOUR DE L'INTERFACE
        function updateUIForUser(userEmail, isAdmin) {
            const loginSection = document.getElementById('login-section');
            const employeeInputSection = document.getElementById('employee-input-section');
            const headerStatusDiv = document.getElementById('header-status');
            const adminControls = document.getElementById('admin-controls-container');

            headerStatusDiv.innerHTML = '';
            
            if (userEmail) {
                loginSection.style.display = 'none';
                let headerContent = `Connecté : <strong>${userEmail}</strong> <button onclick="logoutEmployee()" class="logout-button">Déconnexion</button>`;
                
                if (isAdmin) {
                    headerContent = `<span id="admin-status">Mode Responsable</span>` + headerContent;
                    employeeInputSection.style.display = 'none';
                    adminControls.disabled = false;
                    loadEmployees();
                } else {
                    employeeInputSection.style.display = 'block';
                    adminControls.disabled = true;
                    loadUserHours();
                }
                headerStatusDiv.innerHTML = headerContent;

            } else {
                loginSection.style.display = 'block';
                employeeInputSection.style.display = 'none';
                adminControls.disabled = true;
                document.getElementById('login-password').value = '';
            }
            loadHistory();
        }

        // GESTION DES HEURES
        function getEmployeeHours() {
            const hoursJSON = localStorage.getItem(EMPLOYEE_HOURS_KEY);
            return hoursJSON ? JSON.parse(hoursJSON) : {};
        }

        function loadUserHours() {
            if (!currentUser) return;
            const allHours = getEmployeeHours();
            const hours = allHours[currentUser.uid] || 0;
            document.getElementById('user-hours-input').value = hours > 0 ? hours : '';
        }

        function saveUserHours() {
            if (!currentUser) return;
            const hoursInput = document.getElementById('user-hours-input');
            const hours = parseFloat(hoursInput.value) || 0;
            let allHours = getEmployeeHours();
            allHours[currentUser.uid] = hours;
            localStorage.setItem(EMPLOYEE_HOURS_KEY, JSON.stringify(allHours));
        }

        // --- SECTION ADMIN ---
        function addEmployeeField() {
            alert("Pour ajouter un employé, veuillez le faire depuis la console Firebase (Authentication > Users > Add user).");
        }

        function loadEmployees() {
            const list = document.getElementById('employee-list');
            list.innerHTML = '<p>La liste des employés est gérée dans la console Firebase. Le calcul prendra en compte les heures de ceux qui en ont saisi.</p>';
        }

        // --- Fonctions utilitaires (à compléter) ---
        function calculateTips() { /* ... Votre logique de calcul ... */ }
        function loadHistory() { /* ... Votre logique d'historique ... */ }
        function saveTotalTips() { /* ... etc ... */ }
        function updateInputFields() { /* ... etc ... */ }
        function calculateConsolidatedTotal() { /* ... etc ... */ }
        function saveDailyTips() { /* ... etc ... */ }
        function loadDailyTips() { /* ... etc ... */ }
        function saveWeeklyTips() { /* ... etc ... */ }
        function loadWeeklyTips() { /* ... etc ... */ }
        function updateTipsLabel(period) { /* ... etc ... */ }
        function updatePeriodLabel(period) { /* ... etc ... */ }
        function clearHistory() { /* ... etc ... */ }

    </script>
</body>
</html>
