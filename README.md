<!DOCTYPE html>
<html lang="tr" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <!-- EKRAN ÖLÇEKLENDİRME SORUNUNU ÇÖZEN CRITICAL META ETİKETİ -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0, viewport-fit=cover">
    <title>Vardiya ve İzin Takip Sistemi</title>
    
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <meta name="theme-color" content="#f8fafc">
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/3652/3652191.png">
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: { extend: { fontFamily: { sans: ['Plus Jakarta Sans', 'sans-serif'] } } }
        }
    </script>

    <style>
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            -webkit-tap-highlight-color: transparent; 
        }
        .app-section { display: none; animation: slideUpFade 0.3s ease-out; }
        .app-section.active { display: block; }
        
        @keyframes slideUpFade { 
            from { opacity: 0; transform: translateY(10px); } 
            to { opacity: 1; transform: translateY(0); } 
        }
        
        ::placeholder { color: #94a3b8; opacity: 1; }
        .dark ::placeholder { color: #64748b; }
        
        /* Tarih Kutusunun Ekrana Yayılması İçin Kesin Kurallar */
        input[type="date"], input[type="number"] { 
            -webkit-appearance: none; 
            width: 100%; 
            min-width: 0; 
            box-sizing: border-box;
            font-size: 16px; /* iOS'ta otomatik zoom'u engeller */
        }
        .dark input[type="date"]::-webkit-calendar-picker-indicator { filter: invert(1); opacity: 0.7; }
        ::-webkit-scrollbar { display: none; }
        
        .glass-card {
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
        }
        .dark .glass-card {
            background: rgba(30, 41, 59, 0.9);
        }
    </style>
</head>
<body class="bg-slate-50 dark:bg-slate-900 text-slate-800 dark:text-slate-100 min-h-screen pb-16 transition-colors duration-300">

    <!-- Konteyner padding ve max genişliği mobil için optimize edildi -->
    <div class="max-w-md mx-auto w-full px-4 sm:px-6 pt-6">
        
        <header class="mb-8 flex items-center justify-between">
            <div>
                <h1 class="text-3xl font-extrabold tracking-tight bg-gradient-to-r from-blue-600 to-indigo-600 dark:from-blue-400 dark:to-indigo-400 bg-clip-text text-transparent pb-1">Vardiya Akışı</h1>
                <p class="text-xs font-bold text-slate-500 dark:text-slate-400 uppercase tracking-widest mt-1">Akıllı Asistan</p>
            </div>
            <button onclick="toggleTheme()" class="p-3.5 rounded-2xl bg-white dark:bg-slate-800 shadow-sm border border-slate-100 dark:border-slate-700 active:scale-95 transition-transform">
                <span id="theme-icon" class="text-xl leading-none block">🌙</span>
            </button>
        </header>

        <div id="section-menu" class="app-section active space-y-4">
            
            <div class="p-5 rounded-3xl shadow-lg shadow-blue-500/10 dark:shadow-none bg-gradient-to-br from-blue-600 to-indigo-600 text-white relative overflow-hidden mb-2">
                <div class="absolute -right-6 -top-6 w-32 h-32 bg-white/10 rounded-full blur-2xl"></div>
                <div class="relative z-10 flex justify-between items-center">
                    <div>
                        <p class="text-[11px] font-bold uppercase tracking-widest text-blue-100 mb-1.5" id="today-date-text">Bugün</p>
                        <h2 class="text-xl font-black leading-tight" id="today-shift-text">Yükleniyor...</h2>
                    </div>
                    <div class="w-14 h-14 rounded-2xl bg-white/20 flex items-center justify-center text-3xl backdrop-blur-md" id="today-icon">
                        ⏳
                    </div>
                </div>
            </div>

            <button onclick="showSection('takvim-aylar')" class="w-full p-5 glass-card rounded-3xl shadow-sm border border-slate-200/80 dark:border-slate-700/80 transition-all flex items-center justify-between active:scale-[0.98]">
                <div class="text-left">
                    <span class="text-3xl block mb-2">📅</span>
                    <h2 class="font-bold text-lg text-slate-800 dark:text-slate-100">Aylık Vardiya Takvimi</h2>
                    <p class="text-sm text-slate-500 dark:text-slate-400 mt-1">İstatistikler ve Resmi Tatiller</p>
                </div>
                <div class="w-10 h-10 rounded-full bg-blue-50 dark:bg-blue-900/40 flex items-center justify-center text-blue-600 dark:text-blue-400 font-bold">→</div>
            </button>
            
            <button onclick="showSection('izin-hesap')" class="w-full p-5 glass-card rounded-3xl shadow-sm border border-slate-200/80 dark:border-slate-700/80 transition-all flex items-center justify-between active:scale-[0.98]">
                <div class="text-left">
                    <span class="text-3xl block mb-2">✈️</span>
                    <h2 class="font-bold text-lg text-slate-800 dark:text-slate-100">İzin Planlayıcı</h2>
                    <p class="text-sm text-slate-500 dark:text-slate-400 mt-1">Gelişmiş bitiş tarihi hesabı</p>
                </div>
                <div class="w-10 h-10 rounded-full bg-purple-50 dark:bg-purple-900/40 flex items-center justify-center text-purple-600 dark:text-purple-400 font-bold">→</div>
            </button>
            
            <button onclick="showSection('mesai-hesap')" class="w-full p-5 glass-card rounded-3xl shadow-sm border border-slate-200/80 dark:border-slate-700/80 transition-all flex items-center justify-between active:scale-[0.98]">
                <div class="text-left">
                    <span class="text-3xl block mb-2">💰</span>
                    <h2 class="font-bold text-lg text-slate-800 dark:text-slate-100">Mesai ve Ek Kazanç</h2>
                    <p class="text-sm text-slate-500 dark:text-slate-400 mt-1">Özel çarpanlı hakediş tablosu</p>
                </div>
                <div class="w-10 h-10 rounded-full bg-emerald-50 dark:bg-emerald-900/40 flex items-center justify-center text-emerald-600 dark:text-emerald-400 font-bold">→</div>
            </button>
        </div>

        <div id="section-takvim-aylar" class="app-section space-y-6">
            <button onclick="showSection('menu')" class="flex items-center gap-2 text-slate-600 dark:text-slate-300 font-bold text-sm bg-white dark:bg-slate-800 px-5 py-3 rounded-full shadow-sm border border-slate-200 dark:border-slate-700 w-max active:scale-95">
                <span>←</span> Ana Menü
            </button>
            <div id="month-list" class="grid grid-cols-2 sm:grid-cols-3 gap-3"></div>
        </div>

        <div id="section-takvim-detay" class="app-section space-y-5">
            <div class="flex items-center justify-between glass-card p-2 pr-3 rounded-2xl shadow-sm border border-slate-200 dark:border-slate-700">
                <button onclick="showSection('takvim-aylar')" class="flex items-center gap-2 text-slate-600 dark:text-slate-300 font-bold text-sm px-4 py-2.5 rounded-xl hover:bg-slate-100 dark:hover:bg-slate-800 active:scale-95">
                    <span>←</span> Geri
                </button>
                <div class="flex items-center gap-3">
                    <h2 id="month-name" class="text-lg font-black text-blue-600 dark:text-blue-400"></h2>
                    <button onclick="exportIcs()" class="w-10 h-10 rounded-xl bg-blue-50 dark:bg-blue-900/40 text-blue-600 dark:text-blue-400 flex items-center justify-center active:scale-95 shadow-sm">
                        📤
                    </button>
                </div>
            </div>
            
            <div class="grid grid-cols-3 gap-3">
                <div class="glass-card border border-emerald-200 dark:border-emerald-800/60 p-4 rounded-2xl text-center shadow-sm">
                    <span class="block text-xs font-bold text-emerald-600 dark:text-emerald-400 uppercase tracking-wider mb-1">Gündüz</span>
                    <span id="stat-gunduz" class="text-2xl font-black text-emerald-700 dark:text-emerald-300">0</span>
                </div>
                <div class="glass-card border border-indigo-200 dark:border-indigo-800/60 p-4 rounded-2xl text-center shadow-sm">
                    <span class="block text-xs font-bold text-indigo-600 dark:text-indigo-400 uppercase tracking-wider mb-1">Gece</span>
                    <span id="stat-gece" class="text-2xl font-black text-indigo-700 dark:text-indigo-300">0</span>
                </div>
                <div class="glass-card border border-slate-200 dark:border-slate-700 p-4 rounded-2xl text-center shadow-sm">
                    <span class="block text-xs font-bold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-1">İzin</span>
                    <span id="stat-izin" class="text-2xl font-black text-slate-600 dark:text-slate-300">0</span>
                </div>
            </div>

            <!-- Takvim grid aralıkları açıldı, metin boyutları iyileştirildi -->
            <div class="glass-card p-4 rounded-3xl shadow-sm border border-slate-200 dark:border-slate-700">
                <div id="calendar-grid" class="grid grid-cols-7 gap-2 text-center"></div>
            </div>

            <div id="holiday-box" class="hidden bg-rose-50 dark:bg-rose-900/20 border border-rose-200 dark:border-rose-800/60 p-5 rounded-3xl shadow-sm space-y-3">
                <h3 class="text-sm font-extrabold text-rose-600 dark:text-rose-400 uppercase tracking-wider flex items-center gap-2">
                    <span>🎉</span> Bu Ayki Özel Günler
                </h3>
                <ul id="holiday-list" class="space-y-2 text-sm font-medium text-rose-800 dark:text-rose-300"></ul>
            </div>
        </div>

        <div id="section-izin-hesap" class="app-section space-y-5">
            <button onclick="showSection('menu')" class="flex items-center gap-2 text-slate-600 dark:text-slate-300 font-bold text-sm bg-white dark:bg-slate-800 px-5 py-3 rounded-full shadow-sm border border-slate-200 dark:border-slate-700 w-max active:scale-95">
                <span>←</span> Ana Menü
            </button>
            
            <div class="glass-card p-6 rounded-3xl shadow-sm border border-slate-200 dark:border-slate-700 space-y-6">
                <div>
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-widest mb-2 ml-1">İzin Başlangıç Tarihi</label>
                    <input type="date" id="start-date" class="w-full px-5 py-4 bg-slate-50 dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-purple-500 outline-none text-base font-semibold text-slate-700 dark:text-slate-200 shadow-inner">
                </div>
                
                <div>
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-widest mb-2 ml-1">Kullanılacak İzin (Gün)</label>
                    <input type="number" id="izin-gun" placeholder="Gün sayısı giriniz" class="w-full px-5 py-4 bg-slate-50 dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-purple-500 outline-none text-base font-semibold text-slate-700 dark:text-slate-200 shadow-inner">
                </div>
                
                <button onclick="hesaplaIzin()" class="w-full py-5 bg-purple-600 hover:bg-purple-700 text-white rounded-2xl font-black text-lg active:scale-[0.98] transition-transform shadow-lg shadow-purple-500/30">
                    Bitiş Tarihini Hesapla
                </button>
                
                <div id="izin-sonuc" class="p-6 bg-purple-50 dark:bg-purple-900/30 text-purple-800 dark:text-purple-300 rounded-2xl hidden text-center border border-purple-200 dark:border-purple-800/60 shadow-sm mt-4"></div>
            </div>
        </div>

        <div id="section-mesai-hesap" class="app-section space-y-5">
            <button onclick="showSection('menu')" class="flex items-center gap-2 text-slate-600 dark:text-slate-300 font-bold text-sm bg-white dark:bg-slate-800 px-5 py-3 rounded-full shadow-sm border border-slate-200 dark:border-slate-700 w-max active:scale-95">
                <span>←</span> Ana Menü
            </button>
            
            <div class="glass-card p-6 rounded-3xl shadow-sm border border-slate-200 dark:border-slate-700 space-y-6">
                <div>
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-widest mb-2 ml-1">Toplam Mesai Saati</label>
                    <input type="number" id="mesai-saat" placeholder="Örn: 12.5" step="0.5" class="w-full px-5 py-4 bg-slate-50 dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none text-base font-semibold text-slate-700 dark:text-slate-200 shadow-inner">
                </div>
                
                <div>
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-widest mb-2 ml-1">Saatlik Ücret (TL)</label>
                    <input type="number" id="mesai-ucret" placeholder="Örn: 150" class="w-full px-5 py-4 bg-slate-50 dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none text-base font-semibold text-slate-700 dark:text-slate-200 shadow-inner">
                </div>
                
                <div>
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-widest mb-2 ml-1">Mesai Çarpanı Seçimi</label>
                    <div class="grid grid-cols-4 gap-2">
                        <button onclick="setMultiplier(1.5)" id="btn-mult-1.5" class="mult-btn py-4 bg-emerald-600 text-white rounded-2xl text-sm font-black shadow-md transition-all">1.5x</button>
                        <button onclick="setMultiplier(2.0)" id="btn-mult-2.0" class="mult-btn py-4 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 rounded-2xl text-sm font-bold border border-slate-200 dark:border-slate-700 transition-all">2.0x</button>
                        <button onclick="setMultiplier(1.0)" id="btn-mult-1.0" class="mult-btn py-4 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 rounded-2xl text-sm font-bold border border-slate-200 dark:border-slate-700 transition-all">1.0x</button>
                        <input type="number" id="mesai-carpan" oninput="customMultiplierInput()" step="0.1" class="w-full text-center px-2 py-4 bg-slate-50 dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-700 text-sm font-bold outline-none focus:bg-white dark:focus:bg-slate-800 focus:border-emerald-500 focus:ring-2 focus:ring-emerald-500 text-slate-700 dark:text-slate-200" placeholder="Özel">
                    </div>
                </div>
                
                <button onclick="hesaplaMesai()" class="w-full py-5 bg-emerald-600 hover:bg-emerald-700 text-white rounded-2xl font-black text-lg active:scale-[0.98] transition-transform shadow-lg shadow-emerald-500/30">
                    Ek Kazancı Hesapla
                </button>
                
                <div id="mesai-sonuc" class="p-6 bg-emerald-50 dark:bg-emerald-900/30 text-emerald-800 dark:text-emerald-300 rounded-2xl hidden text-center border border-emerald-200 dark:border-emerald-800/60 shadow-sm mt-4"></div>
            </div>
        </div>
    </div>

    <script>
        const TR_HOLIDAYS_2026 = {
            "0-1": "Yılbaşı", "2-19": "Ramazan Bayramı Arifesi", "2-20": "Ramazan Bayramı (1. Gün)",
            "2-21": "Ramazan Bayramı (2. Gün)", "2-22": "Ramazan Bayramı (3. Gün)", "3-23": "23 Nisan",
            "4-1": "1 Mayıs", "4-19": "19 Mayıs", "4-26": "Kurban Bayramı Arifesi",
            "4-27": "Kurban Bayramı (1. Gün)", "4-28": "Kurban Bayramı (2. Gün)", "4-29": "Kurban Bayramı (3. Gün)",
            "4-30": "Kurban Bayramı (4. Gün)", "6-15": "15 Temmuz", "7-30": "30 Ağustos Zafer Bayramı",
            "9-29": "29 Ekim Cumhuriyet Bayramı"
        };

        let selectedMultiplier = 1.5;
        let activeMonthIndex = 0;
        let activeMonthName = "";

        function toggleTheme() {
            document.documentElement.classList.toggle('dark');
            const isDark = document.documentElement.classList.contains('dark');
            localStorage.setItem('theme', isDark ? 'dark' : 'light');
            document.getElementById('theme-icon').innerText = isDark ? '☀️' : '🌙';
        }

        window.onload = () => {
            if (localStorage.getItem('theme') === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
                document.documentElement.classList.add('dark');
                document.getElementById('theme-icon').innerText = '☀️';
            }
            initTodayWidget();
        }

        function initTodayWidget() {
            const today = new Date(); 
            const d = today.getDate();
            const m = today.getMonth();
            const y = today.getFullYear();
            
            const aylar = ["Ocak", "Şubat", "Mart", "Nisan", "Mayıs", "Haziran", "Temmuz", "Ağustos", "Eylül", "Ekim", "Kasım", "Aralık"];
            document.getElementById('today-date-text').innerText = `${d} ${aylar[m]} ${y}`;
            
            if(y === 2026) {
                const shift = getShift(new Date(2026, m, d));
                document.getElementById('today-shift-text').innerText = shift === 'İzin' ? 'İzinlisin' : `${shift} Vardiyası`;
                document.getElementById('today-icon').innerText = shift === 'Gündüz' ? '☀️' : (shift === 'Gece' ? '🌙' : '🏖️');
            } else {
                document.getElementById('today-shift-text').innerText = "2026 Planı";
                document.getElementById('today-icon').innerText = "🗓️";
            }
        }

        function showSection(id) {
            document.querySelectorAll('.app-section').forEach(s => s.classList.remove('active'));
            document.getElementById('section-' + id).classList.add('active');
            window.scrollTo({ top: 0, behavior: 'smooth' });
            if(id === 'takvim-aylar') renderMonths();
        }

        function renderMonths() {
            const container = document.getElementById('month-list');
            container.innerHTML = "";
            const aylar = ["Ocak", "Şubat", "Mart", "Nisan", "Mayıs", "Haziran", "Temmuz", "Ağustos", "Eylül", "Ekim", "Kasım", "Aralık"];
            aylar.forEach((m, i) => {
                const btn = document.createElement('button');
                btn.className = "p-5 glass-card rounded-2xl shadow-sm text-sm font-bold text-slate-700 dark:text-slate-200 border border-slate-200/60 dark:border-slate-700/60 hover:border-blue-400 active:scale-95 transition-all";
                btn.innerText = m;
                btn.onclick = () => showCalendar(i, m);
                container.appendChild(btn);
            });
        }

        function getShift(d) {
            const refDate = new Date(2026, 4, 15);
            d.setHours(0,0,0,0);
            const diffDays = Math.floor((d.getTime() - refDate.getTime()) / (1000 * 60 * 60 * 24));
            const cycle = ['Gündüz', 'Gündüz', 'Gece', 'Gece', 'İzin', 'İzin'];
            let idx = (3 + diffDays) % 6;
            return cycle[idx < 0 ? idx + 6 : idx];
        }

        function showCalendar(mIndex, mName) {
            activeMonthIndex = mIndex;
            activeMonthName = mName;
            showSection('takvim-detay');
            document.getElementById('month-name').innerText = mName + " 2026";
            
            const grid = document.getElementById('calendar-grid');
            const holidayBox = document.getElementById('holiday-box');
            const holidayList = document.getElementById('holiday-list');
            
            let cGunduz = 0, cGece = 0, cIzin = 0;
            
            grid.innerHTML = `
                <div class="text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Pt</div>
                <div class="text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Sa</div>
                <div class="text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Ça</div>
                <div class="text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Pe</div>
                <div class="text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Cu</div>
                <div class="text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Ct</div>
                <div class="text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Pa</div>
            `;
            
            const daysInMonth = new Date(2026, mIndex + 1, 0).getDate();
            const firstDayIndex = (new Date(2026, mIndex, 1).getDay() + 6) % 7; 
            
            for(let i = 0; i < firstDayIndex; i++) grid.appendChild(document.createElement('div'));

            let monthlyHolidays = [];

            for(let d = 1; d <= daysInMonth; d++) {
                const shift = getShift(new Date(2026, mIndex, d));
                if (shift === 'Gündüz') cGunduz++; else if (shift === 'Gece') cGece++; else cIzin++;

                const holidayName = TR_HOLIDAYS_2026[`${mIndex}-${d}`];
                if (holidayName) monthlyHolidays.push({ day: d, name: holidayName });

                const div = document.createElement('div');
                let baseClass = "relative flex flex-col items-center justify-center py-3 rounded-xl h-16 border border-transparent shadow-sm ";
                let shiftClass = shift === 'Gündüz' ? 'bg-emerald-50 dark:bg-emerald-900/30 text-emerald-700 dark:text-emerald-400' : 
                                 shift === 'Gece' ? 'bg-indigo-50 dark:bg-indigo-900/30 text-indigo-700 dark:text-indigo-400' : 
                                 'bg-slate-50 dark:bg-slate-800 text-slate-500 dark:text-slate-400';

                let holidayIndicator = holidayName ? `<span class="absolute top-1.5 right-1.5 w-2 h-2 bg-rose-500 rounded-full"></span>` : "";
                if(holidayName) baseClass += " !border-rose-300 dark:!border-rose-500/50 ";

                div.className = baseClass + shiftClass;
                div.innerHTML = `${holidayIndicator}<span class="text-xs font-bold leading-tight">${d}</span><span class="text-[10px] font-black mt-1 tracking-tight">${shift}</span>`;
                grid.appendChild(div);
            }

            document.getElementById('stat-gunduz').innerText = cGunduz;
            document.getElementById('stat-gece').innerText = cGece;
            document.getElementById('stat-izin').innerText = cIzin;

            if (monthlyHolidays.length > 0) {
                holidayBox.classList.remove('hidden');
                holidayList.innerHTML = "";
                monthlyHolidays.forEach(h => {
                    const li = document.createElement('li');
                    li.className = "flex items-start gap-2 text-sm";
                    li.innerHTML = `<span class="text-rose-500 mt-0.5 font-black">•</span> <span><strong class="font-extrabold">${h.day} ${mName}:</strong> ${h.name}</span>`;
                    holidayList.appendChild(li);
                });
            } else holidayBox.classList.add('hidden');
        }

        function exportIcs() {
            const daysCount = new Date(2026, activeMonthIndex + 1, 0).getDate();
            let ics = ["BEGIN:VCALENDAR", "VERSION:2.0", "PRODID:-//VardiyaAkisi//TR", "CALSCALE:GREGORIAN"];
            const mFormat = (activeMonthIndex + 1).toString().padStart(2, '0');

            for (let d = 1; d <= daysCount; d++) {
                const shift = getShift(new Date(2026, activeMonthIndex, d));
                if (shift === 'Gündüz' || shift === 'Gece') {
                    const dFormat = d.toString().padStart(2, '0');
                    const baseDate = `2026${mFormat}${dFormat}`;
                    let sTime = shift === 'Gündüz' ? "080000" : "200000";
                    let eTime = shift === 'Gündüz' ? "200000" : "080000";
                    
                    let endStr = `${baseDate}T${eTime}`;
                    if (shift === 'Gece') {
                        const nDay = new Date(2026, activeMonthIndex, d + 1);
                        endStr = `${2026}${(nDay.getMonth()+1).toString().padStart(2,'0')}${nDay.getDate().toString().padStart(2,'0')}T${eTime}`;
                    }

                    ics.push("BEGIN:VEVENT", `UID:va_${baseDate}_${shift}`, `DTSTAMP:${baseDate}T000000Z`, 
                             `DTSTART;TZID=Europe/Istanbul:${baseDate}T${sTime}`, `DTEND;TZID=Europe/Istanbul:${endStr}`, 
                             `SUMMARY:${shift} Vardiyası`, "END:VEVENT");
                }
            }
            ics.push("END:VCALENDAR");
            const blob = new Blob([ics.join("\r\n")], { type: 'text/calendar' });
            const link = document.createElement('a');
            link.href = window.URL.createObjectURL(blob);
            link.download = `Vardiya_${activeMonthName}_2026.ics`;
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }

        function hesaplaIzin() {
            const startStr = document.getElementById('start-date').value;
            const gun = parseInt(document.getElementById('izin-gun').value);
            const sonucKutusu = document.getElementById('izin-sonuc');

            if(!startStr || isNaN(gun) || gun < 1) {
                sonucKutusu.classList.remove('hidden');
                sonucKutusu.innerHTML = `<span class="text-rose-600 dark:text-rose-400 font-bold text-sm">Lütfen tarih ve gün girin.</span>`;
                sonucKutusu.className = "p-6 bg-rose-50 dark:bg-rose-900/20 rounded-2xl text-center border border-rose-200 dark:border-rose-900/50 mt-4";
                return;
            }
            
            const start = new Date(startStr);
            start.setDate(start.getDate() + gun + 2 - 1);
            
            sonucKutusu.classList.remove('hidden');
            sonucKutusu.className = "p-6 bg-gradient-to-br from-purple-50 to-indigo-50 dark:from-purple-900/40 dark:to-indigo-900/40 text-purple-900 dark:text-purple-100 rounded-2xl text-center border border-purple-200 dark:border-purple-700/50 shadow-sm mt-4";
            sonucKutusu.innerHTML = `<div class="text-[11px] font-extrabold uppercase tracking-widest text-purple-500 dark:text-purple-400 mb-2">İşe Başlama Tarihi</div><div class="text-2xl font-black">${start.toLocaleDateString('tr-TR', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}</div>`;
        }

        function setMultiplier(val) {
            selectedMultiplier = val;
            document.getElementById('mesai-carpan').value = val;
            updateMultButtons();
        }

        function customMultiplierInput() {
            const val = parseFloat(document.getElementById('mesai-carpan').value);
            if(!isNaN(val)) { selectedMultiplier = val; updateMultButtons(true); }
        }

        function updateMultButtons(isCustom = false) {
            document.querySelectorAll('.mult-btn').forEach(btn => {
                btn.className = "mult-btn py-4 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-400 border border-slate-200 dark:border-slate-700 rounded-2xl text-sm font-bold transition-all";
            });
            if (!isCustom) {
                const activeBtn = document.getElementById(`btn-mult-${selectedMultiplier.toFixed(1)}`);
                if (activeBtn) activeBtn.className = "mult-btn py-4 bg-emerald-600 border border-emerald-600 text-white rounded-2xl text-sm font-black shadow-lg shadow-emerald-500/30 transition-all";
            }
        }

        function hesaplaMesai() {
            const saat = parseFloat(document.getElementById('mesai-saat').value);
            const ucret = parseFloat(document.getElementById('mesai-ucret').value);
            const sonucKutusu = document.getElementById('mesai-sonuc');

            if(isNaN(saat) || isNaN(ucret)) {
                sonucKutusu.classList.remove('hidden');
                sonucKutusu.innerHTML = `<span class="text-rose-600 dark:text-rose-400 font-bold text-sm">Lütfen geçerli saat ve ücret girin.</span>`;
                sonucKutusu.className = "p-6 bg-rose-50 dark:bg-rose-900/20 rounded-2xl text-center border border-rose-200 dark:border-rose-900/50 mt-4";
                return;
            }

            const toplam = saat * ucret * selectedMultiplier;
            
            sonucKutusu.classList.remove('hidden');
            sonucKutusu.className = "p-6 bg-gradient-to-br from-emerald-50 to-teal-50 dark:from-emerald-900/40 dark:to-teal-900/40 text-emerald-900 dark:text-emerald-100 rounded-2xl text-center border border-emerald-200 dark:border-emerald-700/50 shadow-sm mt-4";
            sonucKutusu.innerHTML = `
                <div class="text-[11px] font-extrabold uppercase tracking-widest text-emerald-600 dark:text-emerald-400 mb-2">Toplam Ek Hakediş</div>
                <div class="text-4xl font-black mb-2">${toplam.toLocaleString('tr-TR', { minimumFractionDigits: 2 })} ₺</div>
                <div class="text-xs text-emerald-700/80 dark:text-emerald-300/80 font-bold">(${saat} Saat × ${ucret} TL × ${selectedMultiplier} Çarpan)</div>
            `;
        }
    </script>
</body>
</html>