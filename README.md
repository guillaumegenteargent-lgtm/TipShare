
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TipShare - Calculateur de Pourboires</title>
    <style>
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
        input[type="number"], input[type="text"], select {
            width: calc(100% - 22px);
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
        }
        #admin-status {
            font-weight: bold;
            padding: 3px 6px;
            border-radius: 3px;
            display: inline-block;
            background-color: #28a745;
            color: white;
        }
        #login-button {
            width: auto;
            padding: 8px 15px;
            background-color: #007bff;
            font-size: 0.9em;
        }
        #login-button.logout {
            background-color: #dc3545;
        }
        .employee-input-group {
            display: grid;
            grid-template-columns: 2fr 1fr 40px;
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
            height: 35px;
            padding: 0;
            font-size: 18px;
            line-height: 35px;
            text-align: center;
        }
        .remove-btn:hover {
            background-color: #d32f2f;
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
        .highlight {
            font-weight: bold;
        }
        .history-entry {
            border: 1px solid #ddd;
            padding: 10px;
            margin-bottom: 10px;
            border-radius: 5px;
            background-color: #fff;
        }
    </style>
</head>
<body onload="loadDataAndInitialize()">
    <div class="container">
        <div id="header-status">
            <button id="login-button" onclick="toggleAdminMode()">Login Responsable</button>
        </div>

        <h1>💰 TipShare : Calculateur de Pourboires</h1>

        <fieldset id="admin-controls-container" disabled>
            <div class="section">
                <h2>1. 💶 Montant Total des Pourboires</h2>
                <label for="total-tips">Montant total (€) :</label>
                <input type="number" id="total-tips" value="100" min="0" step="0.01" oninput="saveTotalTips()">
            </div>

            <div class="section">
                <h2>2. 🧑‍🤝‍🧑 Employés et Heures</h2>
                <div id="employee-list"></div>
                <button type="button" onclick="addEmployeeField()">➕ Ajouter un Employé</button>
            </div>

            <button type="button" onclick="calculateTips()">Calculer la Répartition</button>
        </fieldset>
        
        <div id="results" style="display:none;">
            <h2>Résultats</h2>
            <div class="result-item">
                <span>Total des Heures :</span>
                <span id="total-hours-output" class="highlight">0</span>
            </div>
            <div class="result-item">
                <span>Taux par Heure :</span>
                <span id="tip-rate-output" class="highlight">0.00 €</span>
            </div>
            <hr>
            <div id="individual-shares"></div>
        </div>

        <div class="section">
            <h2>Historique</h2>
            <div id="history-list"></div>
            <button id="clear-history-btn" style="display:none;" onclick="clearHistory()">Effacer l'Historique</button>
        </div>
    </div>

    <script>
        const EMPLOYEES_KEY = 'tipshare_employees';
        const TIPS_KEY = 'tipshare_total_tips';
        const HISTORY_KEY = 'tipshare_history';
        const ADMIN_STATE_KEY = 'tipshare_admin_active';
        const ADMIN_PASSWORD = '-Guigui951-';

        function loadDataAndInitialize() {
            loadEmployees();
            loadTotalTips();
            loadHistory();
            checkAdminMode();
        }

        function toggleAdminMode() {
            if (localStorage.getItem(ADMIN_STATE_KEY) === 'true') {
                localStorage.setItem(ADMIN_STATE_KEY, 'false');
                alert('Mode Responsable désactivé.');
            } else {
                const password = prompt('Veuillez entrer le mot de passe Responsable :');
                if (password === ADMIN_PASSWORD) {
                    localStorage.setItem(ADMIN_STATE_KEY, 'true');
                    alert('Mode Responsable activé.');
                } else if (password) {
                    alert('Mot de passe incorrect.');
                }
            }
            checkAdminMode();
        }

        function checkAdminMode() {
            const adminActive = localStorage.getItem(ADMIN_STATE_KEY) === 'true';
            document.getElementById('admin-controls-container').disabled = !adminActive;
            const loginButton = document.getElementById('login-button');
            const clearHistoryBtn = document.getElementById('clear-history-btn');
            
            if (adminActive) {
                loginButton.textContent = 'Logout Responsable';
                loginButton.classList.add('logout');
                clearHistoryBtn.style.display = 'block';
            } else {
                loginButton.textContent = 'Login Responsable';
                loginButton.classList.remove('logout');
                clearHistoryBtn.style.display = 'none';
            }
        }

        function addEmployeeField(name = '', hours = '') {
            const list = document.getElementById('employee-list');
            const div = document.createElement('div');
            div.className = 'employee-input-group';
            div.innerHTML = \`
                <input type="text" placeholder="Nom de l'employé" value="\${name}" oninput="saveEmployees()">
                <input type="number" placeholder="Heures" value="\${hours}" min="0" step="0.5" oninput="saveEmployees()">
                <button class="remove-btn" onclick="this.parentElement.remove(); saveEmployees();">×</button>
            \`;
            list.appendChild(div);
        }

        function saveEmployees() {
            const employees = [];
            document.querySelectorAll('.employee-input-group').forEach(group => {
                const name = group.children[0].value.trim();
                const hours = group.children[1].value;
                if (name) {
                    employees.push({ name, hours });
                }
            });
            localStorage.setItem(EMPLOYEES_KEY, JSON.stringify(employees));
        }

        function loadEmployees() {
            const employees = JSON.parse(localStorage.getItem(EMPLOYEES_KEY) || '[]');
            document.getElementById('employee-list').innerHTML = '';
            employees.forEach(emp => addEmployeeField(emp.name, emp.hours));
        }

        function saveTotalTips() {
            const totalTips = document.getElementById('total-tips').value;
            localStorage.setItem(TIPS_KEY, totalTips);
        }

        function loadTotalTips() {
            const totalTips = localStorage.getItem(TIPS_KEY) || '100';
            document.getElementById('total-tips').value = totalTips;
        }

        function calculateTips() {
            saveEmployees();
            const totalTips = parseFloat(document.getElementById('total-tips').value) || 0;
            const employees = JSON.parse(localStorage.getItem(EMPLOYEES_KEY) || '[]');
            
            let totalHours = 0;
            employees.forEach(emp => {
                totalHours += parseFloat(emp.hours) || 0;
            });

            const tipRate = totalHours > 0 ? totalTips / totalHours : 0;
            
            document.getElementById('total-hours-output').textContent = totalHours.toFixed(2);
            document.getElementById('tip-rate-output').textContent = \`\${tipRate.toFixed(2)} €\`;
            
            const sharesDiv = document.getElementById('individual-shares');
            sharesDiv.innerHTML = '';
            
            const calculationResult = {
                date: new Date().toLocaleString('fr-FR'),
                totalTips: totalTips.toFixed(2),
                totalHours: totalHours.toFixed(2),
                tipRate: tipRate.toFixed(2),
                shares: []
            };

            employees.forEach(emp => {
                const hours = parseFloat(emp.hours) || 0;
                const share = hours * tipRate;
                
                const resultItem = document.createElement('div');
                resultItem.className = 'result-item';
                resultItem.innerHTML = \`<span>\${emp.name}:</span> <span class="highlight">\${share.toFixed(2)} €</span>\`;
                sharesDiv.appendChild(resultItem);

                calculationResult.shares.push({ name: emp.name, share: share.toFixed(2) });
            });

            document.getElementById('results').style.display = 'block';
            saveToHistory(calculationResult);
        }

        function saveToHistory(result) {
            const history = JSON.parse(localStorage.getItem(HISTORY_KEY) || '[]');
            history.unshift(result);
            localStorage.setItem(HISTORY_KEY, JSON.stringify(history));
            loadHistory();
        }

        function loadHistory() {
            const history = JSON.parse(localStorage.getItem(HISTORY_KEY) || '[]');
            const historyDiv = document.getElementById('history-list');
            historyDiv.innerHTML = '';

            if (history.length === 0) {
                historyDiv.innerHTML = '<p>Aucun calcul enregistré.</p>';
                return;
            }

            history.forEach(entry => {
                const entryDiv = document.createElement('div');
                entryDiv.className = 'history-entry';
                
                let sharesHTML = entry.shares.map(s => \`<li>\${s.name}: \${s.share} €</li>\`).join('');
                
                entryDiv.innerHTML = \`
                    <strong>\${entry.date}</strong><br>
                    Total: \${entry.totalTips} € | Taux: \${entry.tipRate} €/h
                    <ul>\${sharesHTML}</ul>
                \`;
                historyDiv.appendChild(entryDiv);
            });
        }
        
        function clearHistory() {
            if (confirm("Êtes-vous sûr de vouloir effacer tout l'historique ?")) {
                localStorage.removeItem(HISTORY_KEY);
                loadHistory();
            }
        }
    </script>
</body>
</html>
