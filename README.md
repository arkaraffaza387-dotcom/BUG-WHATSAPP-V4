<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>GENERAL-BOTZ V4</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        dasar: '#050714',
                        kartu: '#0F1424',
                        utama: '#165DFF',
                        emas: '#D4AF37',
                        lembut: '#A8B2D1',
                        batas: '#232940'
                    },
                    animation: {
                        'float': 'float 3s ease-in-out infinite',
                        'glow-pulse': 'glowPulse 2s ease-in-out infinite',
                        'fade-scale': 'fadeScale 0.5s ease-out forwards',
                    }
                }
            }
        }
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        * { font-family: 'Inter', sans-serif; }
        
        body {
            background-color: #050714;
            background-image: radial-gradient(circle at top, rgba(22,93,255,0.08), transparent 55%);
            min-height: 100vh;
            margin: 0;
            overflow-x: hidden;
        }
        .kaca {
            background: rgba(15, 20, 36, 0.85);
            border: 1px solid rgba(255,255,255,0.08);
            backdrop-filter: blur(12px);
            box-shadow: 0 8px 24px rgba(0,0,0,0.35);
            border-radius: 1.5rem;
        }
        .tombol-utama {
            background: linear-gradient(135deg,#165DFF,#367BFF);
            color: #fff;
            border-radius: 0.75rem;
            transition: all .3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }
        .tombol-utama:hover {
            filter: brightness(1.15);
            box-shadow: 0 0 25px rgba(22,93,255,0.6);
            transform: translateY(-3px);
        }

        .tab-content {
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            opacity: 0;
            transform: translateY(20px);
        }
        .tab-content.active {
            opacity: 1;
            transform: translateY(0);
            animation: fadeScale 0.5s ease-out;
        }

        @keyframes fadeScale {
            from { opacity: 0; transform: scale(0.95); }
            to { opacity: 1; transform: scale(1); }
        }
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-15px); }
        }
        @keyframes glowPulse {
            0%, 100% { box-shadow: 0 0 15px rgba(22,93,255,0.4); }
            50% { box-shadow: 0 0 30px rgba(22,93,255,0.7); }
        }

        .tab-active {
            position: relative;
            color: white;
        }
        .tab-active::after {
            content: '';
            position: absolute;
            bottom: -3px;
            left: 0;
            width: 100%;
            height: 4px;
            background: linear-gradient(to right, #165DFF, #00f5ff);
        }

        .notification {
            position: fixed; top: 20px; left: 50%; transform: translateX(-50%);
            background: #111; border: 2px solid #00ff88; padding: 15px 25px;
            z-index: 10000; border-radius: 20px; box-shadow: 0 8px 24px rgba(0,0,0,0.5);
            display: none; text-align: center; width: 85%; color: white;
        }

        .dashboard-container {
            width: 100%; max-width: 480px; background: #111111; padding: 20px;
            min-height: 100vh; display: flex; flex-direction: column;
            box-shadow: 0 0 30px rgba(0, 255, 136, 0.1);
        }
        .dashboard-input {
            width: 100%; padding: 12px; background: #1a1a1a; border: 1px solid #333;
            color: #00ff88; border-radius: 8px; outline: none;
        }
        .dashboard-input:focus {
            border-color: #00ff88; box-shadow: 0 0 12px rgba(0, 255, 136, 0.4);
        }
        .bug-grid {
            display: grid; grid-template-columns: 1fr 1fr; gap: 10px;
            margin-bottom: 20px; max-height: 420px; overflow-y: auto; padding-right: 8px;
        }
        .bug-btn {
            background: #151515; border: 1px solid #333; color: #00ff88;
            padding: 14px 8px; border-radius: 10px; text-align: center;
            font-size: 0.75em; cursor: pointer; transition: all 0.3s ease;
        }
        .bug-btn:hover {
            border-color: #00ff88; transform: translateY(-3px); box-shadow: 0 0 15px rgba(0,255,136,0.3);
        }
        .bug-btn.prem-only {
            border-color: #ffd700; color: #ffd700;
        }
        .console-log {
            background: #050505; border: 1px solid #222; height: 140px;
            overflow-y: auto; padding: 10px; font-size: 0.7em;
            color: #00ff44; font-family: monospace; border-radius: 8px;
        }
        .status-bar {
            display: flex; justify-content: space-between; font-size: 0.7em;
            color: #888; margin-bottom: 15px; background: #000; padding: 8px;
            border-radius: 6px; border: 1px solid rgba(0, 255, 136, 0.2);
        }

        .maintenance-screen {
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.98); z-index: 99999;
            display: none; flex-direction: column; align-items: center;
            justify-content: center; color: white; text-align: center; padding: 20px;
        }
    </style>
