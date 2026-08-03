<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Money Maker Club</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { background-color: #0f172a; color: #fff; display: flex; justify-content: center; align-items: flex-start; min-height: 100vh; padding: 15px; }
        
        /* Main Container - Expanded Size */
        .app-container { width: 100%; max-width: 520px; background: #1e293b; border-radius: 16px; padding: 24px; box-shadow: 0 10px 30px rgba(0,0,0,0.6); border: 1px solid #334155; margin-top: 10px; }
        
        /* Auth UI */
        .auth-card { display: flex; flex-direction: column; gap: 15px; text-align: center; }
        .auth-card h2 { color: #f59e0b; font-size: 28px; cursor: pointer; user-select: none; }
        .input-group input { width: 100%; padding: 14px; border-radius: 8px; border: 1px solid #475569; background: #0f172a; color: #fff; font-size: 16px; outline: none; margin-bottom: 10px; }
        .btn { width: 100%; padding: 14px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; font-size: 16px; transition: 0.2s; }
        .btn-primary { background: #f59e0b; color: #000; }
        .btn-secondary { background: #334155; color: #fff; margin-top: 8px; }
        
        /* Dashboard UI */
        .header-card { background: #0f172a; border-radius: 12px; padding: 18px; margin-bottom: 20px; border: 1px solid #334155; }
        .user-info { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; }
        .brand-title { font-size: 20px; font-weight: bold; color: #f59e0b; cursor: pointer; user-select: none; }
        .balance { font-size: 32px; font-weight: bold; color: #10b981; margin-top: 5px; }
        .actions-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 20px; }
        
        /* Game Section */
        .game-card { background: #0f172a; border-radius: 12px; padding: 20px; border: 1px solid #334155; text-align: center; }
        .period-box { display: flex; justify-content: space-between; font-size: 16px; color: #94a3b8; margin-bottom: 15px; }
        .timer-box { background: #1e293b; padding: 12px; border-radius: 8px; font-size: 26px; font-weight: bold; margin: 15px 0; color: #ef4444; border: 1px solid #334155; }
        .color-buttons { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; margin-bottom: 18px; }
        .btn-green { background: #10b981; color: #fff; font-size: 18px; padding: 16px; }
        .btn-violet { background: #8b5cf6; color: #fff; font-size: 18px; padding: 16px; }
        .btn-red { background: #ef4444; color: #fff; font-size: 18px; padding: 16px; }
        
        /* Admin Modal */
        .admin-modal { background: #0f172a; padding: 20px; border-radius: 12px; border: 2px solid #ef4444; margin-top: 20px; }
        .admin-modal h3 { color: #ef4444; margin-bottom: 15px; text-align: center; }
        .user-row { display: flex; justify-content: space-between; align-items: center; background: #1e293b; padding: 10px; border-radius: 6px; margin-bottom: 8px; font-size: 14px; }
        .user-row input { width: 80px; padding: 6px; border-radius: 4px; border: 1px solid #475569; background: #0f172a; color: #fff; }
        
        .hidden { display: none !important; }
    </style>
</head>
<body>

<div class="app-container">
    <!-- AUTH SCREEN -->
    <div id="authView" class="auth-card">
        <h2 onclick="triggerSecretAdmin()">🎰 Money Maker Club</h2>
        <p style="color: #94a3b8; font-size: 14px; margin-bottom: 10px;">Login ya Naya Account Banayein</p>
        <div class="input-group">
            <input type="tel" id="userPhone" placeholder="Mobile Number (10 Digits)" maxlength="10">
            <input type="password" id="userPass" placeholder="Password">
        </div>
        <button class="btn btn-primary" onclick="handleLogin()">Login Account</button>
        <button class="btn btn-secondary" onclick="handleRegister()">Naya Account Banayein (Register)</button>
    </div>

    <!-- GAME DASHBOARD -->
    <div id="dashboardView" class="hidden">
        <div class="header-card">
            <div class="user-info">
                <span class="brand-title" onclick="triggerSecretAdmin()">🎰 Money Maker</span>
                <div>
                    <span id="displayUser" style="font-size: 14px; color: #94a3b8; margin-right: 8px;">ID: --</span>
                    <button class="btn btn-secondary" style="width: auto; padding: 6px 12px; font-size: 12px;" onclick="handleLogout()">Logout</button>
                </div>
            </div>
            <div style="font-size: 12px; color: #94a3b8;">Total Balance</div>
            <div class="balance" id="displayBalance">₹0.00</div>
        </div>

        <div class="actions-grid">
            <button class="btn btn-secondary" onclick="alert('Deposit ke liye Admin ko DM karein!')">Deposit</button>
            <button class="btn btn-secondary" onclick="alert('Withdrawal ke liye Admin ko DM karein!')">Withdraw</button>
        </div>

        <div class="game-card">
            <div class="period-box">
                <span>Period ID: <strong id="periodId" style="color: #fff;">2026001</strong></span>
                <span>Status: <strong style="color: #10b981;">Active</strong></span>
            </div>
            <div class="timer-box" id="timer">00:30</div>
            
            <div class="color-buttons">
                <button class="btn btn-green" onclick="placeBet('Green')">Green</button>
                <button class="btn btn-violet" onclick="placeBet('Violet')">Violet</button>
                <button class="btn btn-red" onclick="placeBet('Red')">Red</button>
            </div>
            
            <div class="input-group">
                <input type="number" id="betAmount" placeholder="Enter Amount (Min ₹10)" min="10">
            </div>
        </div>
    </div>

    <!-- SECRET ADMIN PANEL (HIDDEN) -->
    <div id="adminPanel" class="admin-modal hidden">
        <h3>👑 Secret Admin Control</h3>
        <div id="adminUserList"></div>
        <button class="btn btn-secondary" onclick="closeAdmin()" style="margin-top: 10px;">Close Admin Panel</button>
    </div>
</div>

<script>
    // --- PERSISTENT SESSION ON PAGE LOAD ---
    window.onload = function() {
        const activeUser = localStorage.getItem('mmc_active_session');
        if (activeUser) {
            showDashboard(JSON.parse(activeUser));
        }
    };

    // --- AUTHENTICATION LOGIC ---
    function handleRegister() {
        const phone = document.getElementById('userPhone').value.trim();
        const pass = document.getElementById('userPass').value.trim();
        
        if (phone.length !== 10 || !pass) return alert("Kripya 10-digit number aur password sahi se bharein!");

        let users = JSON.parse(localStorage.getItem('mmc_users') || '{}');
        if (users[phone]) return alert("Ye number pehle se registered hai! Direct Login karein.");

        const newUser = { phone: phone, pass: pass, balance: 20.00 };
        users[phone] = newUser;
        localStorage.setItem('mmc_users', JSON.stringify(users));

        saveSessionAndLogin(newUser);
    }

    function handleLogin() {
        const phone = document.getElementById('userPhone').value.trim();
        const pass = document.getElementById('userPass').value.trim();

        let users = JSON.parse(localStorage.getItem('mmc_users') || '{}');
        if (users[phone] && users[phone].pass === pass) {
            saveSessionAndLogin(users[phone]);
        } else {
            alert("Galat Mobile Number ya Password!");
        }
    }

    function saveSessionAndLogin(userObj) {
        localStorage.setItem('mmc_active_session', JSON.stringify(userObj));
        showDashboard(userObj);
    }

    function showDashboard(userObj) {
        document.getElementById('authView').classList.add('hidden');
        document.getElementById('dashboardView').classList.remove('hidden');
        document.getElementById('displayUser').innerText = "ID: " + userObj.phone;
        document.getElementById('displayBalance').innerText = "₹" + parseFloat(userObj.balance).toFixed(2);
        startTimer();
    }

    function handleLogout() {
        localStorage.removeItem('mmc_active_session');
        location.reload();
    }

    // --- LIGHTWEIGHT TIMER ---
    let timeLeft = 30;
    let timerRunning = false;
    function startTimer() {
        if(timerRunning) return;
        timerRunning = true;
        setInterval(() => {
            timeLeft--;
            if (timeLeft < 0) timeLeft = 30;
            document.getElementById('timer').innerText = "00:" + (timeLeft < 10 ? "0" + timeLeft : timeLeft);
        }, 1000);
    }

    // --- BETTING LOGIC ---
    function placeBet(color) {
        const amount = parseFloat(document.getElementById('betAmount').value);
        let activeUser = JSON.parse(localStorage.getItem('mmc_active_session'));

        if (!amount || amount < 10) return alert("Minimum Bet Amount ₹10 hai!");
        if (activeUser.balance < amount) return alert("Aapke paas पर्याप्त balance nahi hai!");

        activeUser.balance -= amount;
        
        // Sync active session and DB
        localStorage.setItem('mmc_active_session', JSON.stringify(activeUser));
        let users = JSON.parse(localStorage.getItem('mmc_users'));
        users[activeUser.phone].balance = activeUser.balance;
        localStorage.setItem('mmc_users', JSON.stringify(users));

        document.getElementById('displayBalance').innerText = "₹" + activeUser.balance.toFixed(2);
        document.getElementById('betAmount').value = '';
        alert(color + " par ₹" + amount + " ki bet lag chuki hai!");
    }

    // --- SECRET ADMIN SYSTEM ---
    let clickCount = 0;
    function triggerSecretAdmin() {
        clickCount++;
        if (clickCount >= 4) {
            clickCount = 0;
            const adminPass = prompt("Enter Admin Secret Key:");
            if (adminPass === "admin123") { // Admin password yahan change kar sakte hain
                openAdmin();
            } else if (adminPass !== null) {
                alert("Galat Admin Key!");
            }
        }
    }

    function openAdmin() {
        document.getElementById('adminPanel').classList.remove('hidden');
        loadAdminUsers();
    }

    function closeAdmin() {
        document.getElementById('adminPanel').classList.add('hidden');
    }

    function loadAdminUsers() {
        const container = document.getElementById('adminUserList');
        container.innerHTML = '';
        let users = JSON.parse(localStorage.getItem('mmc_users') || '{}');

        if (Object.keys(users).length === 0) {
            container.innerHTML = "<p style='text-align:center;'>Koi user found nahi hua.</p>";
            return;
        }

        for (let phone in users) {
            const u = users[phone];
            const div = document.createElement('div');
            div.className = 'user-row';
            div.innerHTML = `
                <div>
                    <strong>${u.phone}</strong><br>
                    <small>Pass: ${u.pass}</small>
                </div>
                <div>
                    ₹<input type="number" id="adm_bal_${u.phone}" value="${u.balance}">
                    <button class="btn btn-primary" style="padding: 4px 8px; font-size: 12px; width: auto;" onclick="saveAdminBal('${u.phone}')">Save</button>
                </div>
            `;
            container.appendChild(div);
        }
    }

    function saveAdminBal(phone) {
        const newBal = parseFloat(document.getElementById('adm_bal_' + phone).value);
        let users = JSON.parse(localStorage.getItem('mmc_users'));
        
        if (users[phone]) {
            users[phone].balance = newBal;
            localStorage.setItem('mmc_users', JSON.stringify(users));

            // Sync if this user is currently active
            let active = JSON.parse(localStorage.getItem('mmc_active_session'));
            if (active && active.phone === phone) {
                active.balance = newBal;
                localStorage.setItem('mmc_active_session', JSON.stringify(active));
                document.getElementById('displayBalance').innerText = "₹" + newBal.toFixed(2);
            }
            alert("ID " + phone + " ka balance ₹" + newBal + " update ho gaya!");
            loadAdminUsers();
        }
    }
</script>
</body>
</html>
