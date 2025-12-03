<!DOCTYPE html>

<html lang="fr">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>TipShare - Calculateur de Pourboires</title>

    <style>

        /* * STYLES CSS INCLUS ICI (Pour la simplicité du fichier unique) */

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

        /* Conteneur de gestion des permissions (Mode Responsable) */

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

        /* ======================================= */

        /* STYLES POUR LE STATUT EN HAUT À DROITE */

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

        /* Styles de la zone Admin/Statut Responsable */

        #admin-status {

            font-weight: bold;

            padding: 3px 6px;

            border-radius: 3px;

            display: inline-block;

            background-color: #28a745;

            color: white;

            margin-right: 10px;

        }

        /* ======================================= */

        /* Styles pour les champs de saisie détaillés */

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

        /* Styles pour la liste des employés */

        #employee-list {

            margin-top: 15px;

            border-top: 1px solid #eee;

            padding-top: 10px;

        }

        .employee-input-group {

            /* 3 colonnes: Nom | Heures | Boutons */

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

        /* Styles pour la connexion */

        #login-section,

        #employee-input-section {

            border: 1px solid #ccc;

            padding: 15px;

            margin-bottom: 20px;

            border-radius: 8px;

        }

        /* Styles pour les résultats */

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

        /* Styles pour l'historique */

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

</head>

