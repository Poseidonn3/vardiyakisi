
<!DOCTYPE html>
<html lang="tr" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0, viewport-fit=cover">
    <title>Vardiya ve İzin Takip Sistemi</title>
    
    <!-- PWA & Mobil Uyumluluk -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <meta name="theme-color" content="#f8fafc">
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/3652/3652191.png">
    
    <!-- Tailwind CSS ve Google Fonts -->
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
        .app-section { display: none; animation: slideUpFade 0.4s cubic-bezier(0.16, 1, 0.3, 1); }
        .app-section.active { display: block; }
        
        @keyframes slideUpFade { 
            from { opacity: 0; transform: translateY(15px); } 
            to { opacity: 1; transform: translateY(0); } 
        }
        
        ::placeholder { color: #94a3b8; opacity: 1; }
        .dark ::placeholder { color: #64748b; }
        
        /* Input Taşımlarını Engelleyen Kesin Kurallar */
        input[type="date"], input[type="number"] { 
            -webkit-appearance: none; 
            width: 100%; 
            min-width: 0; 
            box-sizing: border-box;
        }
        .dark input[type="date"]::-webkit-calendar-picker-indicator { filter: invert(1); opacity: 0.7; }
        ::-webkit-scrollbar { display: none; }
        
        /* Cam Efekti */
        .glass-card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
        }
        .dark .glass-card {
            background: rgba(30, 41, 59, 0.85);
        }
    </style>
</head>
<body class="bg-slate-50 dark:bg-slate-950 text-slate-800 dark:text-slate-100 min-h-screen pb-12 transition-colors duration-500 bg-[radial-gradient(ellipse_at_top_right,_var(--tw-gradient-stops))] from-blue-50/50 via-slate-50 to-slate-100 dark:from-slate-900 dark:via-slate-950 dark:to-slate-900">

    <div class="max-w-md mx-auto w-full px-4 sm:px-5 pt-8">
        
        <!-- ÜST BAŞLIK VE TEMA -->
        <header class="mb-8 flex items-center justify-between">
            <div>
                <h1 class="text-3xl sm:text-4xl font-extrabold tracking-tight bg-gradient-to-r from-blue-600 to-indigo-600 dark:from-blue-400 dark:to-indigo-400 bg-clip-text text-transparent pb-1">Vardiya Akışı</h1>
                <p class="text-[11px] sm:text-xs font-bold text-slate-500 dark:text-slate-400 uppercase tracking-widest">Akıllı Asistan</p>
            </div>
            <button onclick="toggleTheme()" class="p-3 sm:p-4 rounded-2xl bg-white dark:bg-slate-800 shadow-sm border border-slate-100 dark:border-slate-700 hover:scale-105 active:scale-95 transition-all duration-300">
                <span id="theme-icon" class="text-xl leading-none flex">🌙</span>
            </button>
        </header>

        <!-- ANA MENÜ -->
        <div id="section-menu" class="app-section active space-y-4">
            
            <!-- Bugünün Özeti Akıllı Kartı -->
            <div class="p-5 sm:p-6 rounded-3xl shadow-lg shadow-blue-500/20 dark:shadow-none bg-gradient-to-br from-blue-600 to-indigo-600 text-white relative overflow-hidden mb-2">
                <div class="absolute -right-4 -top-4 w-28 h-28 bg-white/10 rounded-full blur-xl"></div>
                <div class="relative z-10 flex justify-between items-center">
                    <div class="flex-1 min-w-0">
                        <p class="text-[10px] sm:text-xs font-bold uppercase tracking-wider text-blue-100 mb-1" id="today-date-text">Bugün</p>
                        <h2 class="text-xl sm:text-2xl font-black truncate" id="today-shift-text">Hesaplanıyor...</h2>
                    </div>
                    <div class="w-12 h-12 sm:w-14 sm:h-14 rounded-2xl bg-white/20 flex-shrink-0 flex items-center justify-center text-2xl sm:text-3xl backdrop-blur-sm ml-3" id="today-icon">
                        ⏳
                    </div>
                </div>
            </div>

            <!-- Menü Butonları -->
            <button onclick="showSection('takvim-aylar')" class="w-full p-5 sm:p-6 glass-card rounded-3xl shadow-sm border border-slate-200/60 dark:border-slate-700/50 hover:border-blue-400 dark:hover:border-blue-500 transition-all group flex items-center justify-between active:scale-[0.98]">
                <div class="text-left flex-1">
                    <span class="text-3xl block mb-2 drop-shadow-sm">📅</span>
                    <h2 class="font-bold text-lg text-slate-800 dark:text-slate-100">Aylık Vardiya Takvimi</h2>
                    <p class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 mt-1">İstatistikler ve Resmi Tatiller</p>
                </div>
                <div class="w-10 h-10 rounded-full bg-blue-50 dark:bg-blue-900/30 flex items-center justify-center text-blue-600 dark:text-blue-400 font-bold group-hover:translate-x-2 transition-transform shrink-0">→</div>
            </button>
            
            <button onclick="showSection('izin-hesap')" class="w-full p-5 sm:p-6 glass-card rounded-3xl shadow-sm border border-slate-200/60 dark:border-slate-700/50 hover:border-purple-400 dark:hover:border-purple-500 transition-all group flex items-center justify-between active:scale-[0.98]">
                <div class="text-left flex-1">
                    <span class="text-3xl block mb-2 drop-shadow-sm">✈️</span>
                    <h2 class="font-bold text-lg text-slate-800 dark:text-slate-100">İzin Planlayıcı</h2>
                    <p class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 mt-1">Gelişmiş bitiş tarihi hesabı</p>
                </div>
                <div class="w-10 h-10 rounded-full bg-purple-50 dark:bg-purple-900/30 flex items-center justify-center text-purple-600 dark:text-purple-400 font-bold group-hover:translate-x-2 transition-transform shrink-0">→</div>
            </button>
            
            <button onclick="showSection('mesai-hesap')" class="w-full p-5 sm:p-6 glass-card rounded-3xl shadow-sm border border-slate-200/60 dark:border-slate-700/50 hover:border-emerald-400 dark:hover:border-emerald-500 transition-all group flex items-center justify-between active:scale-[0.98]">
                <div class="text-left flex-1">
                    <span class="text-3xl block mb-2 drop-shadow-sm">💰</span>
                    <h2 class="font-bold text-lg text-slate-800 dark:text-slate-100">Mesai ve Ek Kazanç</h2>
                    <p class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 mt-1">Yol/Yemek destekli hakediş</p>
                </div>
                <div class="w-10 h-10 rounded-full bg-emerald-50 dark:bg-emerald-900/30 flex items-center justify-center text-emerald-600 dark:text-emerald-400 font-bold group-hover:translate-x-2 transition-transform shrink-0">→</div>
            </button>
        </div>

        <!-- TAKVİM: AY LİSTESİ -->
        <div id="section-takvim-aylar" class="app-section space-y-6">
            <button onclick="showSection('menu')" class="flex items-center gap-2 text-slate-500 dark:text-slate-400 hover:text-slate-800 dark:hover:text-slate-100 font-bold text-sm transition-colors glass-card px-5 py-3 rounded-full shadow-sm border border-slate-200/60 dark:border-slate-700/50 w-max active:scale-95">
                <span>←</span> Ana Menü
            </button>
            <div id="month-list" class="grid grid-cols-2 sm:grid-cols-3 gap-3 sm:gap-4"></div>
        </div>

        <!-- TAKVİM: DETAY, TATİLLER VE İSTATİSTİKLER -->
        <div id="section-takvim-detay" class="app-section space-y-5">
            <div class="flex items-center justify-between glass-card p-2 pr-3 rounded-2xl shadow-sm border border-slate-200/60 dark:border-slate-700/50">
                <button onclick="showSection('takvim-aylar')" class="flex items-center gap-2 text-slate-500 dark:text-slate-400 hover:text-slate-800 dark:hover:text-slate-100 font-bold text-sm transition-colors px-4 py-2.5 rounded-xl hover:bg-slate-100 dark:hover:bg-slate-800 active:scale-95">
                    <span>←</span> Geri
                </button>
                <div class="flex items-center gap-3">
                    <h2 id="month-name" class="text-lg sm:text-xl font-black text-blue-600 dark:text-blue-400"></h2>
                    <button onclick="exportIcs()" class="w-10 h-10 rounded-xl bg-blue-50 dark:bg-blue-900/40 text-blue-600 dark:text-blue-400 flex items-center justify-center hover:scale-105 active:scale-95 transition-all shadow-sm" title="Takvime Aktar (.ics)">
                        📤
                    </button>
                </div>
            </div>
            
            <!-- İstatistik Kartları -->
            <div class="grid grid-cols-3 gap-2 sm:gap-3">
                <div class="glass-card border border-emerald-200/60 dark:border-emerald-800/50 p-3 sm:p-4 rounded-2xl text-center shadow-sm">
                    <span class="block text-[9px] sm:text-[10px] font-bold text-emerald-600 dark:text-emerald-400 uppercase tracking-wide mb-1">Gündüz</span>
                    <span id="stat-gunduz" class="text-xl sm:text-2xl font-black text-emerald-700 dark:text-emerald-300">0</span>
                </div>
                <div class="glass-card border border-indigo-200/60 dark:border-indigo-800/50 p-3 sm:p-4 rounded-2xl text-center shadow-sm">
                    <span class="block text-[9px] sm:text-[10px] font-bold text-indigo-600 dark:text-indigo-400 uppercase tracking-wide mb-1">Gece</span>
                    <span id="stat-gece" class="text-xl sm:text-2xl font-black text-indigo-700 dark:text-indigo-300">0</span>
                </div>
                <div class="glass-card border border-slate-200/60 dark:border-slate-700/50 p-3 sm:p-4 rounded-2xl text-center shadow-sm">
                    <span class="block text-[9px] sm:text-[10px] font-bold text-slate-500 dark:text-slate-400 uppercase tracking-wide mb-1">İzin</span>
                    <span id="stat-izin" class="text-xl sm:text-2xl font-black text-slate-600 dark:text-slate-300">0</span>
                </div>
            </div>

            <!-- Takvim Izgarası -->
            <div class="glass-card p-2 sm:p-4 rounded-3xl shadow-sm border border-slate-200/60 dark:border-slate-700/50">
                <div id="calendar-grid" class="grid grid-cols-7 gap-1 sm:gap-2 text-center w-full"></div>
            </div>

            <!-- O Ayın Tatillerini Gösteren Kutu -->
            <div id="holiday-box" class="hidden bg-gradient-to-br from-rose-50 to-orange-50 dark:from-rose-950/40 dark:to-orange-950/40 border border-rose-200 dark:border-rose-800/50 p-5 rounded-3xl shadow-sm space-y-3">
                <h3 class="text-xs font-extrabold text-rose-600 dark:text-rose-400 uppercase tracking-wider flex items-center gap-2">
                    <span>🎉</span> Özel Günler
                </h3>
                <ul id="holiday-list" class="space-y-2 text-xs sm:text-sm font-medium text-rose-800 dark:text-rose-300"></ul>
            </div>
        </div>

        <!-- İZİN HESAPLAYICI -->
        <div id="section-izin-hesap" class="app-section space-y-5">
            <button onclick="showSection('menu')" class="flex items-center gap-2 text-slate-500 dark:text-slate-400 hover:text-slate-800 dark:hover:text-slate-100 font-bold text-sm transition-colors glass-card px-5 py-2.5 rounded-full shadow-sm border border-slate-200/60 dark:border-slate-700/50 w-max active:scale-95">
                <span>←</span> Ana Menü
            </button>
            
            <div class="glass-card p-5 sm:p-6 rounded-3xl shadow-sm border border-slate-200/60 dark:border-slate-700/50 space-y-5">
                <div class="w-full">
                    <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">İzin Başlangıç Tarihi</label>
                    <input type="date" id="start-date" class="w-full px-4 py-3.5 bg-slate-50/50 dark:bg-slate-900/50 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-purple-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-inner block">
                </div>
                
                <div class="w-full">
                    <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">Kullanılacak İzin (Gün)</label>
                    <input type="number" id="izin-gun" placeholder="Örn: 14" class="w-full px-4 py-3.5 bg-slate-50/50 dark:bg-slate-900/50 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-purple-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-inner block">
                </div>
                
                <button onclick="hesaplaIzin()" class="w-full py-4 mt-2 bg-gradient-to-r from-purple-600 to-indigo-600 hover:opacity-90 active:scale-[0.98] text-white rounded-2xl font-black transition-all shadow-lg shadow-purple-500/30">
                    Hesapla
                </button>
                
                <div id="izin-sonuc" class="p-5 bg-purple-50 dark:bg-purple-900/30 text-purple-800 dark:text-purple-300 rounded-2xl hidden text-center border border-purple-200 dark:border-purple-800/50 shadow-sm transition-all mt-4"></div>
            </div>
        </div>

        <!-- MESAİ VE KAZANÇ HESAPLAYICI -->
        <div id="section-mesai-hesap" class="app-section space-y-5">
            <button onclick="showSection('menu')" class="flex items-center gap-2 text-slate-500 dark:text-slate-400 hover:text-slate-800 dark:hover:text-slate-100 font-bold text-sm transition-colors glass-card px-5 py-2.5 rounded-full shadow-sm border border-slate-200/60 dark:border-slate-700/50 w-max active:scale-95">
                <span>←</span> Ana Menü
            </button>
            
            <div class="glass-card p-5 sm:p-6 rounded-3xl shadow-sm border border-slate-200/60 dark:border-slate-700/50 space-y-5">
                
                <!-- ANA MESAİ ALANLARI -->
                <div class="grid grid-cols-2 gap-3">
                    <div class="w-full">
                        <label class="block text-[10px] sm:text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">Mesai Saati</label>
                        <input type="number" id="mesai-saat" placeholder="Örn: 12.5" step="0.5" class="w-full px-3 sm:px-4 py-3.5 bg-slate-50/50 dark:bg-slate-900/50 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-inner block">
                    </div>
                    
                    <div class="w-full">
                        <label class="block text-[10px] sm:text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">Saatlik Ücret</label>
                        <input type="number" id="mesai-ucret" placeholder="Örn: 150₺" class="w-full px-3 sm:px-4 py-3.5 bg-slate-50/50 dark:bg-slate-900/50 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-inner block">
                    </div>
                </div>

                <!-- YENİ: ADETLİ YOL VE YEMEK ALANLARI -->
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 border-t border-slate-200/60 dark:border-slate-700/60 pt-5 mt-2">
                    
                    <!-- Yol Kartı -->
                    <div class="bg-slate-50/50 dark:bg-slate-800/30 p-3.5 rounded-2xl border border-slate-200/60 dark:border-slate-700/50">
                        <label class="block text-[10px] sm:text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1 flex items-center gap-1">🚗 Yol Desteği <span class="text-[9px] lowercase opacity-70 font-medium">(İsteğe bağlı)</span></label>
                        <div class="grid grid-cols-2 gap-2">
                            <input type="number" id="mesai-yol-ucret" placeholder="₺/Gün" class="w-full px-3 py-3 bg-white/60 dark:bg-slate-900/60 rounded-xl border border-slate-200/80 dark:border-slate-700/80 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-xs font-bold text-slate-700 dark:text-slate-200 shadow-sm block">
                            <input type="number" id="mesai-yol-gun" placeholder="Adet/Gün" class="w-full px-3 py-3 bg-white/60 dark:bg-slate-900/60 rounded-xl border border-slate-200/80 dark:border-slate-700/80 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-xs font-bold text-slate-700 dark:text-slate-200 shadow-sm block">
                        </div>
                    </div>
                    
                    <!-- Yemek Kartı -->
                    <div class="bg-slate-50/50 dark:bg-slate-800/30 p-3.5 rounded-2xl border border-slate-200/60 dark:border-slate-700/50">
                        <label class="block text-[10px] sm:text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1 flex items-center gap-1">🍔 Yemek Desteği <span class="text-[9px] lowercase opacity-70 font-medium">(İsteğe bağlı)</span></label>
                        <div class="grid grid-cols-2 gap-2">
                            <input type="number" id="mesai-yemek-ucret" placeholder="₺/Gün" class="w-full px-3 py-3 bg-white/60 dark:bg-slate-900/60 rounded-xl border border-slate-200/80 dark:border-slate-700/80 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-xs font-bold text-slate-700 dark:text-slate-200 shadow-sm block">
                            <input type="number" id="mesai-yemek-gun" placeholder="Adet/Gün" class="w-full px-3 py-3 bg-white/60 dark:bg-slate-900/60 rounded-xl border border-slate-200/80 dark:border-slate-700/80 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-xs font-bold text-slate-700 dark:text-slate-200 shadow-sm block">
                        </div>
                    </div>

                </div>
                
                <div class="border-t border-slate-200/60 dark:border-slate-700/60 pt-5 mt-2">
                    <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">Mesai Çarpanı Seçimi</label>
                    <div class="grid grid-cols-4 gap-2">
                        <button onclick="setMultiplier(1.5)" id="btn-mult-1.5" class="mult-btn py-3.5 bg-emerald-600 text-white rounded-xl text-xs sm:text-sm font-black shadow-md transition-all">1.5x</button>
                        <button onclick="setMultiplier(2.0)" id="btn-mult-2.0" class="mult-btn py-3.5 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 rounded-xl text-xs sm:text-sm font-bold border border-slate-200 dark:border-slate-700 transition-all">2.0x</button>
                        <button onclick="setMultiplier(1.0)" id="btn-mult-1.0" class="mult-btn py-3.5 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 rounded-xl text-xs sm:text-sm font-bold border border-slate-200 dark:border-slate-700 transition-all">1.0x</button>
                        <input type="number" id="mesai-carpan" oninput="customMultiplierInput()" step="0.1" class="w-full text-center px-1 py-3 bg-slate-50/50 dark:bg-slate-900/50 rounded-xl border border-slate-200 dark:border-slate-700 text-xs sm:text-sm font-bold outline-none focus:bg-white dark:focus:bg-slate-800 focus:border-emerald-500 focus:ring-2 focus:ring-emerald-500 text-slate-700 dark:text-slate-200 block" placeholder="Özel">
                    </div>
                </div>
                
                <button onclick="hesaplaMesai()" class="w-full py-4 mt-2 bg-gradient-to-r from-emerald-500 to-teal-600 hover:opacity-90 active:scale-[0.98] text-white rounded-2xl font-black text-lg transition-transform shadow-lg shadow-emerald-500/30">
                    Kazancı Hesapla
                </button>
                
                <div id="mesai-sonuc" class="p-5 bg-emerald-50 dark:bg-emerald-900/30 text-emerald-800 dark:text-emerald-300 rounded-2xl hidden text-center border border-emerald-200 dark:border-emerald-800/60 shadow-sm mt-4 transition-all"></div>
            </div>
        </div>
    </div>

    <script>
        // --- 2026 RESMİ VE DİNİ TATİLLER ---
        const TR_HOLIDAYS_2026 = {
            "0-1": "Yılbaşı", "2-19": "Ramazan Bayramı Arifesi", "2-20": "Ramazan Bayramı (1. Gün)",
            "2-21": "Ramazan Bayramı (2. Gün)", "2-22": "Ramazan Bayramı (3. Gün)", "3-23": "23 Nisan",
            "4-1": "1 Mayıs", "4-19": "19 Mayıs", "4-26": "Kurban Bayramı Arifesi",
            "4-27": "Kurban Bayramı (1. Gün)", "4-28": "Kurban Bayramı (2. Gün)", "4-29": "Kurban Bayramı (3. Gün)",
            "4-30": "Kurban Bayramı (4. Gün)", "6-15": "15 Temmuz", "7-30": "30 Ağustos Zafer",
            "9-29": "29 Ekim Cumhuriyet"
        };

        let selectedMultiplier = 1.5;
        let activeMonthIndex = 0;
        let activeMonthName = "";

        // TEMA YÖNETİMİ
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

        // AKILLI BUGÜN ÖZETİ WIDGET'I
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

        // SAYFA GEÇİŞLERİ
        function showSection(id) {
            document.querySelectorAll('.app-section').forEach(s => s.classList.remove('active'));
            document.getElementById('section-' + id).classList.add('active');
            window.scrollTo({ top: 0, behavior: 'smooth' });
            if(id === 'takvim-aylar') renderMonths();
        }

        // AY BUTONLARI
        function renderMonths() {
            const container = document.getElementById('month-list');
            container.innerHTML = "";
            const aylar = ["Ocak", "Şubat", "Mart", "Nisan", "Mayıs", "Haziran", "Temmuz", "Ağustos", "Eylül", "Ekim", "Kasım", "Aralık"];
            aylar.forEach((m, i) => {
                const btn = document.createElement('button');
                btn.className = "py-3.5 px-2 glass-card rounded-2xl shadow-sm text-sm font-bold text-slate-700 dark:text-slate-200 border border-slate-200/60 dark:border-slate-700/60 hover:border-blue-400 active:scale-95 transition-all w-full flex items-center justify-center";
                btn.innerText = m;
                btn.onclick = () => showCalendar(i, m);
                container.appendChild(btn);
            });
        }

        // VARDİYA DÖNGÜSÜ
        function getShift(d) {
            const refDate = new Date(2026, 4, 15);
            d.setHours(0,0,0,0);
            const diffDays = Math.floor((d.getTime() - refDate.getTime()) / (1000 * 60 * 60 * 24));
            const cycle = ['Gündüz', 'Gündüz', 'Gece', 'Gece', 'İzin', 'İzin'];
            let idx = (3 + diffDays) % 6;
            return cycle[idx < 0 ? idx + 6 : idx];
        }

        // TAKVİM ÇİZİMİ
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
                <div class="text-[10px] sm:text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Pt</div>
                <div class="text-[10px] sm:text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Sa</div>
                <div class="text-[10px] sm:text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Ça</div>
                <div class="text-[10px] sm:text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Pe</div>
                <div class="text-[10px] sm:text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Cu</div>
                <div class="text-[10px] sm:text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Ct</div>
                <div class="text-[10px] sm:text-[11px] font-black text-slate-400 dark:text-slate-500 mb-2">Pa</div>
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
                let baseClass = "relative flex flex-col items-center justify-center py-2.5 sm:py-3.5 rounded-[10px] border border-transparent shadow-sm w-full h-[52px] sm:h-16 ";
                let shiftClass = shift === 'Gündüz' ? 'bg-emerald-50 dark:bg-emerald-900/30 text-emerald-700 dark:text-emerald-400' : 
                                 shift === 'Gece' ? 'bg-indigo-50 dark:bg-indigo-900/30 text-indigo-700 dark:text-indigo-400' : 
                                 'bg-slate-50 dark:bg-slate-800 text-slate-500 dark:text-slate-400';

                let holidayIndicator = holidayName ? `<span class="absolute top-1 right-1 w-1.5 h-1.5 sm:w-2 sm:h-2 bg-rose-500 rounded-full shadow-[0_0_4px_rgba(244,63,94,0.6)]"></span>` : "";
                if(holidayName) baseClass += " !border-rose-300 dark:!border-rose-500/50 ";

                div.className = baseClass + shiftClass;
                div.innerHTML = `${holidayIndicator}<span class="text-[12px] sm:text-sm font-bold leading-none mb-1">${d}</span><span class="text-[9px] sm:text-[10px] font-black w-full text-center leading-none uppercase px-0.5">${shift}</span>`;
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
                    li.className = "flex items-start gap-2 text-xs sm:text-sm";
                    li.innerHTML = `<span class="text-rose-500 mt-0.5 font-black">•</span> <span><strong class="font-extrabold">${h.day} ${mName}:</strong> ${h.name}</span>`;
                    holidayList.appendChild(li);
                });
            } else holidayBox.classList.add('hidden');
        }

        // ICS AKTAR
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

        // İZİN HESAPLAYICI
        function hesaplaIzin() {
            const startStr = document.getElementById('start-date').value;
            const gun = parseInt(document.getElementById('izin-gun').value);
            const sonucKutusu = document.getElementById('izin-sonuc');

            if(!startStr || isNaN(gun) || gun < 1) {
                sonucKutusu.classList.remove('hidden');
                sonucKutusu.innerHTML = `<span class="text-rose-600 dark:text-rose-400 font-bold text-sm">Lütfen tarih ve gün girin.</span>`;
                sonucKutusu.className = "p-5 bg-rose-50 dark:bg-rose-900/20 rounded-2xl text-center border border-rose-200 dark:border-rose-900/50 mt-4";
                return;
            }
            
            const start = new Date(startStr);
            start.setDate(start.getDate() + gun + 2 - 1);
            
            sonucKutusu.classList.remove('hidden');
            sonucKutusu.className = "p-6 bg-gradient-to-br from-purple-50 to-indigo-50 dark:from-purple-900/40 dark:to-indigo-900/40 text-purple-900 dark:text-purple-100 rounded-2xl text-center border border-purple-200 dark:border-purple-700/50 shadow-sm mt-4";
            sonucKutusu.innerHTML = `<div class="text-[10px] sm:text-[11px] font-extrabold uppercase tracking-widest text-purple-500 dark:text-purple-400 mb-2">İşe Başlama Tarihi</div><div class="text-xl sm:text-2xl font-black truncate">${start.toLocaleDateString('tr-TR', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}</div>`;
        }

        // MESAİ ÇARPAN SEÇİMİ
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
                btn.className = "mult-btn py-3.5 sm:py-4 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-400 border border-slate-200 dark:border-slate-700 rounded-xl sm:rounded-2xl text-xs sm:text-sm font-bold transition-all";
            });
            if (!isCustom) {
                const activeBtn = document.getElementById(`btn-mult-${selectedMultiplier.toFixed(1)}`);
                if (activeBtn) activeBtn.className = "mult-btn py-3.5 sm:py-4 bg-emerald-600 border border-emerald-600 text-white rounded-xl sm:rounded-2xl text-xs sm:text-sm font-black shadow-lg shadow-emerald-500/30 transition-all";
            }
        }

        // MESAİ HESAPLAMA (YOL/YEMEK GÜN VE ADET BAZLI)
        function hesaplaMesai() {
            const saat = parseFloat(document.getElementById('mesai-saat').value);
            const ucret = parseFloat(document.getElementById('mesai-ucret').value);
            
            // Yol Hesabı
            const yolUcret = parseFloat(document.getElementById('mesai-yol-ucret').value) || 0;
            const yolGun = parseFloat(document.getElementById('mesai-yol-gun').value) || 0;
            const toplamYol = yolUcret * yolGun;
            
            // Yemek Hesabı
            const yemekUcret = parseFloat(document.getElementById('mesai-yemek-ucret').value) || 0;
            const yemekGun = parseFloat(document.getElementById('mesai-yemek-gun').value) || 0;
            const toplamYemek = yemekUcret * yemekGun;
            
            const sonucKutusu = document.getElementById('mesai-sonuc');

            if(isNaN(saat) || isNaN(ucret)) {
                sonucKutusu.classList.remove('hidden');
                sonucKutusu.innerHTML = `<span class="text-rose-600 dark:text-rose-400 font-bold text-sm">Lütfen mesai saati ve saatlik ücret girin.</span>`;
                sonucKutusu.className = "p-5 bg-rose-50 dark:bg-rose-900/20 rounded-2xl text-center border border-rose-200 dark:border-rose-900/50 mt-4";
                return;
            }

            const mesaiKazanci = saat * ucret * selectedMultiplier;
            const genelToplam = mesaiKazanci + toplamYol + toplamYemek;
            
            let ekstraMetin = "";
            let yolMetin = toplamYol > 0 ? `${yolUcret}₺×${yolGun} Yol` : '';
            let yemekMetin = toplamYemek > 0 ? `${yemekUcret}₺×${yemekGun} Yemek` : '';

            if (toplamYol > 0 || toplamYemek > 0) {
                ekstraMetin = ` <br>+ ${yolMetin} ${(toplamYol > 0 && toplamYemek > 0) ? '&' : ''} ${yemekMetin}`;
            }
            
            sonucKutusu.classList.remove('hidden');
            sonucKutusu.className = "p-6 bg-gradient-to-br from-emerald-50 to-teal-50 dark:from-emerald-900/40 dark:to-teal-900/40 text-emerald-900 dark:text-emerald-100 rounded-2xl text-center border border-emerald-200 dark:border-emerald-700/50 shadow-sm mt-4";
            sonucKutusu.innerHTML = `
                <div class="text-[10px] sm:text-[11px] font-extrabold uppercase tracking-widest text-emerald-600 dark:text-emerald-400 mb-2">Toplam Hakediş Tutarı</div>
                <div class="text-3xl sm:text-4xl font-black mb-2 truncate">${genelToplam.toLocaleString('tr-TR', { minimumFractionDigits: 2 })} ₺</div>
                <div class="text-[10px] sm:text-[11px] text-emerald-700/90 dark:text-emerald-300/90 font-bold mt-2 pt-2 border-t border-emerald-200/50 dark:border-emerald-800/50 inline-block leading-relaxed">
                    [(${saat}s × ${ucret}₺ × ${selectedMultiplier})${ekstraMetin}]
                </div>
            `;
        }
    </script>
</body>
</html>