</head>
<body class="text-white flex items-center justify-center p-4">

    <div id="notification" class="notification"></div>

    <!-- MAINTENANCE SCREEN -->
    <div id="maintenance-screen" class="maintenance-screen">
        <div class="kaca p-8 max-w-md text-center">
            <h1 class="text-3xl font-bold text-red-500 mb-4">SERVER MAINTENANCE</h1>
            <p id="maintenance-text" class="text-xl mb-6">SERVER IS CURRENTLY UNDER PATCH AND UPDATE</p>
            <p class="text-sm text-gray-400 mb-8">UPDATE EVERY 1ST OF THE MONTH 12:00 - 15:00</p>
            <button onclick="forceLogout()" class="tombol-utama px-10 py-4 text-lg">EXIT APPLICATION</button>
        </div>
    </div>

    <!-- LANGUAGE MODAL -->
    <div id="lang-modal" class="hidden fixed inset-0 bg-black/95 flex items-center justify-center z-[99999]">
        <div class="kaca p-8 max-w-sm w-full text-center rounded-3xl">
            <h2 class="text-2xl font-bold mb-6">SELECT LANGUAGE</h2>
            <div class="space-y-3">
                <div onclick="selectLanguage('EN')" class="p-4 bg-[#1a1a1a] hover:bg-[#00ff88]/10 cursor-pointer rounded-2xl flex items-center gap-3 text-left">🇬🇧 ENGLISH</div>
                <div onclick="selectLanguage('ID')" class="p-4 bg-[#1a1a1a] hover:bg-[#00ff88]/10 cursor-pointer rounded-2xl flex items-center gap-3 text-left">🇮🇩 INDONESIA</div>
                <div onclick="selectLanguage('RU')" class="p-4 bg-[#1a1a1a] hover:bg-[#00ff88]/10 cursor-pointer rounded-2xl flex items-center gap-3 text-left">🇷🇺 RUSSIA</div>
            </div>
            <button onclick="hideLangModal()" class="mt-6 w-full py-3 bg-[#0F1424] border border-[#232940] rounded-xl">CLOSE</button>
        </div>
    </div>

    <!-- LOGIN PAGE -->
    <div id="login-page" class="w-full max-w-md">
        <div class="kaca p-8 text-center">
            <div class="mb-6">
                <img src="https://d2xsxph8kpxj0f.cloudfront.net/310519663746958640/oN9dk99xNWfi2iUtA8BFud/haru_yosuga_logo-ZYHqo9RPzgNRsJcDcnysiz.webp" 
                     alt="Logo" class="mx-auto w-28 h-28" style="animation: float 3s ease-in-out infinite;">
                <h1 class="text-4xl font-bold bg-gradient-to-r from-blue-400 to-cyan-300 bg-clip-text text-transparent">GENERAL-BOTZ V4</h1>
                <p class="text-emas text-xl font-semibold mt-1">OFFICIAL BUG INJECTOR</p>
            </div>

            <div class="flex justify-end mb-4">
                <button onclick="showLangModal()" class="text-sm px-5 py-2 bg-[#0F1424] border border-[#232940] rounded-2xl flex items-center gap-2">
                    <span id="current-lang">ENGLISH</span> <i class="fa fa-globe"></i>
                </button>
            </div>

            <div class="flex rounded-2xl overflow-hidden border border-[#232940] mb-8 bg-[#0a0a0f]">
                <button id="tabPremium" onclick="switchTab(0)" class="flex-1 py-4 text-lg font-semibold tab-active">💎 PREMIUM</button>
                <button id="tabBiasa" onclick="switchTab(1)" class="flex-1 py-4 text-lg font-semibold">👤 FREE</button>
            </div>

            <div id="contentPremium" class="tab-content active">
                <div class="mb-5">
                    <label class="block text-sm mb-2 text-gray-300">USERNAME</label>
                    <input type="text" id="userPremium" class="dashboard-input" placeholder="ENTER USERNAME">
                </div>
                <div class="mb-6">
                    <label class="block text-sm mb-2 text-gray-300">PASSWORD</label>
                    <input type="password" id="passPremium" class="dashboard-input" placeholder="••••••••">
                </div>
                <button onclick="masukPremium()" class="tombol-utama w-full py-4 text-lg font-semibold">LOGIN PREMIUM</button>
                <button onclick="navigateTo('register-premium-page')" class="w-full py-4 mt-3 bg-[#0F1424] border border-[#232940] rounded-2xl">REGISTER PREMIUM</button>
            </div>

            <div id="contentBiasa" class="tab-content hidden">
                <div class="mb-5">
                    <label class="block text-sm mb-2 text-gray-300">USERNAME</label>
                    <input type="text" id="userBiasa" class="dashboard-input" placeholder="ENTER USERNAME">
                </div>
                <div class="mb-6">
                    <label class="block text-sm mb-2 text-gray-300">PASSWORD</label>
                    <input type="password" id="passBiasa" class="dashboard-input" placeholder="••••••••">
                </div>
                <button onclick="masukBiasa()" class="tombol-utama w-full py-4 text-lg font-semibold">LOGIN FREE</button>
                <button onclick="navigateTo('reg-step-tiktok')" class="w-full py-4 mt-3 bg-[#0F1424] border border-[#232940] rounded-2xl">REGISTER FREE ACCOUNT</button>
            </div>

            <p id="pesanSistem" class="text-sm mt-6 min-h-[1.5rem] text-red-400"></p>
        </div>
    </div>

    <!-- REGISTER PREMIUM -->
    <div id="register-premium-page" class="w-full max-w-md hidden">
        <div class="kaca p-8 text-center">
            <h2 class="text-2xl font-bold text-emas mb-6">REGISTER PREMIUM</h2>
            <input type="password" id="reg-prem-code" class="dashboard-input mb-3" placeholder="VERIFICATION CODE">
            <input type="text" id="reg-prem-user" class="dashboard-input mb-3" placeholder="NEW USERNAME">
            <input type="password" id="reg-prem-pass" class="dashboard-input mb-6" placeholder="NEW PASSWORD">
            <button onclick="processRegisterPremium()" class="tombol-utama w-full py-3">ACTIVATE PREMIUM</button>
            <button onclick="navigateTo('login-page')" class="w-full py-3 mt-2 bg-[#0F1424] border border-[#232940] rounded-xl">BACK</button>
        </div>
    </div>

    <!-- REG STEP TIKTOK -->
    <div id="reg-step-tiktok" class="w-full max-w-md hidden">
        <div class="kaca p-8 text-center">
            <h2 class="text-2xl font-bold mb-6">TIKTOK VERIFICATION</h2>
            <button onclick="openTikTok()" class="tombol-utama w-full py-3">FOLLOW TIKTOK</button>
            <button id="tiktok-next" onclick="navigateTo('reg-step-wa')" class="hidden w-full py-3 mt-2 bg-[#0F1424] border border-[#232940] rounded-xl">CONTINUE</button>
            <button onclick="navigateTo('login-page')" class="w-full py-3 mt-2 bg-[#0F1424] border border-[#232940] rounded-xl">CANCEL</button>
        </div>
    </div>

    <!-- REG STEP WA -->
    <div id="reg-step-wa" class="w-full max-w-md hidden">
        <div class="kaca p-8 text-center">
            <h2 class="text-2xl font-bold mb-6">WHATSAPP VERIFICATION</h2>
            <button onclick="openWAChannel()" class="tombol-utama w-full py-3">JOIN WA CHANNEL</button>
            <button id="wa-next" onclick="navigateTo('register-free-page')" class="hidden w-full py-3 mt-2 bg-[#0F1424] border border-[#232940] rounded-xl">CONTINUE</button>
            <button onclick="navigateTo('reg-step-tiktok')" class="w-full py-3 mt-2 bg-[#0F1424] border border-[#232940] rounded-xl">BACK</button>
        </div>
    </div>

    <!-- REGISTER FREE -->
    <div id="register-free-page" class="w-full max-w-md hidden">
        <div class="kaca p-8 text-center">
            <h2 class="text-2xl font-bold mb-6">REGISTER FREE ACCOUNT</h2>
            <input type="text" id="reg-free-user" class="dashboard-input mb-3" placeholder="USERNAME">
            <input type="password" id="reg-free-pass" class="dashboard-input mb-6" placeholder="PASSWORD">
            <button onclick="processRegisterFree()" class="tombol-utama w-full py-3">CREATE FREE ACCOUNT</button>
            <button onclick="navigateTo('login-page')" class="w-full py-3 mt-2 bg-[#0F1424] border border-[#232940] rounded-xl">BACK</button>
        </div>
    </div>

    <!-- DASHBOARD -->
    <div id="dashboard-page" class="dashboard-container hidden">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
            <h2 style="margin:0; font-size:1.2em; color:#00ff88;">GENERAL-BOTZ V4</h2>
            <div class="flex gap-2">
                <button onclick="showLangModal()" class="text-xs px-3 py-1 bg-[#0F1424] border border-[#232940] rounded-xl">🌐</button>
                <span id="user-badge" style="font-size:0.6em; padding:4px 12px; border-radius:9999px; border:1px solid #555;">FREE</span>
            </div>
        </div>

        <div class="status-bar">
            <span>USER: <b id="display-username">-</b></span>
            <span>EXP: <b id="expiry-date">-</b></span>
        </div>

        <div id="lock-section" class="hidden p-4 rounded-xl border border-yellow-400 bg-yellow-400/10 mb-6">
            <h3 class="text-yellow-400 text-center mb-3">🔒 DEVICE LOCK (PREMIUM)</h3>
            <button onclick="executeDeviceLock()" class="tombol-utama w-full py-3">INJECT DEVICE LOCK</button>
        </div>

        <div class="mb-4">
            <label class="block text-sm mb-2 text-gray-400">TARGET NUMBER (62...)</label>
            <input type="tel" id="target-number" class="dashboard-input" placeholder="628xxxxxxxxxx">
        </div>

        <h2 class="text-sm text-gray-400 mb-3">BUG LIST (<span id="bug-count">0</span>)</h2>
        <div class="bug-grid" id="bug-list"></div>

        <div class="console-log" id="console-output">
            <div>> SYSTEM READY... WELCOME TO GENERAL-BOTZ V4</div>
        </div>

        <div class="flex gap-3 mt-6">
            <button onclick="logout()" class="flex-1 py-4 text-red-500 border border-red-500 rounded-2xl font-semibold">LOGOUT</button>
            <button onclick="forceLogout()" class="flex-1 py-4 text-red-500 border border-red-500 rounded-2xl font-semibold">EXIT APPLICATION</button>
        </div>
    </div>

    <!-- SUCCESS MODAL -->
    <div id="success-modal" class="hidden fixed inset-0 bg-black/95 flex items-center justify-center z-[9999]">
        <div class="kaca p-6 max-w-md w-full text-center rounded-3xl">
            <h2 class="text-2xl font-bold text-emas mb-4">🎉 LOGIN SUCCESSFUL!</h2>
            <iframe id="login-video" width="100%" height="220" src="https://www.youtube.com/embed/CY5WLrSYPzo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
            <button id="close-video-btn" onclick="closeSuccessModal()" class="tombol-utama w-full py-4 mt-6 text-lg">CLOSE</button>
        </div>
    </div>

    <script>
        const DB_KEY = "nexus_v2_data";
        const SESSION_KEY = "nexus_v2_session";
        const SECRET_CODE = "GENERALBOTZ-V4-INFINITE-NEXUS";   // ← KODE BARU
        const LAST_UPDATE_KEY = "last_update_month";

        let dailySendCount = 0;
        let lastSendDate = null;
        let currentLang = "EN";

        const translations = {
            EN: { ready: "> SYSTEM READY... WELCOME TO GENERAL-BOTZ V4", maintenance: "SERVER IS CURRENTLY UNDER PATCH AND UPDATE" },
            ID: { ready: "> SISTEM SIAP... SELAMAT DATANG DI GENERAL-BOTZ V4", maintenance: "SERVER SEDANG DALAM PATCH DAN UPDATE" },
            RU: { ready: "> СИСТЕМА ГОТОВА... ДОБРО ПОЖАЛОВАТЬ", maintenance: "СЕРВЕР В ПРОЦЕССЕ ОБНОВЛЕНИЯ" }
        };

        const BUG_TYPES = ["GHOST_CALL","MEDIA_BURST","RAM_EATER","TEXT_BOMB","SENSOR_BUG","DB_CORRUPT","NOTIF_SPAM","FLICKER","DARK_LOCK","REPLY_LOOP","BATTERY_DRAIN","CPU_OVERHEAT","GPS_GLITCH","MIC_MUTE","CAM_FREEZE","STORAGE_FULL","WIFI_DROP","BLUETOOTH_LAG","UI_SHAKE","FONT_MESS","KEYBOARD_STUCK"];

        function t(key) {
            return translations[currentLang][key] || key;
        }

        function showNotification(msg) {
            const n = document.getElementById('notification');
            n.innerText = msg.toUpperCase();
            n.style.display = 'block';
            setTimeout(() => n.style.display = 'none', 4000);
        }

        function pesan(teks) {
            document.getElementById('pesanSistem').innerHTML = teks.toUpperCase();
        }

        function navigateTo(id) {
            document.querySelectorAll('div[id$="-page"], div[id^="content"]').forEach(el => el.classList.add('hidden'));
            const target = document.getElementById(id);
            if (target) target.classList.remove('hidden');
        }

        function switchTab(n) {
            document.getElementById('tabPremium').classList.toggle('tab-active', n === 0);
            document.getElementById('tabBiasa').classList.toggle('tab-active', n === 1);
            document.getElementById('contentPremium').classList.toggle('hidden', n !== 0);
            document.getElementById('contentBiasa').classList.toggle('hidden', n !== 1);
            document.getElementById('contentPremium').classList.toggle('active', n === 0);
            document.getElementById('contentBiasa').classList.toggle('active', n === 1);
        }

        function showLangModal() {
            document.getElementById('lang-modal').classList.remove('hidden');
        }

        function hideLangModal() {
            document.getElementById('lang-modal').classList.add('hidden');
        }

        function selectLanguage(lang) {
            currentLang = lang;
            localStorage.setItem("preferred_lang", lang);
            document.getElementById('current-lang').textContent = lang === 'EN' ? 'ENGLISH' : lang === 'ID' ? 'INDONESIA' : 'RUSSIA';
            hideLangModal();
            showNotification("LANGUAGE CHANGED TO " + (lang === 'EN' ? 'ENGLISH' : lang === 'ID' ? 'INDONESIA' : 'RUSSIA'));
        }

        function isMaintenanceTime() {
            const now = new Date();
            return now.getDate() === 1 && now.getHours() >= 12 && now.getHours() <= 15;
        }

        function checkMaintenance() {
            if (isMaintenanceTime()) {
                document.getElementById('maintenance-screen').style.display = 'flex';
                document.getElementById('maintenance-text').textContent = t('maintenance').toUpperCase();
                return true;
            }
            return false;
        }

        function processRegisterPremium() {
            const code = document.getElementById('reg-prem-code').value.trim();
            const user = document.getElementById('reg-prem-user').value.trim();
            const pass = document.getElementById('reg-prem-pass').value;

            if (code !== SECRET_CODE) {
                return pesan("WRONG CODE! CORRECT CODE: GENERALBOTZ-V4-INFINITE-NEXUS");
            }
            if (user.length < 4) return pesan("USERNAME MIN 4 CHARACTERS");

            let users = JSON.parse(localStorage.getItem(DB_KEY) || "[]");
            if (users.find(u => u.username === user)) return pesan("USERNAME ALREADY EXISTS");

            users.push({ username: user, password: pass, type: "PREMIUM", expiredAt: "LIFETIME" });
            localStorage.setItem(DB_KEY, JSON.stringify(users));
            pesan("PREMIUM ACCOUNT CREATED SUCCESSFULLY");
            setTimeout(() => navigateTo('login-page'), 1500);
        }

        function processRegisterFree() {
            const user = document.getElementById('reg-free-user').value.trim();
            const pass = document.getElementById('reg-free-pass').value;

            if (user.length < 4) return pesan("USERNAME MIN 4 CHARACTERS");

            let users = JSON.parse(localStorage.getItem(DB_KEY) || "[]");
            if (users.find(u => u.username === user)) return pesan("USERNAME ALREADY EXISTS");

            const exp = new Date(); exp.setDate(exp.getDate() + 4);
            users.push({ username: user, password: pass, type: "FREE", expiredAt: exp.toISOString() });
            localStorage.setItem(DB_KEY, JSON.stringify(users));
            pesan("FREE ACCOUNT CREATED SUCCESSFULLY (4 DAYS)");
            setTimeout(() => navigateTo('login-page'), 1500);
        }

        function masukPremium() {
            if (checkMaintenance()) return;
            const user = document.getElementById('userPremium').value.trim();
            const pass = document.getElementById('passPremium').value;
            let users = JSON.parse(localStorage.getItem(DB_KEY) || "[]");
            const found = users.find(x => x.username === user && x.password === pass && x.type === "PREMIUM");
            if (found) {
                localStorage.setItem(SESSION_KEY, JSON.stringify(found));
                loadDashboard(found);
            } else pesan("WRONG PREMIUM USERNAME OR PASSWORD");
        }

        function masukBiasa() {
            if (checkMaintenance()) return;
            const user = document.getElementById('userBiasa').value.trim();
            const pass = document.getElementById('passBiasa').value;
            let users = JSON.parse(localStorage.getItem(DB_KEY) || "[]");
            const found = users.find(x => x.username === user && x.password === pass && x.type === "FREE");
            if (found) {
                if (new Date() > new Date(found.expiredAt)) return pesan("FREE ACCOUNT HAS EXPIRED");
                localStorage.setItem(SESSION_KEY, JSON.stringify(found));
                loadDashboard(found);
            } else pesan("WRONG FREE USERNAME OR PASSWORD");
        }

        function checkDailyLimit(isPrem) {
            if (isPrem) return true;
            const today = new Date().toDateString();
            if (lastSendDate !== today) { dailySendCount = 0; lastSendDate = today; }
            if (dailySendCount >= 10) {
                showNotification("FREE DAILY LIMIT 10X REACHED");
                return false;
            }
            return true;
        }

        function sendBug(type) {
            const target = document.getElementById('target-number').value.trim();
            if (!target.startsWith('62') || target.length < 11) return showNotification("INVALID TARGET NUMBER");

            const session = JSON.parse(localStorage.getItem(SESSION_KEY) || "{}");
            if (!checkDailyLimit(session.type === "PREMIUM")) return;

            addLog(`INJECTING ${type} TO ${target}...`);
            setTimeout(() => {
                addLog(`✓ ${type} SENT SUCCESSFULLY!`);
                showNotification("BUG SENT SUCCESSFULLY");
                dailySendCount++;
            }, 1200);
        }

        function addLog(msg) {
            const consoleEl = document.getElementById('console-output');
            const line = document.createElement('div');
            line.textContent = `> ${msg}`;
            consoleEl.appendChild(line);
            consoleEl.scrollTop = consoleEl.scrollHeight;
        }

        function getBugLimit(type) {
            let prem = 10000;
            let free = 150;
            const now = new Date();
            const currentMonth = `\( {now.getFullYear()}- \){now.getMonth()+1}`;

            if (now.getDate() === 1 && localStorage.getItem(LAST_UPDATE_KEY) !== currentMonth) {
                prem += 1500;
                free += 20;
                localStorage.setItem(LAST_UPDATE_KEY, currentMonth);
                showNotification("SYSTEM UPDATED - NEW BUG LIMITS APPLIED");
            }
            return type === "PREMIUM" ? prem : free;
        }

        function loadDashboard(acc) {
            if (checkMaintenance()) return;
            document.getElementById('login-page').classList.add('hidden');
            document.getElementById('dashboard-page').classList.remove('hidden');

            document.getElementById('display-username').textContent = acc.username;
            const isPrem = acc.type === "PREMIUM";
            const limit = getBugLimit(acc.type);

            document.getElementById('bug-count').textContent = limit;
            const badge = document.getElementById('user-badge');
            badge.textContent = isPrem ? "PREMIUM" : "FREE";
            badge.style.borderColor = isPrem ? "#ffd700" : "#00ff88";
            badge.style.color = isPrem ? "#ffd700" : "#00ff88";

            document.getElementById('expiry-date').textContent = isPrem ? "LIFETIME" : new Date(acc.expiredAt).toLocaleDateString('id-ID');
            document.getElementById('lock-section').classList.toggle('hidden', !isPrem);

            const bugList = document.getElementById('bug-list');
            bugList.innerHTML = "";
            for (let i = 0; i < limit; i++) {
                const name = BUG_TYPES[i % BUG_TYPES.length] + (i >= 23 ? `_${i+1}` : "");
                const btn = document.createElement('div');
                btn.className = `bug-btn ${i >= 100 ? 'prem-only' : ''}`;
                btn.innerHTML = `<strong>${name.replace(/_/g, ' ')}</strong>`;
                btn.onclick = () => sendBug(name);
                bugList.appendChild(btn);
            }

            document.getElementById('success-modal').classList.remove('hidden');
            addLog(t('ready'));
        }

        function executeDeviceLock() {
            showNotification("DEVICE LOCK INJECTED SUCCESSFULLY");
        }

        function logout() {
            localStorage.removeItem(SESSION_KEY);
            location.reload();
        }

        function forceLogout() {
            localStorage.clear();
            alert("APPLICATION CLOSED");
            window.close();
        }

        function openTikTok() {
            window.open('https://www.tiktok.com/@kiboyaslinofake', '_blank');
            setTimeout(() => document.getElementById('tiktok-next').classList.remove('hidden'), 1000);
        }

        function openWAChannel() {
            window.open('https://whatsapp.com/channel/0029VbDBArY9MF8uLpmviF1S', '_blank');
            setTimeout(() => document.getElementById('wa-next').classList.remove('hidden'), 1000);
        }

        function closeSuccessModal() {
            document.getElementById('success-modal').classList.add('hidden');
        }

        window.onload = () => {
            const savedLang = localStorage.getItem("preferred_lang") || "EN";
            currentLang = savedLang;
            document.getElementById('current-lang').textContent = savedLang === 'EN' ? 'ENGLISH' : savedLang === 'ID' ? 'INDONESIA' : 'RUSSIA';

            if (!checkMaintenance()) {
                switchTab(0);
                const session = localStorage.getItem(SESSION_KEY);
                if (session) {
                    loadDashboard(JSON.parse(session));
                } else {
                    document.getElementById('login-page').classList.remove('hidden');
                }
            }
        };
    </script>
</body>
</html>