<body onload="loadDataAndInitialize()">

    <div class="container">

        <div id="header-status"></div>

        <h1>💰 TipShare : Calculateur de Pourboires par Heure</h1>

        <div id="login-section">

            <h2>👤 Connexion Employé</h2>

            <div id="login-form">

                <label for="login-name">Nom d'utilisateur :</label>

                <input type="text" id="login-name" placeholder="Votre nom" required>

                <label for="login-password">Mot de passe :</label>

                <input type="password" id="login-password" placeholder="Mot de passe" required>

                <button onclick="loginEmployee()" style="background-color: #28a745;">Se Connecter</button>

                <p style="text-align:center; margin-top: 10px;">

                    <button onclick="showRegisterForm()"

                        style="width: auto; background-color: #6c757d; padding: 8px 15px; font-size: 0.9em;">Créer un

                        Compte (Responsable requis)</button>

                </p>

            </div>

            <div id="register-form" style="display:none;">

                <h3>Créer un Compte Employé (Responsable Connecté Requis)</h3>

                <p style="font-size: 0.9em; color: #dc3545;">**Vous devez être connecté en mode Responsable pour créer

                    des comptes.**</p>

                <label for="register-name">Nom :</label>

                <input type="text" id="register-name" placeholder="Nom pour le compte" required>

                <label for="register-password">Mot de passe :</label>

                <input type="password" id="register-password" placeholder="Mot de passe pour le compte" required>

                <button onclick="registerEmployee()" style="background-color: #007bff;">Enregistrer le Compte</button>

                <p style="text-align:center; margin-top: 10px;">

                    <button onclick="hideRegisterForm()"

                        style="width: auto; background-color: #dc3545; padding: 8px 15px; font-size: 0.9em;">Annuler</button>

                </p>

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

                    <input type="number" id="total-tips" value="100" min="0" step="0.01" oninput="saveTotalTips()"

                        required>

                </div>

                <div id="tips-daily-inputs" style="display:none;" class="tips-input-detail">

                </div>

                <div id="tips-weekly-inputs" style="display:none;" class="tips-input-detail">

                </div>

                <p style="text-align: center; font-size: 0.9em; margin-top: 10px;">

                    Total consolidé : <strong id="consolidated-total">0.00 €</strong>

                </p>

            </div>

            <hr>

            <div class="section">

                <h2>3. 🧑‍🤝‍🧑 Liste des Employés Enregistrés</h2>

                <div id="employee-list">

                </div>

                <button type="button" id="add-employee-button" onclick="addEmployeeField()">➕ Ajouter un Employé</button>

            </div>

            <button type="button" onclick="calculateTips()">Calculer la Répartition (Responsable)</button>

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

            <div id="individual-shares">

            </div>

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

            <button id="clear-history-btn" type="button" onclick="clearHistory()"

                style="background-color: #f44336; display: none;">Effacer l'Historique</button>

        </div>

    </div>

    <script>

        // Clés de stockage

        const EMPLOYEE_ACCOUNTS_KEY = 'tipshare_employee_accounts';

        const EMPLOYEE_HOURS_KEY = 'tipshare_employee_hours';

        const LOGGED_IN_USER_KEY = 'tipshare_logged_in_user';

        const TIPS_STORAGE_KEY = 'tipshare_total_tips';

        const DAILY_TIPS_STORAGE_KEY = 'tipshare_daily_tips';

        const WEEKLY_TIPS_STORAGE_KEY = 'tipshare_weekly_tips';

        const PERIOD_STORAGE_KEY = 'tipshare_period';

        const HISTORY_STORAGE_KEY = 'tipshare_history';

        const ADMIN_STATE_KEY = 'tipshare_admin_active';

        const LAST_USERNAME_KEY = 'tipshare_last_username'; 

        // MOT DE PASSE SUPER ADMIN ET NOM D'UTILISATEUR ADMINISTRATEUR INTERNE

        const ADMIN_PASSWORD = '-Guigui951-'; 

        const ADMIN_USERNAME = 'Responsable'; // Nom d'utilisateur pour la connexion Responsable

        const daysOfWeek = ["Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi", "Dimanche"];

        const weeksOfMonth = ["Semaine 1", "Semaine 2", "Semaine 3", "Semaine 4", "Semaine 5"];

        // État global de l'utilisateur connecté

        let currentUser = null;

        // ----------------------------------------------------

        // INITIALISATION & DONNÉES DE BASE

        // ----------------------------------------------------

        function loadDataAndInitialize() {

            initializeEmployeeAccounts();

            const lastLoggedInUser = localStorage.getItem(LOGGED_IN_USER_KEY);

            const isAdminStored = localStorage.getItem(ADMIN_STATE_KEY) === 'true';

            // 1. Initialiser l'état à déconnecté par défaut

            currentUser = null;

            localStorage.setItem(ADMIN_STATE_KEY, 'false');

            

            // 2. Tenter de rétablir l'état si l'utilisateur était censé être connecté

            if (lastLoggedInUser) {

                // Pour l'Admin, on vérifie juste la clé stockée

                if (lastLoggedInUser === ADMIN_USERNAME && isAdminStored) {

                    currentUser = ADMIN_USERNAME;

                    localStorage.setItem(ADMIN_STATE_KEY, 'true');

                } 

                // Pour un employé standard, on s'assure que le compte existe toujours

                else if (lastLoggedInUser !== ADMIN_USERNAME) {

                    const accounts = getEmployeeAccounts();

                    const employeeExists = accounts.some(acc => acc.name === lastLoggedInUser);

                    if (employeeExists) {

                        currentUser = lastLoggedInUser;

                        localStorage.setItem(ADMIN_STATE_KEY, 'false');

                    } else {

                        // L'employé n'existe plus, forcer la déconnexion

                        localStorage.removeItem(LOGGED_IN_USER_KEY);

                    }

                }

            }

            

            // 3. Mettre à jour l'UI

            updateLoginUI(false); 

            checkAdminMode(false); 

            loadTotalTips();

            loadHistory();

            updateInputFields();

            

            // Pré-remplir le dernier nom d'utilisateur (pour faciliter la reconnexion)

            const lastUsername = localStorage.getItem(LAST_USERNAME_KEY);

            if (lastUsername) {

                document.getElementById('login-name').value = lastUsername;

            }

        }

        /**

         * Initialise un compte par défaut si la liste est vide.

         */

        function initializeEmployeeAccounts() {

            let accountsJSON = localStorage.getItem(EMPLOYEE_ACCOUNTS_KEY);

            if (!accountsJSON) {

                // Création d'un compte de base si c'est la première exécution

                const defaultAccounts = [

                    { name: "Antoine", password: "123" },

                    { name: "Béatrice", password: "456" },

                    { name: "Cédric", password: "789" }

                ];

                localStorage.setItem(EMPLOYEE_ACCOUNTS_KEY, JSON.stringify(defaultAccounts));

                // Initialisation des heures par défaut (pour la première utilisation)

                const defaultHours = {

                    "Antoine": 40,

                    "Béatrice": 30,

                    "Cédric": 10

                };

                localStorage.setItem(EMPLOYEE_HOURS_KEY, JSON.stringify(defaultHours));

            }

        }

        /**

         * Récupère la liste des comptes employés.

         */

        function getEmployeeAccounts() {

            const accountsJSON = localStorage.getItem(EMPLOYEE_ACCOUNTS_KEY);

            return accountsJSON ? JSON.parse(accountsJSON) : [];

        }

        /**

         * Récupère les heures enregistrées pour tous les employés.

         */

        function getEmployeeHours() {

            const hoursJSON = localStorage.getItem(EMPLOYEE_HOURS_KEY);

            return hoursJSON ? JSON.parse(hoursJSON) : {};

        }

        // ----------------------------------------------------

        // GESTION DES SESSIONS (LOGIN/REGISTER)

        // ----------------------------------------------------

        function showRegisterForm() {

            if (currentUser !== ADMIN_USERNAME) {

                alert("Seul le Responsable (connecté via le nom d'utilisateur et mot de passe secret) peut créer de nouveaux comptes.");

                return;

            }

            document.getElementById('login-form').style.display = 'none';

            document.getElementById('register-form').style.display = 'block';

        }

        function hideRegisterForm() {

            document.getElementById('login-form').style.display = 'block';

            document.getElementById('register-form').style.display = 'none';

        }

        /**

         * Enregistre un nouvel employé (Responsable doit être connecté).

         */

        function registerEmployee() {

            if (currentUser !== ADMIN_USERNAME) {

                alert("Seul le Responsable peut créer de nouveaux comptes.");

                return;

            }

            const name = document.getElementById('register-name').value.trim();

            const password = document.getElementById('register-password').value;

            // 1. Vérification des champs requis

            if (!name || !password) {

                alert("Veuillez remplir le nom et le mot de passe du nouvel employé.");

                return;

            }

            

            // 2. Vérification de la nouvelle règle : Interdiction du nom ou mot de passe Responsable

            if (name.toLowerCase() === ADMIN_USERNAME.toLowerCase() || password === ADMIN_PASSWORD) {

                alert("Ce nom d'utilisateur ou ce mot de passe est réservé au compte Responsable.");

                return;

            }

            

            // 3. Vérification de l'unicité du nom d'utilisateur

            let accounts = getEmployeeAccounts();

            if (accounts.some(acc => acc.name.toLowerCase() === name.toLowerCase())) {

                alert(`Le nom d'utilisateur "${name}" existe déjà.`);

                return;

            }

            // 4. Enregistrement du compte

            accounts.push({ name: name, password: password });

            localStorage.setItem(EMPLOYEE_ACCOUNTS_KEY, JSON.stringify(accounts));

            // 5. Mise à jour de la liste Responsable

            loadEmployees();

            // 6. Notification 

            alert(`Compte "${name}" créé avec succès !

Nom d'utilisateur: ${name}

Mot de passe: ${password}`);

            // 7. Nettoyage des champs

            hideRegisterForm();

            document.getElementById('register-name').value = '';

            document.getElementById('register-password').value = '';

        }

        /**

         * Connecte un employé ou le Responsable.

         */

        function loginEmployee() {

            const name = document.getElementById('login-name').value.trim();

            const password = document.getElementById('login-password').value;

            const loginPasswordInput = document.getElementById('login-password');

            

            if (!name || !password) {

                 alert("Veuillez remplir le nom et le mot de passe.");

                 return;

            }

            // 1. CHECK FOR ADMIN LOGIN (NOM D'UTILISATEUR "Responsable" ET MOT DE PASSE "-Guigui951-")

            if (name.toLowerCase() === ADMIN_USERNAME.toLowerCase() && password === ADMIN_PASSWORD) {

                // Successful Admin Login - Priorité absolue

                localStorage.setItem(ADMIN_STATE_KEY, 'true');

                currentUser = ADMIN_USERNAME; 

                localStorage.setItem(LOGGED_IN_USER_KEY, currentUser);

                localStorage.setItem(LAST_USERNAME_KEY, name); 

                updateLoginUI(true); 

                

                loadEmployees(); 

                checkAdminMode(false);

                return;

            }

            

            // 2. CHECK FOR STANDARD EMPLOYEE LOGIN

            const accounts = getEmployeeAccounts();

            const employee = accounts.find(acc => acc.name.toLowerCase() === name.toLowerCase() && acc.password === password);

            if (employee) {

                // Successful Employee Login

                localStorage.setItem(ADMIN_STATE_KEY, 'false'); // S'assurer que le mode admin est désactivé

                currentUser = employee.name;

                localStorage.setItem(LOGGED_IN_USER_KEY, currentUser);

                localStorage.setItem(LAST_USERNAME_KEY, employee.name);

                updateLoginUI(true);

            } else {

                // 3. MESSAGE D'ERREUR CLAIR

                alert("Nom d'utilisateur ou mot de passe incorrect. Veuillez réessayer.");

                // Effacer le mot de passe pour des raisons de sécurité

                loginPasswordInput.value = '';

            }

        }

        /**

         * Déconnecte l'utilisateur courant.

         */

        function logoutEmployee() {

            // Désactiver le mode Admin au moment de la déconnexion

            localStorage.setItem(ADMIN_STATE_KEY, 'false');

            localStorage.removeItem(LOGGED_IN_USER_KEY);

            currentUser = null;

            updateLoginUI(true);

            // Recharger toutes les données pour l'interface de déconnexion

            loadEmployees(); 

            checkAdminMode(false);

        }

        /**

         * Met à jour l'interface de connexion/saisie des heures.

         */

        function updateLoginUI(shouldAlert = false) {

            const loginSection = document.getElementById('login-section');

            const employeeInputSection = document.getElementById('employee-input-section');

            const headerStatusDiv = document.getElementById('header-status');

            const loginForm = document.getElementById('login-form');

            const loginPasswordInput = document.getElementById('login-password');

            // Mise à jour du statut dans le header

            let headerContent = '';

            const adminActive = localStorage.getItem(ADMIN_STATE_KEY) === 'true';

            if (adminActive) {

                // Le statut Responsable est visible uniquement s'il est actif

                 headerContent += `<span id="admin-status">Mode Responsable ON</span>`;

            }

            if (currentUser) {

                // Employé ou Responsable connecté

                loginSection.style.display = 'none';

                employeeInputSection.style.display = 'block';

                headerContent += `

                    Connecté : <strong>${currentUser}</strong> 

                    <button onclick="logoutEmployee()" class="logout-button">Déconnexion</button>

                `;

                loadUserHours();

            } else {

                // Personne n'est connecté

                if (shouldAlert) {

                    alert("Déconnecté.");

                }

                loginSection.style.display = 'block';

                employeeInputSection.style.display = 'none';

                

                // Cacher le statut Responsable s'il était là

                headerContent = '';

                loginForm.style.display = 'block';

                document.getElementById('register-form').style.display = 'none';

                // Le nom reste, seul le mot de passe est effacé

                loginPasswordInput.value = '';

            }

            

            headerStatusDiv.innerHTML = headerContent;

            

            // Assure que le mode Responsable est géré

            checkAdminMode(false);

        }

        /**

         * Charge les heures de l'utilisateur connecté dans le champ de saisie.

         */

        function loadUserHours() {

            if (currentUser === ADMIN_USERNAME) {

                // Le Responsable n'a pas de champ de saisie d'heures

                document.getElementById('user-hours-input').value = '';

                document.getElementById('employee-input-section').style.display = 'none';

                return;

            }

            

            const allHours = getEmployeeHours();

            const hours = allHours[currentUser] || 0;

            document.getElementById('user-hours-input').value = hours > 0 ? hours : '';

            document.getElementById('employee-input-section').style.display = 'block';

        }

        /**

         * Sauvegarde les heures saisies par l'utilisateur connecté.

         */

        function saveUserHours() {

            if (!currentUser || currentUser === ADMIN_USERNAME) return;

            const hoursInput = document.getElementById('user-hours-input');

            const hours = parseFloat(hoursInput.value) || 0;

            let allHours = getEmployeeHours();

            allHours[currentUser] = hours;

            localStorage.setItem(EMPLOYEE_HOURS_KEY, JSON.stringify(allHours));

            // S'assurer que le mode Responsable affiche la liste mise à jour si actif

            if (localStorage.getItem(ADMIN_STATE_KEY) === 'true') {

                loadEmployees();

            }

        }

        // ----------------------------------------------------

        // GESTION DU MODE RESPONSABLE

        // ----------------------------------------------------

        

        /**

         * Met à jour l'interface utilisateur en fonction de l'état du mode Responsable.

         * Appelé après login/logout pour réinitialiser l'état.

         */

        function checkAdminMode(refreshEmployees = true) {

            const adminActive = localStorage.getItem(ADMIN_STATE_KEY) === 'true';

            const controlsFieldset = document.getElementById('admin-controls-container');

            // Active/Désactive la zone de contrôle principale du Responsable

            controlsFieldset.disabled = !adminActive; 

            // Rafraîchir l'affichage des employés pour appliquer 'disabled' et afficher les heures

            if (refreshEmployees) {

                loadEmployees(); 

            }

            // Rafraîchir l'historique pour activer/désactiver les boutons de suppression

            loadHistory();

        }

        // ----------------------------------------------------

        // GESTION DES EMPLOYÉS (Liste Responsable)

        // ----------------------------------------------------

        /**

         * Demande un nouveau mot de passe au Responsable et le met à jour.

         */

        function promptForNewPassword(employeeName) {

            if (localStorage.getItem(ADMIN_STATE_KEY) !== 'true') return;

            const newPassword = prompt(`Définir un nouveau mot de passe pour l'employé "${employeeName}" :`);

            if (newPassword === null) {

                // L'utilisateur a cliqué sur Annuler

                return;

            }

            const trimmedPassword = newPassword.trim();

            if (trimmedPassword.length === 0) {

                alert("Le mot de passe ne peut pas être vide.");

                return;

            }

            // Vérification de la règle de sécurité (interdiction du mot de passe Admin)

            if (trimmedPassword === ADMIN_PASSWORD) {

                alert("Ce mot de passe est réservé au compte Responsable.");

                return;

            }

            // Appel à la fonction de sauvegarde avec le nouveau mot de passe

            saveEmployees(employeeName, trimmedPassword);

            alert(`Le mot de passe de l'employé "${employeeName}" a été mis à jour avec succès.`);

        }

        

        /**

         * Fonction de sauvegarde des comptes employés (nom et mdp).

         * Peut être utilisée par l'Admin pour modifier un mot de passe spécifique.

         * @param {string} [updateName=null] - Nom de l'employé à mettre à jour (pour le mot de passe).

         * @param {string} [newPassword=null] - Nouveau mot de passe à définir.

         */

        function saveEmployees(updateName = null, newPassword = null) {

            const employeeGroups = document.querySelectorAll('.employee-input-group');

            let accounts = getEmployeeAccounts();

            let newAccounts = [];

            employeeGroups.forEach(group => {

                const nameInput = group.querySelector('.employee-name');

                const name = nameInput ? nameInput.value.trim() : '';

                if (name.length > 0) {

                    // Trouver l'ancien mot de passe pour le conserver

                    let existingAccount = accounts.find(acc => acc.name === name);

                    let password = existingAccount ? existingAccount.password : '';

                    

                    // Si on met à jour un mot de passe spécifique

                    if (updateName && newPassword && name === updateName) {

                        password = newPassword;

                    }

                    // S'assurer qu'on n'ajoute pas de doublons dans la nouvelle liste

                    if (!newAccounts.some(acc => acc.name === name)) {

                        newAccounts.push({ name, password });

                    }

                }

            });

            

            localStorage.setItem(EMPLOYEE_ACCOUNTS_KEY, JSON.stringify(newAccounts));

        }

        

        /**

         * Sauvegarde les heures d'un employé spécifique, utilisée par l'Admin.

         * @param {string} employeeName - Le nom de l'employé dont on met à jour les heures.

         * @param {HTMLInputElement} inputElement - L'élément input d'où vient la valeur.

         */

        function saveEmployeeHoursByAdmin(employeeName, inputElement) {

            if (localStorage.getItem(ADMIN_STATE_KEY) !== 'true') return;

            const hours = parseFloat(inputElement.value) || 0;

            

            let allHours = getEmployeeHours();

            

            // Correction : S'assurer que l'objet existe si l'employé n'avait pas d'heures avant.

            allHours[employeeName] = hours;

            

            localStorage.setItem(EMPLOYEE_HOURS_KEY, JSON.stringify(allHours));

            

            // Optionnel: Mettre à jour l'input pour retirer le zéro si la valeur est 0, ou laisser le nombre.

            inputElement.value = hours > 0 ? hours : '';

        }

        /**

         * Crée un champ d'affichage des employés pour le Responsable.

         */

        function createEmployeeField(name = '', hours = 0) {

            const list = document.getElementById('employee-list');

            const newGroup = document.createElement('div');

            newGroup.className = 'employee-input-group';

            const isAdmin = localStorage.getItem(ADMIN_STATE_KEY) === 'true';

            // Afficher la valeur ou laisser vide si 0

            const hoursFormatted = hours > 0 ? hours : '';

            

            // Les champs ne sont cliquables qu'en mode Responsable

            const disabledAttribute = isAdmin ? '' : 'disabled';

            const removeButtonDisabled = isAdmin ? '' : 'disabled';

            

            // Le champ des heures est un <input type="number"> et est modifiable par l'Admin

            const hoursInputHTML = `

                <input type="number" 

                    value="${hoursFormatted}" 

                    placeholder="0"

                    id="hours-input-${name.replace(/\s/g, '-')}"

                    class="employee-hours" 

                    min="0" 

                    step="0.5"

                    oninput="saveEmployeeHoursByAdmin('${name}', this)"

                    ${disabledAttribute}

                    style="background-color: ${isAdmin ? '#fff' : '#f0f0f0'}; padding-right: 2px; text-align: right;">

            `;

            

            // Bouton pour la modification du mot de passe

            const passwordButtonHTML = isAdmin ? 

                `<button type="button" 

                    onclick="promptForNewPassword('${name}')" 

                    style="background-color: #ffc107; color: #343a40; padding: 5px 10px; width: auto; font-size: 0.8em; margin: 0;">

                    Modifier MDP

                </button>` : '';

            newGroup.innerHTML = `

                <input type="text" placeholder="Nom de l'employé" value="${name}" class="employee-name"

                    oninput="saveEmployees()" required ${disabledAttribute}>

                ${hoursInputHTML}

                <div style="display: flex; align-items: center; justify-content: flex-end;">

                    ${passwordButtonHTML}

                    <button class="remove-btn" onclick="removeEmployee('${name}', this.parentNode.parentNode)" ${removeButtonDisabled}>X</button>

                </div>

            `;

            list.appendChild(newGroup);

        }

        /**

         * Supprime un employé du système (Responsable uniquement).

         */

        function removeEmployee(name, node) {

            if (localStorage.getItem(ADMIN_STATE_KEY) !== 'true') return;

            if (!confirm(`Êtes-vous sûr de vouloir supprimer l'employé ${name} ? Cela effacera aussi son mot de passe et ses heures enregistrées.`)) {

                return;

            }

            // 1. Supprimer le compte

            let accounts = getEmployeeAccounts();

            accounts = accounts.filter(acc => acc.name !== name);

            localStorage.setItem(EMPLOYEE_ACCOUNTS_KEY, JSON.stringify(accounts));

            // 2. Supprimer les heures

            let allHours = getEmployeeHours();

            delete allHours[name];

            localStorage.setItem(EMPLOYEE_HOURS_KEY, JSON.stringify(allHours));

            // 3. Supprimer l'élément de l'interface

            node.remove();

        }

        function addEmployeeField() {

            if (localStorage.getItem(ADMIN_STATE_KEY) !== 'true') return;

            alert("Ajoutez le nom dans le champ ci-dessous. Vous pourrez définir le mot de passe via l'onglet 'Créer un Compte' une fois connecté.");

            createEmployeeField('', 0);

            const lastInput = document.querySelector('#employee-list .employee-input-group:last-child .employee-name');

            if (lastInput) lastInput.focus();

        }

        function loadEmployees() {

            const accounts = getEmployeeAccounts();

            const allHours = getEmployeeHours();

            const list = document.getElementById('employee-list');

            list.innerHTML = '';

            if (accounts.length === 0) {

                if (localStorage.getItem(ADMIN_STATE_KEY) === 'true') {

                    createEmployeeField('', 0);

                }

                return;

            }

            accounts.forEach(account => {

                const hours = allHours[account.name] || 0;

                createEmployeeField(account.name, hours);

            });

        }

        // ----------------------------------------------------

        // GESTION DU MONTANT DES POURBOIRES & HISTORIQUE

        // ----------------------------------------------------

        function updateTipsLabel(period) {

            const labelSpan = document.getElementById('total-tips-label');

            let text = 'Montant total des pourboires (€) :';

            switch (period) {

                case 'semaine':

                    text = 'Montant total des pourboires (Semaine) (€) :';

                    break;

                case 'mois':

                    text = 'Montant total des pourboires (Mois) (€) :';

                    break;

                case 'jour':

                default:

                    text = 'Montant total des pourboires (Jour) (€) :';

                    break;

            }

            if (labelSpan) {

                labelSpan.textContent = text;

            }

        }

        function updateInputFields() {

            const period = document.getElementById('tips-period').value;

            const simpleInputDiv = document.getElementById('simple-tips-input');

            const dailyInputsDiv = document.getElementById('tips-daily-inputs');

            const weeklyInputsDiv = document.getElementById('tips-weekly-inputs');

            dailyInputsDiv.style.display = 'none';

            weeklyInputsDiv.style.display = 'none';

            simpleInputDiv.style.display = 'block';

            if (period === 'jour') {

                loadTotalTips();

            } else if (period === 'semaine') {

                simpleInputDiv.style.display = 'none';

                dailyInputsDiv.style.display = 'grid';

                loadDailyTips();

            } else if (period === 'mois') {

                simpleInputDiv.style.display = 'none';

                weeklyInputsDiv.style.display = 'grid';

                loadWeeklyTips();

            }

            updateTipsLabel(period);

            updatePeriodLabel(period);

            calculateConsolidatedTotal();

        }

        function calculateConsolidatedTotal() {

            const period = document.getElementById('tips-period').value;

            let total = 0;

            if (period === 'jour') {

                total = parseFloat(document.getElementById('total-tips').value) || 0;

            } else if (period === 'semaine') {

                const dailyInputs = document.querySelectorAll('#tips-daily-inputs input');

                dailyInputs.forEach(input => {

                    total += parseFloat(input.value) || 0;

                });

            } else if (period === 'mois') {

                const weeklyInputs = document.querySelectorAll('#tips-weekly-inputs input');

                weeklyInputs.forEach(input => {

                    total += parseFloat(input.value) || 0;

                });

            } else {

                total = parseFloat(document.getElementById('total-tips').value) || 0;

            }

            document.getElementById('consolidated-total').textContent = total.toFixed(2) + ' €';

            return total;

        }

        function saveDailyTips() {

            const dailyInputs = document.querySelectorAll('#tips-daily-inputs input');

            const tips = Array.from(dailyInputs).map(input => parseFloat(input.value) || 0);

            localStorage.setItem(DAILY_TIPS_STORAGE_KEY, JSON.stringify(tips));

            calculateConsolidatedTotal();

        }

        function loadDailyTips() {

            const container = document.getElementById('tips-daily-inputs');

            container.innerHTML = '';

            const savedTipsJSON = localStorage.getItem(DAILY_TIPS_STORAGE_KEY);

            const savedTips = savedTipsJSON ? JSON.parse(savedTipsJSON) : Array(7).fill('');

            daysOfWeek.forEach((day, index) => {

                const dayValue = savedTips[index] !== 0 ? savedTips[index] : '';

                const label = document.createElement('label');

                label.setAttribute('for', `tip-day-${index}`);

                label.textContent = `${day} (€) :`;

                const input = document.createElement('input');

                input.type = 'number';

                input.id = `tip-day-${index}`;

                input.value = dayValue;

                input.min = '0';

                input.step = '0.01';

                input.oninput = saveDailyTips;

                input.required = true;

                container.appendChild(label);

                container.appendChild(input);

            });

            calculateConsolidatedTotal();

        }

        function saveWeeklyTips() {

            const weeklyInputs = document.querySelectorAll('#tips-weekly-inputs input');

            const tips = Array.from(weeklyInputs).map(input => parseFloat(input.value) || 0);

            localStorage.setItem(WEEKLY_TIPS_STORAGE_KEY, JSON.stringify(tips));

            calculateConsolidatedTotal();

        }

        function loadWeeklyTips() {

            const container = document.getElementById('tips-weekly-inputs');

            container.innerHTML = '';

            const savedTipsJSON = localStorage.getItem(WEEKLY_TIPS_STORAGE_KEY);

            const savedTips = savedTipsJSON ? JSON.parse(savedTipsJSON) : Array(5).fill('');

            weeksOfMonth.forEach((week, index) => {

                const weekValue = savedTips[index] !== 0 ? savedTips[index] : '';

                const label = document.createElement('label');

                label.setAttribute('for', `tip-week-${index}`);

                label.textContent = `${week} (€) :`;

                const input = document.createElement('input');

                input.type = 'number';

                input.id = `tip-week-${index}`;

                input.value = weekValue;

                input.min = '0';

                input.step = '0.01';

                input.oninput = saveWeeklyTips;

                input.required = true;

                container.appendChild(label);

                container.appendChild(input);

            });

            calculateConsolidatedTotal();

        }

        function saveTotalTips() {

            const totalTips = document.getElementById('total-tips').value;

            const period = document.getElementById('tips-period').value;

            localStorage.setItem(TIPS_STORAGE_KEY, totalTips);

            localStorage.setItem(PERIOD_STORAGE_KEY, period);

            updateTipsLabel(period);

            updatePeriodLabel(period);

            calculateConsolidatedTotal();

        }

        function loadTotalTips() {

            const savedTips = localStorage.getItem(TIPS_STORAGE_KEY);

            const savedPeriod = localStorage.getItem(PERIOD_STORAGE_KEY) || 'jour';

            if (savedTips) {

                document.getElementById('total-tips').value = savedTips;

            } else {

                document.getElementById('total-tips').value = 100;

            }

            document.getElementById('tips-period').value = savedPeriod;

            updateTipsLabel(savedPeriod);

            updatePeriodLabel(savedPeriod);

        }

        function updatePeriodLabel(period) {

            const label = document.getElementById('period-label');

            let text = 'Total des Heures Travaillées :';

            switch (period) {

                case 'semaine':

                    text = 'Total des Heures Travaillées (Semaine) :';

                    break;

                case 'mois':

                    text = 'Total des Heures Travaillées (Mois) :';

                    break;

                case 'jour':

                default:

                    text = 'Total des Heures Travaillées (Jour) :';

                    break;

            }

            if (label) {

                label.textContent = text;

            }

        }

        function loadHistory() {

            const list = document.getElementById('history-list');

            const historyJSON = localStorage.getItem(HISTORY_STORAGE_KEY);

            let history = historyJSON ? JSON.parse(historyJSON) : [];

            list.innerHTML = '';

            const isAdmin = localStorage.getItem(ADMIN_STATE_KEY) === 'true';

            const noHistoryMessage = document.getElementById('no-history-message');

            const clearHistoryBtn = document.getElementById('clear-history-btn');

            if (history.length === 0) {

                if (noHistoryMessage) noHistoryMessage.style.display = 'block';

                if (clearHistoryBtn) clearHistoryBtn.style.display = 'none';

                return;

            }

            if (noHistoryMessage) noHistoryMessage.style.display = 'none';

            if (clearHistoryBtn) clearHistoryBtn.style.display = 'block';

            history.forEach((entry, entryIndex) => {

                const entryDiv = document.createElement('div');

                entryDiv.className = 'history-entry';

                const sharesList = entry.shares.map((s, shareIndex) => {

                    const isPaid = s.paid === undefined ? false : s.paid;

                    const isPaidClass = isPaid ? 'is-paid' : '';

                    return `

                        <li class="share-item-paid ${isPaidClass}">

                            <span>${s.name}: <strong>${s.share} €</strong></span>

                            <label style="font-weight: normal; margin-bottom: 0;">

                                <input type="checkbox"

                                    onclick="togglePaid(${entryIndex}, ${shareIndex})"

                                    ${isPaid ? 'checked' : ''} ${isAdmin ? '' : 'disabled'}> Récupéré

                            </label>

                        </li>`;

                }).join('');

                entryDiv.innerHTML = `

                    <div class="history-header">

                        <span>${entry.timestamp}</span>

                        <button class="history-delete-btn" onclick="deleteHistoryEntry(${entryIndex})" ${isAdmin ? '' : 'disabled'}>Supprimer</button>

                    </div>

                    <div class="history-details">

                        Montant Total : <strong>${entry.totalTips} €</strong> |

                        Total Heures : <strong>${entry.totalHours}h</strong> |

                        Taux : <strong>${entry.tipRate} €/h</strong>

                        <ul style="list-style: none; padding-left: 0; margin-top: 5px; border-top: 1px dotted #ccc; padding-top: 5px;">

                            ${sharesList}

                        </ul>

                    </div>

                `;

                list.appendChild(entryDiv);

            });

            if (clearHistoryBtn) clearHistoryBtn.disabled = !isAdmin;

        }

        function saveHistory(totalTips, totalHours, tipRate, shares) {

            const historyJSON = localStorage.getItem(HISTORY_STORAGE_KEY);

            let history = historyJSON ? JSON.parse(historyJSON) : [];

            const now = new Date();

            const timestamp = now.toLocaleDateString('fr-FR') + ' ' + now.toLocaleTimeString('fr-FR');

            // Formatage des données

            const formattedShares = shares.map(s => ({

                name: s.name,

                share: s.share.toFixed(2),

                paid: false // Nouvelle entrée est non payée

            }));

            const newEntry = {

                timestamp: timestamp,

                totalTips: totalTips.toFixed(2),

                totalHours: totalHours.toFixed(1),

                tipRate: tipRate.toFixed(2),

                shares: formattedShares

            };

            history.unshift(newEntry); // Ajoute au début

            localStorage.setItem(HISTORY_STORAGE_KEY, JSON.stringify(history));

            loadHistory();

        }

        function togglePaid(entryIndex, shareIndex) {

            // S'assurer que seul le Responsable peut modifier l'état de "payé"

            if (localStorage.getItem(ADMIN_STATE_KEY) !== 'true') return;

            const historyJSON = localStorage.getItem(HISTORY_STORAGE_KEY);

            let history = historyJSON ? JSON.parse(historyJSON) : [];

            if (history[entryIndex] && history[entryIndex].shares[shareIndex]) {

                history[entryIndex].shares[shareIndex].paid = !history[entryIndex].shares[shareIndex].paid;

                localStorage.setItem(HISTORY_STORAGE_KEY, JSON.stringify(history));

                loadHistory();

            }

        }

        function deleteHistoryEntry(index) {

            if (localStorage.getItem(ADMIN_STATE_KEY) !== 'true') return;

            if (!confirm("Voulez-vous vraiment supprimer cet enregistrement unique de l'historique ?")) {

                return;

            }

            const historyJSON = localStorage.getItem(HISTORY_STORAGE_KEY);

            let history = historyJSON ? JSON.parse(historyJSON) : [];

            if (index >= 0 && index < history.length) {

                history.splice(index, 1);

                localStorage.setItem(HISTORY_STORAGE_KEY, JSON.stringify(history));

                loadHistory();

            }

        }

        function clearHistory() {

            if (localStorage.getItem(ADMIN_STATE_KEY) !== 'true') return;

            const confirmationKey = 'SUPPRIMER';

            if (!confirm("Êtes-vous sûr de vouloir effacer tout l'historique des répartitions ? Cette action est IRREVERSIBLE.")) {

                return;

            }

            const userConfirmation = prompt(`Pour confirmer la suppression complète, veuillez taper le mot-clé suivant : "${confirmationKey}"`);

            if (userConfirmation === confirmationKey) {

                localStorage.removeItem(HISTORY_STORAGE_KEY);

                loadHistory();

                alert("L'historique a été complètement effacé.");

            } else if (userConfirmation !== null) {

                alert("Erreur de confirmation : L'historique n'a PAS été effacé. Le mot-clé saisi ne correspond pas.");

            }

        }

        // ----------------------------------------------------

        // FONCTION PRINCIPALE DE CALCUL

        // ----------------------------------------------------

        function calculateTips() {

            if (localStorage.getItem(ADMIN_STATE_KEY) !== 'true') {

                alert("Seul le Responsable peut effectuer le calcul de répartition.");

                return;

            }

            // Pas besoin d'appeler saveEmployees() ici car les modifications se font oninput ou par promptForNewPassword

            // saveEmployees(); 

            const totalTips = calculateConsolidatedTotal();

            const allHours = getEmployeeHours();

            const accounts = getEmployeeAccounts();

            const individualSharesDiv = document.getElementById('individual-shares');

            let totalHours = 0;

            individualSharesDiv.innerHTML = '';

            document.getElementById('results').style.display = 'none';

            if (isNaN(totalTips) || totalTips <= 0) {

                alert("Veuillez entrer un montant total de pourboires valide et supérieur à zéro.");

                return;

            }

            const employees = [];

            // Collecte les heures de tous les employés enregistrés

            accounts.forEach(account => {

                const hours = allHours[account.name] || 0;

                // Le Responsable n'a pas d'heures à comptabiliser

                if (hours > 0 && account.name !== ADMIN_USERNAME) { 

                    employees.push({ name: account.name, hours: hours });

                    totalHours += hours;

                }

            });

            if (employees.length === 0 || totalHours === 0) {

                alert("Aucun employé n'a saisi d'heures travaillées pour cette période (ou le total des heures est zéro).");

                return;

            }

            const tipRatePerHour = totalTips / totalHours;

            let distributedTotal = 0;

            const individualSharesData = [];

            employees.forEach(employee => {

                const share = employee.hours * tipRatePerHour;

                const shareFormatted = share.toFixed(2);

                distributedTotal += share;

                individualSharesData.push({ name: employee.name, share: share });

                const shareItem = document.createElement('div');

                shareItem.className = 'result-item';

                shareItem.innerHTML = `

                    <span>${employee.name} (${employee.hours.toFixed(1)}h) :</span>

                    <span class="highlight">${shareFormatted} €</span>

                `;

                individualSharesDiv.appendChild(shareItem);

            });

            // Affichage des totaux

            document.getElementById('total-hours-output').textContent = totalHours.toFixed(1) + ' heures';

            document.getElementById('tip-rate-output').textContent = tipRatePerHour.toFixed(2) + ' €/heure';

            document.getElementById('distributed-total').textContent = distributedTotal.toFixed(2) + ' €';

            document.getElementById('results').style.display = 'block';

            // Sauvegarder ce calcul dans l'historique

            saveHistory(totalTips, totalHours, tipRatePerHour, individualSharesData);

        }

    </script>

</body>

</html>
