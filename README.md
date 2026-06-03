<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>香港單車教練 Hub</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <style>
        .modal-active { display: flex !important; z-index: 9999 !important; animation: fadeIn 0.2s cubic-bezier(0.16, 1, 0.3, 1); }
        @keyframes fadeIn { from { opacity: 0; transform: scale(0.99); } to { opacity: 1; transform: scale(1); } }
        
        input[type="date"]::-webkit-calendar-picker-indicator {
            filter: invert(1);
            cursor: pointer;
        }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="bg-gray-950 text-white font-sans p-3 sm:p-4 md:p-6 min-h-screen h-screen flex flex-col overflow-x-hidden antialiased select-none">

    <header class="mb-4 sm:mb-6 border-b border-gray-800 pb-3 sm:pb-4 flex flex-col md:flex-row justify-between items-start md:items-center gap-3 sm:gap-4 shrink-0">
        <div>
            <h1 class="text-2xl sm:text-3xl font-black tracking-tight text-white">HK Cycling Coach Hub</h1>
        </div>
        
        <div class="flex flex-row items-center gap-3 bg-gray-900 p-2 sm:p-3 rounded-xl sm:rounded-2xl border border-gray-800 shadow-xl w-full md:w-auto justify-between md:justify-start">
            <div class="flex items-center gap-2">
                <span class="text-lg sm:text-xl">📅</span>
                <label for="schedule-date" class="text-[10px] sm:text-xs font-bold text-gray-400 uppercase tracking-wider">切換日期:</label>
            </div>
            <div class="flex items-center gap-2">
                <input type="date" id="schedule-date" onchange="onDateChange()" class="bg-gray-950 border border-gray-700 rounded-lg sm:rounded-xl px-2 sm:px-3 py-1 sm:py-1.5 text-xs sm:text-sm font-mono font-bold text-cyan-400 focus:outline-none focus:border-cyan-500 transition-colors w-36 sm:w-auto">
                <button onclick="resetToToday()" class="bg-gray-800 hover:bg-gray-700 text-gray-300 hover:text-white text-[10px] sm:text-xs font-bold px-2.5 sm:px-3 py-1.5 sm:py-2 rounded-lg sm:rounded-xl border border-gray-700 transition cursor-pointer whitespace-nowrap active:scale-95">今日</button>
            </div>
        </div>
    </header>

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 sm:gap-6 flex-grow overflow-y-auto md:overflow-hidden pr-1 pb-4 md:pb-0 no-scrollbar">
        
        <div class="flex flex-col gap-4 sm:gap-6 h-full">
            <div class="bg-gray-900 p-4 rounded-xl sm:rounded-2xl border border-gray-800 shadow-xl flex flex-col justify-start shrink-0">
                <div class="text-left mb-3 sm:mb-4 border-b border-gray-800 pb-2">
                    <span class="bg-red-500/10 text-red-400 text-[10px] font-bold px-2 py-0.5 rounded-full uppercase tracking-widest">Tools</span>
                    <h2 class="text-md sm:text-lg font-bold text-gray-200 mt-1">教練專用工具箱</h2>
                </div>
                <div class="grid grid-cols-3 md:grid-cols-1 gap-2 sm:gap-3">
                    <button onclick="openTool('timer-modal')" class="p-3 sm:p-4 bg-gradient-to-r from-blue-600 to-cyan-600 hover:from-blue-500 hover:to-cyan-500 text-white rounded-xl font-bold shadow-md transition transform hover:-translate-y-0.5 active:scale-95 cursor-pointer flex flex-col md:flex-row items-center justify-center md:justify-start gap-1 sm:gap-3">
                        <span class="text-xl sm:text-2xl">⏱️</span>
                        <p class="font-black text-[10px] sm:text-sm md:text-md leading-none text-center md:text-left">計時器</p>
                    </button>
                    <button onclick="openTool('tt-modal')" class="p-3 sm:p-4 bg-gradient-to-r from-emerald-600 to-teal-600 hover:from-emerald-500 hover:to-teal-500 text-white rounded-xl font-bold shadow-md transition transform hover:-translate-y-0.5 active:scale-95 cursor-pointer flex flex-col md:flex-row items-center justify-center md:justify-start gap-1 sm:gap-3">
                        <span class="text-xl sm:text-2xl">🚴</span>
                        <p class="font-black text-[10px] sm:text-sm md:text-md leading-none text-center md:text-left">個人計時</p>
                    </button>
                    <button onclick="openTool('omnium-modal')" class="p-3 sm:p-4 bg-gradient-to-r from-purple-600 to-indigo-600 hover:from-purple-500 hover:to-indigo-500 text-white rounded-xl font-bold shadow-md transition transform hover:-translate-y-0.5 active:scale-95 cursor-pointer flex flex-col md:flex-row items-center justify-center md:justify-start gap-1 sm:gap-3">
                        <span class="text-xl sm:text-2xl">📊</span>
                        <p class="font-black text-[10px] sm:text-sm md:text-md leading-none text-center md:text-left">全能計算</p>
                    </button>
                </div>
            </div>

            <div class="bg-gray-900 p-4 rounded-xl sm:rounded-2xl border border-gray-800 shadow-xl flex flex-col md:flex-grow md:min-h-0 md:max-h-[50%] overflow-hidden">
                <div class="text-left mb-3 border-b border-gray-800 pb-2 shrink-0">
                    <span class="bg-cyan-500/10 text-cyan-400 text-[9px] sm:text-[10px] font-bold px-2 py-0.5 rounded-full uppercase tracking-widest">Schedule Search</span>
                    <h2 class="text-md sm:text-lg font-bold text-gray-200 mt-0.5">教練月份速查</h2>
                </div>
                
                <div class="grid grid-cols-2 md:grid-cols-1 gap-2.5 mb-3 shrink-0">
                    <div>
                        <label class="block text-[9px] sm:text-[10px] font-bold text-gray-400 uppercase tracking-wider mb-1">選擇月份</label>
                        <select id="month-dropdown-select" onchange="manageCoachSearchFlow('init')" class="w-full bg-gray-950 border border-gray-700 rounded-lg sm:rounded-xl px-2.5 py-1.5 sm:py-2 text-xs font-bold text-white focus:outline-none focus:border-cyan-500 transition-colors cursor-pointer">
                        </select>
                    </div>
                    <div>
                        <label class="block text-[9px] sm:text-[10px] font-bold text-gray-400 uppercase tracking-wider mb-1">選擇教練</label>
                        <select id="coach-dropdown-select" onchange="manageCoachSearchFlow('render')" class="w-full bg-gray-950 border border-gray-700 rounded-lg sm:rounded-xl px-2.5 py-1.5 sm:py-2 text-xs font-bold text-white focus:outline-none focus:border-cyan-500 transition-colors cursor-pointer" disabled>
                            <option value="">-- 請先選擇月份 --</option>
                        </select>
                    </div>
                </div>

                <div id="coach-search-results" class="space-y-2 overflow-y-auto max-h-[220px] md:max-h-none pr-1 text-xs no-scrollbar flex-grow">
                    <p class="text-gray-500 italic text-center py-4">請依序選取月份與教練...</p>
                </div>
            </div>
        </div>

        <div class="md:col-span-1 lg:col-span-2 xl:col-span-3 grid grid-cols-1 lg:grid-cols-2 gap-4 sm:gap-6 h-full md:overflow-hidden">
            <div class="bg-gray-900 p-4 sm:p-5 rounded-xl sm:rounded-2xl border border-gray-800 shadow-xl flex flex-col md:h-full overflow-hidden">
                <h2 class="text-xs sm:text-sm font-bold mb-3 sm:mb-4 text-cyan-400 flex items-center gap-2 tracking-wide uppercase border-b border-gray-800 pb-2 shrink-0">
                    <span class="w-2.5 h-2.5 rounded-full bg-cyan-500 animate-pulse"></span>
                    <span id="youth-title-date">中長青訓 項目</span>
                </h2>
                <div id="youth-schedule-list" class="space-y-4 overflow-y-auto flex-grow max-h-[400px] md:max-h-none pr-1 no-scrollbar">
                    <p class="text-gray-500 italic text-sm animate-pulse">Loading...</p>
                </div>
            </div>

            <div class="bg-gray-900 p-4 sm:p-5 rounded-xl sm:rounded-2xl border border-gray-800 shadow-xl flex flex-col md:h-full overflow-hidden">
                <h2 class="text-xs sm:text-sm font-bold mb-3 sm:mb-4 text-amber-400 flex items-center gap-2 tracking-wide uppercase border-b border-gray-800 pb-2 shrink-0">
                    <span class="w-2.5 h-2.5 rounded-full bg-amber-500 animate-pulse"></span>
                    <span id="stars-title-date">明日之星 項目</span>
                </h2>
                <div id="stars-schedule-list" class="space-y-4 overflow-y-auto flex-grow max-h-[400px] md:max-h-none pr-1 no-scrollbar">
                    <p class="text-gray-500 italic text-sm animate-pulse">Loading...</p>
                </div>
            </div>
        </div>
    </div>

    <div id="timer-modal" class="hidden fixed inset-0 bg-black/95 flex-col pt-14 z-50 overscroll-none">
        <div class="absolute top-2 left-3 right-3 z-[9999] flex items-center justify-between gap-2 bg-gray-900/90 backdrop-blur-md p-2 rounded-xl border border-gray-800">
            <span class="text-[10px] text-gray-400 px-1 font-mono">組件: timer.html</span>
            <button onclick="closeTool('timer-modal')" class="text-white text-xs font-bold bg-red-600 hover:bg-red-500 px-3 py-1.5 rounded-lg shadow-lg cursor-pointer transition active:scale-95">✕ 關閉</button>
        </div>
        <iframe src="./timer.html" class="w-full h-full border-none bg-transparent"></iframe>
    </div>

    <div id="tt-modal" class="hidden fixed inset-0 bg-black/95 flex-col pt-14 z-50 overscroll-none">
        <div class="absolute top-2 left-3 right-3 z-[9999] flex items-center justify-between gap-2 bg-gray-900/90 backdrop-blur-md p-2 rounded-xl border border-gray-800">
            <span class="text-[10px] text-gray-400 px-1 font-mono">組件: tt.html</span>
            <button onclick="closeTool('tt-modal')" class="text-white text-xs font-bold bg-red-600 hover:bg-red-500 px-3 py-1.5 rounded-lg shadow-lg cursor-pointer transition active:scale-95">✕ 關閉</button>
        </div>
        <iframe src="./tt.html" class="w-full h-full border-none bg-transparent"></iframe>
    </div>

    <div id="omnium-modal" class="hidden fixed inset-0 bg-black/95 flex-col pt-14 z-50 overscroll-none">
        <div class="absolute top-2 left-3 right-3 z-[9999] flex items-center justify-between gap-2 bg-gray-900/90 backdrop-blur-md p-2 rounded-xl border border-gray-800">
            <span class="text-[10px] text-gray-400 px-1 font-mono">組件: omnium.html</span>
            <button onclick="closeTool('omnium-modal')" class="text-white text-xs font-bold bg-red-600 hover:bg-red-500 px-3 py-1.5 rounded-lg shadow-lg cursor-pointer transition active:scale-95">✕ 關閉</button>
        </div>
        <iframe src="./omnium.html" class="w-full h-full border-none bg-transparent"></iframe>
    </div>

    <script>
        function openTool(id) { 
            const targetModal = document.getElementById(id);
            if (targetModal) {
                targetModal.classList.add('modal-active'); 
                document.body.classList.add('overflow-hidden'); // 鎖定背景滾動
                const iframe = targetModal.querySelector('iframe');
                if (iframe && iframe.src) { iframe.src = iframe.src; }
            }
        }
        
        function closeTool(id) { 
            const targetModal = document.getElementById(id);
            if (targetModal) {
                targetModal.classList.remove('modal-active'); 
                document.body.classList.remove('overflow-hidden'); // 解鎖背景滾動
            }
        }

        // ================= 課表與排班主程式 =================
        const dateInput = document.getElementById('schedule-date');
        const today = new Date();
        dateInput.value = `${today.getFullYear()}-${String(today.getMonth() + 1).padStart(2, '0')}-${String(today.getDate()).padStart(2, '0')}`;

        const SPREADSHEET_BASE = 'https://docs.google.com/spreadsheets/d/1PcTlsCTDaTchMICMx8IQ_AgwHZszINgqxysK_ssWc2k/export?format=csv';
        const GID_ATTENDANCE = '0';            
        const GID_YOUTH_PROGRAM = '1879963109'; 
        const GID_STARS_PROGRAM = '92551528';   

        let cachedAttendanceRows = [];

        function initMonthOptions() {
            const monthSelect = document.getElementById('month-dropdown-select');
            monthSelect.innerHTML = '';
            const curMonth = today.getMonth() + 1;
            for(let m=1; m<=12; m++) {
                const opt = document.createElement('option');
                opt.value = m;
                opt.textContent = `${m} 月份`;
                if(m === curMonth) opt.selected = true;
                monthSelect.appendChild(opt);
            }
        }
        initMonthOptions();

        function getSelectedYear() {
            if (dateInput && dateInput.value) return dateInput.value.split('-')[0];
            return String(new Date().getFullYear());
        }

        function onDateChange() { fetchSchedule(); }
        
        function syncAndViewDate(isoDateStr) {
            dateInput.value = isoDateStr;
            fetchSchedule();
            // 在行動端裝置上，點擊後平滑滾動回頁面頂部以看查訓練項目
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function resetToToday() {
            const t = new Date();
            dateInput.value = `${t.getFullYear()}-${String(t.getMonth() + 1).padStart(2, '0')}-${String(t.getDate()).padStart(2, '0')}`;
            fetchSchedule();
        }

        function cleanToNumericFingerprint(str) {
            if (!str) return '';
            const numbers = str.match(/\d+/g);
            if (!numbers || numbers.length < 2) return '';
            let year = '', month = '', day = '';
            if (numbers.length >= 3) {
                year = numbers[0].length === 4 ? numbers[0] : '20' + numbers[0];
                month = String(parseInt(numbers[1], 10));
                day = String(parseInt(numbers[2], 10));
            } else if (numbers.length === 2) {
                year = getSelectedYear(); 
                month = String(parseInt(numbers[0], 10));
                day = String(parseInt(numbers[1], 10));
            }
            return `${year}${month}${day}`;
        }

        function parseCellToDateObj(str) {
            if (!str) return null;
            const numbers = str.match(/\d+/g);
            if (!numbers || numbers.length < 2) return null;
            let year = parseInt(getSelectedYear(), 10), month = 0, day = 1;
            if (numbers.length >= 3) {
                year = numbers[0].length === 4 ? parseInt(numbers[0], 10) : parseInt('20' + numbers[0], 10);
                month = parseInt(numbers[1], 10) - 1;
                day = parseInt(numbers[2], 10);
            } else if (numbers.length === 2) {
                year = parseInt(getSelectedYear(), 10); 
                month = parseInt(numbers[0], 10) - 1;
                day = parseInt(numbers[1], 10);
            }
            return new Date(year, month, day);
        }

        function manageCoachSearchFlow(action) {
            const monthSelect = document.getElementById('month-dropdown-select');
            const coachDropdown = document.getElementById('coach-dropdown-select');
            const resultBox = document.getElementById('coach-search-results');
            const selectedMonth = parseInt(monthSelect.value, 10);
            
            if (action === 'init') {
                resultBox.innerHTML = '<p class="text-gray-500 italic text-center py-6">請選擇教練姓名...</p>';
                if (!selectedMonth || cachedAttendanceRows.length === 0) return;

                let monthCoachSet = new Set();
                for (let i = 0; i < cachedAttendanceRows.length; i++) {
                    const row = cachedAttendanceRows[i];
                    if (!row || row.length < 2) continue;
                    const dateStr = row[0] ? row[0].trim() : '';
                    if (!dateStr || dateStr.includes('Date') || dateStr.includes('日期')) continue;
                    
                    const cellDateObj = parseCellToDateObj(dateStr);
                    if (cellDateObj && (cellDateObj.getMonth() + 1) === selectedMonth) {
                        [2, 3, 5, 6, 8, 9].forEach(idx => {
                            if (row[idx]) {
                                String(row[idx]).split(/[\s,/\、，]+/).forEach(name => {
                                    const cleanName = name.trim();
                                    if (cleanName.length > 0 && cleanName.length < 15 && !cleanName.includes('-') && !/^\d+$/.test(cleanName)) {
                                        monthCoachSet.add(cleanName);
                                    }
                                });
                            }
                        });
                    }
                }

                const sortedCoaches = Array.from(monthCoachSet).sort((a, b) => a.localeCompare(b, 'zh-Hant'));
                coachDropdown.innerHTML = '<option value="">-- 請選擇教練 --</option>';
                
                if (sortedCoaches.length > 0) {
                    coachDropdown.disabled = false;
                    sortedCoaches.forEach(coach => {
                        const option = document.createElement('option');
                        option.value = coach;
                        option.textContent = coach;
                        coachDropdown.appendChild(option);
                    });
                } else {
                    coachDropdown.disabled = true;
                    coachDropdown.innerHTML = '<option value="">-- 該月份無排班數據 --</option>';
                }
            }
            
            if (action === 'render') {
                const selectedCoach = coachDropdown.value.trim().toLowerCase();
                if (!selectedCoach) {
                    resultBox.innerHTML = '<p class="text-gray-500 italic text-center py-6">請選擇教練姓名...</p>';
                    return;
                }

                let monthMatches = [];
                for (let i = 0; i < cachedAttendanceRows.length; i++) {
                    const row = cachedAttendanceRows[i];
                    if (!row || row.length < 2) continue;
                    const dateStr = row[0] ? row[0].trim() : '';
                    const weekdayStr = row[1] ? row[1].trim() : '';
                    if (!dateStr || dateStr.includes('Date') || dateStr.includes('日期')) continue;
                    
                    const cellDateObj = parseCellToDateObj(dateStr);
                    if (cellDateObj && (cellDateObj.getMonth() + 1) === selectedMonth) {
                        const youthBlock = ((row[2]||'') + (row[3]||'')).toLowerCase();
                        const starsBlock = ((row[5]||'') + (row[6]||'') + (row[8]||'') + (row[9]||'')).toLowerCase();

                        let roles = [];
                        if (youthBlock.includes(selectedCoach)) roles.push('<span class="text-cyan-400 font-bold">青訓</span>');
                        if (starsBlock.includes(selectedCoach)) roles.push('<span class="text-amber-400 font-bold">明日之星</span>');

                        if (roles.length > 0) {
                            const isoDate = `${cellDateObj.getFullYear()}-${String(cellDateObj.getMonth() + 1).padStart(2, '0')}-${String(cellDateObj.getDate()).padStart(2, '0')}`;
                            monthMatches.push({
                                timestamp: cellDateObj.getTime(),
                                isoDate: isoDate,
                                dateDisplay: dateStr,
                                weekday: weekdayStr,
                                duty: roles.join(' + ')
                            });
                        }
                    }
                }

                monthMatches.sort((a, b) => a.timestamp - b.timestamp);

                if (monthMatches.length > 0) {
                    resultBox.innerHTML = monthMatches.map(m => `
                        <div onclick="syncAndViewDate('${m.isoDate}')" class="bg-gray-950 p-2.5 rounded-xl border border-gray-850 flex justify-between items-center hover:border-cyan-500/50 hover:bg-gray-900 transition active:scale-98 shadow-inner cursor-pointer group animate-fadeIn">
                            <div>
                                <p class="font-mono font-bold text-gray-300 group-hover:text-cyan-400 transition text-[11px]">${m.dateDisplay}</p>
                                <p class="text-[10px] text-gray-500 font-medium">${m.weekday}</p>
                            </div>
                            <div class="text-right text-[11px] flex flex-col items-end gap-1">
                                <div>${m.duty}</div>
                                <span class="text-[8px] bg-gray-900 text-gray-500 group-hover:bg-cyan-950 group-hover:text-cyan-400 px-1.5 py-0.2 rounded border border-gray-800 transition">點擊查看 ➔</span>
                            </div>
                        </div>
                    `).join('');
                } else {
                    resultBox.innerHTML = `<p class="text-gray-500 italic text-center py-8">🏖️ 該教練在 ${selectedMonth} 月份無排班紀錄。</p>`;
                }
            }
        }

        async function fetchSchedule() {
            try {
                const selectedDateVal = dateInput.value;
                if (!selectedDateVal) return;
                
                const parts = selectedDateVal.split('-');
                const sYear = parseInt(parts[0], 10);
                const sMonth = parseInt(parts[1], 10);
                const sDay = parseInt(parts[2], 10);
                const targetDateObj = new Date(sYear, sMonth - 1, sDay);

                const columnMapping = [7, 1, 2, 3, 4, 5, 6]; 
                const targetColumnIdx = columnMapping[targetDateObj.getDay()];

                const weekDaysChinese = ["星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六"];
                const targetChineseWeekday = weekDaysChinese[targetDateObj.getDay()];
                
                const sheetDateTag = `${sDay}-${sMonth}月`;
                const slashDateTag = `${sDay}/${sMonth}`;
                const targetFingerprint = `${sYear}${sMonth}${sDay}`;

                document.getElementById('youth-title-date').innerText = `中長青訓 (${sMonth}月${sDay}日 ${targetChineseWeekday})`;
                document.getElementById('stars-title-date').innerText = `明日之星 (${sMonth}月${sDay}日 ${targetChineseWeekday})`;

                const youthContainer = document.getElementById('youth-schedule-list');
                const starsContainer = document.getElementById('stars-schedule-list');
                youthContainer.innerHTML = '<p class="text-gray-500 italic text-sm animate-pulse">Loading...</p>'; 
                starsContainer.innerHTML = '<p class="text-gray-500 italic text-sm animate-pulse">Loading...</p>'; 

                const [resAttendance, resProgram, resStars] = await Promise.all([
                    fetch(`${SPREADSHEET_BASE}&gid=${GID_ATTENDANCE}`),
                    fetch(`${SPREADSHEET_BASE}&gid=${GID_YOUTH_PROGRAM}`),
                    fetch(`${SPREADSHEET_BASE}&gid=${GID_STARS_PROGRAM}`)
                ]);

                const textAttendance = await resAttendance.text();
                const textProgram = await resProgram.text();
                const textStars = await resStars.text();

                cachedAttendanceRows = parseRobustCSV(textAttendance);
                const rowsProgram = parseRobustCSV(textProgram);
                const rowsStars = parseRobustCSV(textStars);

                manageCoachSearchFlow('init');

                youthContainer.innerHTML = ''; 
                starsContainer.innerHTML = ''; 

                let youthDisplayCoach = '未排班';
                let starsDisplayCoach = '未排班';

                for (let i = 0; i < cachedAttendanceRows.length; i++) {
                    const row = cachedAttendanceRows[i];
                    if (!row || row.length === 0) continue;
                    const cellA = row[0] ? String(row[0]).trim() : '';
                    if (!cellA) continue;

                    if (cleanToNumericFingerprint(cellA) === targetFingerprint) {
                        let youthCoachChinese = row[2] ? String(row[2]).trim().replace(/\r?\n|\r/g, ' ') : ''; 
                        let youthCoachEnglish = row[3] ? String(row[3]).trim().replace(/\r?\n|\r/g, ' ') : ''; 
                        if (youthCoachChinese || youthCoachEnglish) {
                            youthDisplayCoach = (youthCoachChinese && youthCoachEnglish) ? `${youthCoachChinese}, ${youthCoachEnglish}` : (youthCoachChinese || youthCoachEnglish);
                        }

                        let starsCoachParts = [];
                        [5, 6, 8, 9].forEach(idx => {
                            if (row[idx] && String(row[idx]).trim().length > 0) {
                                starsCoachParts.push(String(row[idx]).trim().replace(/\r?\n|\r/g, ' '));
                            }
                        });
                        if (starsCoachParts.length > 0) {
                            starsDisplayCoach = starsCoachParts.join(" / ");
                        }
                        break; 
                    }
                }

                // 中長青訓課表提取
                let youthFound = false;
                let finalYouthContent = "";
                for (let r = 0; r < rowsProgram.length; r++) {
                    const row = rowsProgram[r];
                    if (!row || row.length <= targetColumnIdx) continue;
                    const cellText = row[targetColumnIdx] ? String(row[targetColumnIdx]).trim() : '';

                    if (cellText === sheetDateTag || cellText.startsWith(sheetDateTag) || cellText === slashDateTag) {
                        let tempLines = [];
                        for (let offset = 1; offset < 30; offset++) {
                            const nextRowIdx = r + offset;
                            if (nextRowIdx >= rowsProgram.length) break; 
                            const nextRow = rowsProgram[nextRowIdx];
                            if (!nextRow || nextRow.length <= targetColumnIdx) continue;
                            
                            const targetCellText = nextRow[targetColumnIdx] ? String(nextRow[targetColumnIdx]).trim() : '';
                            if (targetCellText.includes('-') && (targetCellText.includes('月') || targetCellText.includes('LEAVE') || targetCellText.length < 7)) {
                                if (targetCellText !== cellText && (targetCellText.includes('月') || targetCellText.includes('-'))) break; 
                            }
                            if (targetCellText.length > 0 && !targetCellText.includes('中長青訓')) tempLines.push(targetCellText);
                        }
                        if (tempLines.length > 0) {
                            youthFound = true;
                            finalYouthContent = tempLines.join("\n\n");
                            break; 
                        }
                    }
                }

                // 明日之星流水帳課表提取
                let starsFound = false;
                let finalStarsContent = "";
                for (let r = 0; r < rowsStars.length; r++) {
                    const row = rowsStars[r];
                    if (!row || row.length < 1) continue; 
                    const cellA = row[0] ? String(row[0]).trim() : '';
                    if (!cellA) continue;

                    if (cleanToNumericFingerprint(cellA) === targetFingerprint) {
                        const cellC = row[2] ? String(row[2]).trim() : '';
                        if (cellC) {
                            starsFound = true;
                            finalStarsContent = cellC;
                            break; 
                        }
                    }
                }

                // UI 渲染
                if (youthFound && finalYouthContent.trim().length > 0) {
                    const isLeave = finalYouthContent.toUpperCase().includes('LEAVE');
                    youthContainer.innerHTML = `
                        <div class="bg-gray-950 p-4 rounded-xl border-l-4 ${isLeave ? 'border-orange-500' : 'border-cyan-500'} shadow-md animate-fadeIn">
                            <div class="flex justify-between items-center mb-3 border-b border-gray-800 pb-2">
                                <span class="text-xs font-bold text-gray-100 bg-gray-800 px-2.5 py-1 rounded border border-gray-700">
                                    當日教練: <span class="text-yellow-400 font-extrabold">${youthDisplayCoach}</span>
                                </span>
                            </div>
                            <h4 class="font-black text-white text-base mb-2">當日訓練菜單項目</h4>
                            <div class="text-xs text-gray-200 bg-gray-900/80 p-3 rounded-lg border border-gray-850 font-mono break-words leading-relaxed whitespace-pre-wrap text-left">${finalYouthContent.trim()}</div>
                        </div>
                    `;
                } else {
                    youthContainer.innerHTML = `
                        <div class="text-center py-12 text-gray-600 border border-dashed border-gray-800 rounded-xl">
                            <span class="text-xl block mb-1">🏖️</span>
                            <p class="text-xs italic">所選日期 中長青訓目前無排程數據。</p>
                        </div>`;
                }

                if (starsFound && finalStarsContent.trim().length > 0) {
                    const isLeave = finalStarsContent.toUpperCase().includes('LEAVE');
                    starsContainer.innerHTML = `
                        <div class="bg-gray-950 p-4 rounded-xl border-l-4 ${isLeave ? 'border-orange-500' : 'border-amber-500'} shadow-md animate-fadeIn">
                            <div class="flex justify-between items-center mb-3 border-b border-gray-800 pb-2">
                                <span class="text-xs font-bold text-gray-100 bg-gray-800 px-2.5 py-1 rounded border border-gray-700">
                                    當日教練 : <span class="text-amber-400 font-extrabold">${starsDisplayCoach}</span>
                                </span>
                            </div>
                            <h4 class="font-black text-white text-base mb-2">當日訓練菜單項目 (Train)</h4>
                            <div class="text-xs text-gray-200 bg-gray-900/80 p-3 rounded-lg border border-gray-850 font-mono break-words leading-relaxed whitespace-pre-wrap text-left">${finalStarsContent.trim()}</div>
                        </div>
                    `;
                } else {
                    starsContainer.innerHTML = `
                        <div class="bg-gray-950 p-4 rounded-xl border border-l-4 border-amber-500/40 shadow-md mb-3">
                            <p class="text-xs text-gray-400">當日指派教練 </p>
                            <p class="text-xl font-black text-amber-400 mt-1">${starsDisplayCoach}</p>
                        </div>
                        <div class="text-center py-8 text-gray-600 border border-dashed border-gray-800 rounded-xl">
                            <span class="text-xl block mb-1">🏖️</span>
                            <p class="text-xs italic">所選日期 明日之星流水帳中無訓練菜單。</p>
                        </div>`;
                }

            } catch (error) {
                console.error(error);
            }
        }

        function parseRobustCSV(text) {
            let insideQuote = false; let entries = [[]]; let currentRow = 0; let currentCell = '';
            for (let i = 0; i < text.length; i++) {
                let c = text[i]; let next = text[i+1];
                if (c === '"') {
                    if (insideQuote && next === '"') { currentCell += '"'; i++; } else { insideQuote = !insideQuote; }
                } else if (c === ',' && !insideQuote) {
                    entries[currentRow].push(currentCell); currentCell = '';
                } else if ((c === '\n' || c === '\r') && !insideQuote) {
                    if (c === '\r' && next === '\n') i++; 
                    entries[currentRow].push(currentCell); currentCell = ''; entries.push([]); currentRow++;
                } else { currentCell += c; }
            }
            if (currentCell || text[text.length-1] === ',') { entries[currentRow].push(currentCell); }
            return entries.filter(r => r && r.length > 0);
        }

        fetchSchedule();
    </script>
</body>
</html>
