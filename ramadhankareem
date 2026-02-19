<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Ramadhan Quest: Fixed Schedule</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        :root {
            --bg-color: #1a1a2e;
            --card-bg: #16213e;
            --accent: #e94560;
            --gold: #ffd700;
            --text: #e0e0e0;
            --green: #4caf50;
            --excel-green: #217346;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; -webkit-tap-highlight-color: transparent; }

        body {
            background: linear-gradient(-45deg, #1a1a2e, #232342, #121225, #1a1a2e);
            background-size: 400% 400%;
            animation: gradientBG 20s ease infinite;
            color: var(--text);
            padding-bottom: 80px;
            overflow-x: hidden;
            min-height: 100vh;
        }

        @keyframes gradientBG {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        /* HEADER */
        .header {
            background: rgba(22, 33, 62, 0.9);
            padding: 20px;
            text-align: center;
            position: sticky;
            top: 0;
            z-index: 100;
            backdrop-filter: blur(10px);
            border-bottom: 1px solid rgba(255,255,255,0.05);
            box-shadow: 0 4px 20px rgba(0,0,0,0.3);
        }

        .level-badge {
            background: var(--accent); color: white; padding: 5px 15px;
            border-radius: 20px; font-weight: bold; font-size: 0.85rem;
            text-transform: uppercase; box-shadow: 0 4px 15px rgba(233, 69, 96, 0.4);
            display: inline-block;
            animation: floatBadge 3s ease-in-out infinite;
        }

        @keyframes floatBadge {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-3px); }
        }

        .exp-container {
            margin-top: 15px; background: rgba(0,0,0,0.5); height: 20px;
            border-radius: 12px; overflow: hidden; position: relative;
            border: 1px solid rgba(255,255,255,0.1);
        }

        .exp-bar {
            height: 100%;
            background: linear-gradient(90deg, var(--accent), var(--gold));
            width: 0%;
            transition: width 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);
            box-shadow: 0 0 10px var(--gold);
        }

        .exp-text {
            position: absolute; width: 100%; text-align: center;
            font-size: 11px; line-height: 20px; font-weight: bold;
            color: white; top: 0; text-shadow: 0 1px 2px rgba(0,0,0,0.8);
            letter-spacing: 0.5px;
        }

        /* CONTAINER */
        .container { max-width: 600px; margin: 0 auto; padding: 20px; }

        h2 { 
            margin-bottom: 20px; margin-top: 30px; font-size: 1.3rem;
            border-bottom: 2px solid var(--accent); display: inline-block;
            position: relative;
        }

        /* COUNTDOWN */
        .countdown-box {
            background: rgba(22, 33, 62, 0.6);
            border: 1px solid rgba(255, 215, 0, 0.3);
            border-radius: 16px; padding: 20px; text-align: center;
            margin-bottom: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            animation: slideUpFade 0.8s ease backwards;
        }

        .timer-display {
            font-size: 1.8rem; font-weight: bold; color: var(--gold);
            font-family: 'Courier New', Courier, monospace; margin: 10px 0;
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.3);
        }
        
        .timer-label { font-size: 0.7rem; color: #aaa; letter-spacing: 1px; text-transform: uppercase;}
        
        .timer-info { font-size: 0.8rem; color: #4caf50; margin-bottom: 5px; font-style: italic; }

        /* BUTTONS */
        button { user-select: none; -webkit-user-select: none; }
        .start-btn {
            background: linear-gradient(45deg, var(--accent), #ff6b6b);
            color: white; border: none; padding: 12px 25px; font-size: 1.1rem;
            border-radius: 50px; cursor: pointer; font-weight: bold;
            box-shadow: 0 10px 20px -10px rgba(233, 69, 96, 1);
            width: 100%; position: relative; overflow: hidden;
            transition: transform 0.2s;
        }
        .start-btn:active { transform: scale(0.95); }

        /* QUEST CARDS */
        .quest-card {
            background: var(--card-bg);
            border-radius: 16px; padding: 18px; margin-bottom: 12px;
            display: flex; justify-content: space-between; align-items: center;
            border-left: 4px solid rgba(255,255,255,0.05);
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            transition: transform 0.3s, background 0.3s;
            opacity: 0; 
            animation: slideInUp 0.5s ease forwards;
        }

        @keyframes slideInUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .quest-card.completed {
            border-left: 4px solid var(--green);
            background: rgba(76, 175, 80, 0.15);
        }

        .quest-info h3 { font-size: 1rem; margin-bottom: 4px; }
        .quest-info p { font-size: 0.8rem; color: rgba(255,255,255,0.5); }
        .exp-gain { 
            color: var(--gold); font-weight: bold; font-size: 0.8rem; 
            background: rgba(255, 215, 0, 0.1); padding: 4px 8px; border-radius: 12px;
        }

        /* CUSTOM CHECKBOX */
        .checkbox-wrapper input { display: none; }
        .checkbox-wrapper label {
            width: 40px; height: 40px;
            border: 2px solid #555;
            border-radius: 50%; display: flex; justify-content: center; align-items: center;
            cursor: pointer; transition: all 0.3s;
            background: rgba(0,0,0,0.2);
        }

        .checkbox-wrapper input:checked + label {
            background: var(--green); border-color: var(--green);
            transform: rotate(10deg);
        }
        .checkbox-wrapper input:checked + label::after {
            content: '✔'; font-size: 1.2rem; color: white;
        }

        /* MOOD & JOURNAL */
        .reflection-section {
            background: rgba(35, 35, 66, 0.5); padding: 20px;
            border-radius: 16px; margin-top: 30px; border: 1px solid rgba(255,255,255,0.05);
        }
        .mood-selector {
            display: flex; justify-content: space-between; margin: 15px 0;
            background: rgba(0,0,0,0.2); padding: 10px; border-radius: 12px;
        }
        .mood-btn {
            font-size: 2rem; background: none; border: none; cursor: pointer;
            transition: transform 0.2s; opacity: 0.4; filter: grayscale(1);
        }
        .mood-btn.selected { opacity: 1; transform: scale(1.3); filter: grayscale(0); }

        textarea {
            width: 100%; height: 100px; background: rgba(0, 0, 0, 0.3); border: 1px solid #444;
            color: white; padding: 12px; border-radius: 10px; resize: none; margin-top: 10px;
            font-size: 0.95rem;
        }
        textarea:focus { outline: none; border-color: var(--accent); }

        .save-btn {
            width: 100%; background: var(--accent); color: white; border: none;
            padding: 14px; border-radius: 12px; font-weight: bold; margin-top: 15px;
            cursor: pointer; font-size: 1rem; transition: background 0.3s;
        }
        
        /* CONTROLS AREA */
        .controls-area {
            margin-top: 40px;
            padding-top: 20px;
            border-top: 1px dashed #444;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .action-btn {
            background: transparent; border: 1px solid #444;
            padding: 12px; cursor: pointer; width: 100%; border-radius: 8px;
            font-size: 0.9rem; font-weight: bold; transition: all 0.3s;
        }
        
        .btn-excel { border-color: var(--excel-green); color: var(--excel-green); }
        .btn-excel:hover { background: rgba(33, 115, 70, 0.1); }
        
        .btn-reset { border-color: #888; color: #aaa; }
        .btn-reset:hover { background: rgba(255, 255, 255, 0.05); }
        
        .btn-danger { border-color: #e94560; color: #e94560; }
        .btn-danger:hover { background: rgba(233, 69, 96, 0.1); }

        /* MODAL */
        .modal {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9); justify-content: center; align-items: center; z-index: 200;
            backdrop-filter: blur(4px);
        }
        .modal-content {
            background: #1a1a2e; padding: 30px; border-radius: 20px; text-align: center;
            border: 2px solid var(--gold); width: 85%; max-width: 400px;
        }
    </style>
</head>
<body>

    <div id="appContent">
        <div class="header">
            <h1 style="font-size: 1.4rem; margin-bottom: 8px;">🌙 Ramadhan Quest</h1>
            <span class="level-badge" id="levelDisplay">Level 1</span>
            
            <div class="exp-container">
                <div class="exp-bar" id="expBar"></div>
                <div class="exp-text"><span id="currentExp">0</span> / <span id="maxExp">100</span> XP</div>
            </div>
        </div>

        <div class="container">
            
            <div class="countdown-box">
                <h3 style="margin-bottom: 5px; color: #e0e0e0; font-size: 1rem;">Sisa Waktu Ramadhan</h3>
                <p class="timer-info">Mulai: Rabu, 18 Feb 18:00</p>
                
                <button class="start-btn" id="startBtn" onclick="startCountdown()">🚀 Sinkronisasi Jadwal</button>
                
                <div style="display: none;" id="timerContainer">
                    <div class="timer-display" id="timerDisplay">00:00:00:00</div>
                    <div class="timer-label">HARI : JAM : MENIT : DETIK</div>
                </div>
            </div>

            <h2>📜 Misi Harian</h2>
            <div id="questContainer"></div>

            <h2>🧠 Refleksi</h2>
            <div class="reflection-section">
                <p style="text-align: center; font-size: 0.9rem; color: #aaa;">Mood hari ini:</p>
                <div class="mood-selector">
                    <button class="mood-btn" onclick="selectMood('😭')">😭</button>
                    <button class="mood-btn" onclick="selectMood('😐')">😐</button>
                    <button class="mood-btn" onclick="selectMood('🙂')">🙂</button>
                    <button class="mood-btn" onclick="selectMood('🥰')">🥰</button>
                    <button class="mood-btn" onclick="selectMood('😇')">😇</button>
                </div>

                <textarea id="journalInput" placeholder="Tuliskan evaluasi dan rasa syukurmu..."></textarea>
                <button class="save-btn" id="saveBtn" onclick="saveReflection()">💾 Simpan Jurnal</button>
            </div>
        </div>
    </div>

    <div class="container">
        <div class="controls-area" id="controlButtons">
            <button class="action-btn btn-excel" id="exportBtn" onclick="exportToExcel()">📊 Export Data ke Excel</button>
            <button class="action-btn btn-reset" onclick="resetDaily()">🔄 Reset Checklist (Hari Baru)</button>
            <button class="action-btn btn-danger" onclick="hardReset()">⚠️ Reset Total (Hapus Data)</button>
        </div>
    </div>

    <div class="modal" id="levelUpModal">
        <div class="modal-content">
            <h1 style="font-size: 3rem; margin-bottom: 10px;">🎉</h1>
            <h2 style="color: var(--gold); font-size: 1.5rem; margin-bottom: 10px;">LEVEL UP!</h2>
            <p>Istiqomah membuahkan hasil. Lanjutkan!</p>
            <br>
            <button class="save-btn" onclick="closeModal()">Lanjut</button>
        </div>
    </div>

    <script>
        // --- DATA ---
        const questsData = [
            { id: 'q_tahajjud', title: 'Sholat Tahajjud', time: '03:00 - 04:00', exp: 40 },
            { id: 'q_sahur', title: 'Makan Sahur', time: 'Sebelum Imsak', exp: 10 },
            { id: 'q_subuh', title: 'Sholat Subuh', time: '04:45', exp: 20 },
            { id: 'q_tilawah', title: 'Tilawah (1 Juz)', time: 'Fleksibel', exp: 50 },
            { id: 'q_sedekah', title: 'Sedekah', time: 'Fleksibel', exp: 30 },
            { id: 'q_dzuhur', title: 'Sholat Dzuhur', time: '12:00', exp: 20 },
            { id: 'q_ashar', title: 'Sholat Ashar', time: '15:15', exp: 20 },
            { id: 'q_puasa', title: 'Tuntas Puasa', time: 'Maghrib', exp: 100 },
            { id: 'q_maghrib', title: 'Sholat Maghrib', time: '18:00', exp: 20 },
            { id: 'q_isya', title: 'Sholat Isya', time: '19:15', exp: 20 },
            { id: 'q_tarawih', title: 'Sholat Tarawih', time: 'Malam', exp: 40 }
        ];

        let userState = {
            level: 1, currentExp: 0, maxExp: 100, completedQuests: [], journal: "", mood: "", targetDate: null
        };
        let timerInterval;

        window.onload = function() {
            loadData(); renderQuests(); updateUI(false); checkCountdownStatus();
        };

        // --- FIXED COUNTDOWN LOGIC ---
        function startCountdown() {
            // Kita anggap user mengkonfirmasi untuk mensinkronkan jadwal
            if(confirm("Sinkronkan timer dengan jadwal resmi: Rabu 18 Februari 18:00?")) {
                
                // SETTING JADWAL FIX DISINI
                // Tahun 2026 (sesuai kalender Rabu 18 Feb)
                // Bulan di JS: 0=Jan, 1=Feb
                const startDate = new Date(2026, 1, 18, 18, 0, 0).getTime();
                
                // Durasi Ramadhan: 30 Hari
                const duration = 30 * 24 * 60 * 60 * 1000;
                
                // Target Date = Start Date + 30 Hari
                userState.targetDate = startDate + duration;
                
                saveData(); 
                checkCountdownStatus();
            }
        }

        function checkCountdownStatus() {
            if (userState.targetDate) {
                document.getElementById('startBtn').style.display = 'none';
                document.getElementById('timerContainer').style.display = 'block';
                clearInterval(timerInterval);
                timerInterval = setInterval(updateTimer, 1000); updateTimer();
            }
        }

        function updateTimer() {
            const now = new Date().getTime();
            const distance = userState.targetDate - now;

            if (distance < 0) {
                clearInterval(timerInterval);
                document.getElementById('timerDisplay').innerHTML = "IDUL FITRI";
                document.querySelector('.timer-label').innerHTML = "Taqabbalallahu Minna Wa Minkum";
                return;
            }

            const d = Math.floor(distance / (1000 * 60 * 60 * 24));
            const h = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const m = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
            const s = Math.floor((distance % (1000 * 60)) / 1000);
            
            document.getElementById('timerDisplay').innerHTML = 
                `${d<10?"0"+d:d}:${h<10?"0"+h:h}:${m<10?"0"+m:m}:${s<10?"0"+s:s}`;
        }

        // --- EXPORT TO EXCEL ---
        function exportToExcel() {
            const btn = document.getElementById('exportBtn');
            const originalText = btn.innerText;
            btn.innerText = "⏳ Mengekspor...";

            const today = new Date();
            const dateStr = today.toISOString().split('T')[0];

            const completedNames = questsData
                .filter(q => userState.completedQuests.includes(q.id))
                .map(q => q.title).join(", ");
                
            const pendingNames = questsData
                .filter(q => !userState.completedQuests.includes(q.id))
                .map(q => q.title).join(", ");

            const dataToExport = [{
                "Tanggal": today.toLocaleDateString('id-ID'),
                "Nama": "User",
                "Level": userState.level,
                "XP": userState.currentExp,
                "Mood": userState.mood || "-",
                "Jurnal": userState.journal || "-",
                "Selesai": completedNames || "-",
                "Belum": pendingNames || "-"
            }];

            try {
                const ws = XLSX.utils.json_to_sheet(dataToExport);
                const wb = XLSX.utils.book_new();
                XLSX.utils.book_append_sheet(wb, ws, "Jurnal");
                XLSX.writeFile(wb, `Jurnal_Ramadhan_${dateStr}.xlsx`);
                setTimeout(() => { btn.innerText = originalText; alert("✅ Berhasil!"); }, 500);
            } catch (error) {
                alert("Gagal. Cek koneksi internet.");
                btn.innerText = originalText;
            }
        }

        // --- QUESTS SYSTEM ---
        function renderQuests() {
            const container = document.getElementById('questContainer');
            container.innerHTML = '';
            questsData.forEach((quest, i) => {
                const isChecked = userState.completedQuests.includes(quest.id);
                const html = `
                    <div class="quest-card ${isChecked ? 'completed' : ''}" id="card-${quest.id}" style="animation-delay: ${i * 0.05}s">
                        <div class="quest-info">
                            <h3>${quest.title}</h3>
                            <p>🕐 ${quest.time}</p>
                        </div>
                        <div style="text-align:right; margin-right:15px;">
                            <span class="exp-gain">+${quest.exp} XP</span>
                        </div>
                        <div class="checkbox-wrapper">
                            <input type="checkbox" id="${quest.id}" ${isChecked ? 'checked' : ''} onchange="toggleQuest('${quest.id}', ${quest.exp})">
                            <label for="${quest.id}"></label>
                        </div>
                    </div>
                `;
                container.innerHTML += html;
            });
            document.getElementById('journalInput').value = userState.journal || "";
            if(userState.mood) highlightMood(userState.mood);
        }

        function toggleQuest(id, exp) {
            const checkbox = document.getElementById(id);
            const card = document.getElementById(`card-${id}`);

            if (checkbox.checked) {
                userState.completedQuests.push(id);
                userState.currentExp += exp;
                card.classList.add('completed');
                checkLevelUp();
            } else {
                userState.completedQuests = userState.completedQuests.filter(q => q !== id);
                userState.currentExp -= exp;
                if(userState.currentExp < 0) userState.currentExp = 0;
                card.classList.remove('completed');
            }
            saveData(); updateUI(true);
        }

        function checkLevelUp() {
            if (userState.currentExp >= userState.maxExp) {
                userState.level++;
                userState.currentExp = userState.currentExp - userState.maxExp;
                userState.maxExp = Math.floor(userState.maxExp * 1.5);
                setTimeout(() => { document.getElementById('levelUpModal').style.display = 'flex'; }, 500);
                saveData();
            }
        }

        function closeModal() {
            document.getElementById('levelUpModal').style.display = 'none';
            updateUI(true);
        }

        function updateUI(animate) {
            document.getElementById('levelDisplay').innerText = `Level ${userState.level}`;
            const expEl = document.getElementById('currentExp');
            if(animate) animateValue(expEl, parseInt(expEl.innerText), userState.currentExp, 500);
            else expEl.innerText = userState.currentExp;
            document.getElementById('maxExp').innerText = userState.maxExp;
            const percentage = (userState.currentExp / userState.maxExp) * 100;
            document.getElementById('expBar').style.width = `${percentage}%`;
        }

        function animateValue(obj, start, end, duration) {
            let startTimestamp = null;
            const step = (timestamp) => {
                if (!startTimestamp) startTimestamp = timestamp;
                const progress = Math.min((timestamp - startTimestamp) / duration, 1);
                obj.innerHTML = Math.floor(progress * (end - start) + start);
                if (progress < 1) window.requestAnimationFrame(step);
            };
            window.requestAnimationFrame(step);
        }

        function selectMood(emoji) { userState.mood = emoji; highlightMood(emoji); }
        function highlightMood(emoji) {
            document.querySelectorAll('.mood-btn').forEach(btn => {
                btn.classList.toggle('selected', btn.innerText === emoji);
            });
        }
        function saveReflection() {
            userState.journal = document.getElementById('journalInput').value;
            saveData();
            const btn = document.getElementById('saveBtn');
            const originalText = btn.innerText;
            btn.style.background = "#4caf50"; btn.innerText = "✨ Tersimpan!";
            setTimeout(() => { btn.innerText = originalText; btn.style.background = ""; }, 1500);
        }
        function saveData() { localStorage.setItem('ramadhanQuestData', JSON.stringify(userState)); }
        function loadData() {
            const data = localStorage.getItem('ramadhanQuestData');
            if (data) userState = JSON.parse(data);
        }
        function resetDaily() {
            if(confirm("Mulai hari baru? (Checklist reset, Level aman)")) {
                userState.completedQuests = []; userState.journal = ""; userState.mood = "";
                saveData(); location.reload();
            }
        }
        function hardReset() {
            if(confirm("Hapus SEMUA data dan mulai dari awal?")) {
                localStorage.removeItem('ramadhanQuestData');
                location.reload();
            }
        }
    </script>
</body>
</html>
