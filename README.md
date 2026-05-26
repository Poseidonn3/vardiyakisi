<!DOCTYPE html>
<html lang="tr" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0, viewport-fit=cover">
    <title>Vardiya Yönetim Merkezi</title>
    
    <!-- PWA -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <meta name="theme-color" content="#0f172a">
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/3652/3652191.png">
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            content: ["./**/*.{html,js}"],
            theme: { 
                extend: { 
                    fontFamily: { sans: ['Plus Jakarta Sans', 'sans-serif'] },
                    colors: {
                        darkbg: '#0f172a',
                        darkcard: '#1e293b'
                    }
                } 
            }
        }
    </script>

    <style>
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            -webkit-tap-highlight-color: transparent; 
        }
        
        .app-section { display: none; animation: slideUpFade 0.3s cubic-bezier(0.16, 1, 0.3, 1); }
        .app-section.active { display: block; }
        
        @keyframes slideUpFade { 
            from { opacity: 0; transform: translateY(10px); } 
            to { opacity: 1; transform: translateY(0); } 
        }
        
        ::placeholder { color: #94a3b8; opacity: 1; }
        .dark ::placeholder { color: #64748b; }
        
        input[type="date"], input[type="number"], select { 
            -webkit-appearance: none; 
            width: 100%; 
            box-sizing: border-box;
        }
        .dark input[type="date"]::-webkit-calendar-picker-indicator { filter: invert(1); opacity: 0.7; }
        ::-webkit-scrollbar { display: none; }

        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
        }
        .dark .glass-card {
            background: rgba(30, 41, 59, 0.85);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }

        .calendar-day {
            aspect-ratio: 1 / 1;
            min-height: 68px;
        }

        @media (max-width: 640px) {
            .calendar-day { min-height: 62px; }
        }
    </style>
</head>
<body class="bg-slate-50 dark:bg-darkbg text-slate-800 dark:text-slate-100 min-h-screen transition-colors duration-500">

    <div class="max-w-[540px] mx-auto w-full min-h-screen flex flex-col px-4 sm:px-6 py-6">

        <!-- HEADER -->
        <header class="mb-6 flex items-center justify-between">
            <div>
                <h1 class="text-3xl font-extrabold tracking-tight bg-gradient-to-r from-blue-600 to-indigo-600 dark:from-blue-400 dark:to-indigo-400 bg-clip-text text-transparent">
                    Vardiya Akışı
                </h1>
                <p class="text-[11px] font-bold text-slate-500 dark:text-slate-400 uppercase tracking-widest">Premium Asistan</p>
            </div>
            <div class="flex gap-2">
                <button onclick="showSection('ayarlar')" class="p-3 rounded-2xl bg-white/80 dark:bg-slate-800/80 shadow-sm border border-slate-200/50 dark:border-slate-700/50 hover:scale-105 active:scale-95 transition-all">
                    <span class="text-xl">⚙️</span>
                </button>
                <button onclick="toggleTheme()" class="p-3 rounded-2xl bg-white/80 dark:bg-slate-800/80 shadow-sm border border-slate-200/50 dark:border-slate-700/50 hover:scale-105 active:scale-95 transition-all">
                    <span id="theme-icon" class="text-xl">🌙</span>
                </button>
            </div>
        </header>

        <!-- ANA İÇERİK -->
        <div class="flex-1 space-y-5">

            <!-- MENÜ -->
            <div id="section-menu" class="app-section active space-y-5">
                <!-- Bugünün Özeti -->
                <div class="p-5 sm:p-6 rounded-[28px] shadow-lg shadow-blue-500/20 dark:shadow-none bg-gradient-to-br from-blue-600 to-indigo-600 text-white relative overflow-hidden">
                    <div class="absolute -right-6 -top-6 w-36 h-36 bg-white/10 rounded-full blur-3xl"></div>
                    <div class="relative z-10">
                        <div class="flex justify-between items-start">
                            <div class="flex-1">
                                <p class="text-xs font-bold uppercase tracking-wider text-blue-100 mb-1" id="today-date-text">Bugün</p>
                                <h2 class="text-2xl font-black" id="today-shift-text">Hesaplanıyor...</h2>
                                <div id="today-hours" class="mt-3 hidden inline-block px-4 py-1.5 bg-black/30 rounded-xl text-sm font-bold"></div>
                            </div>
                            <div id="today-icon" class="text-5xl transition-all">⏳</div>
                        </div>
                    </div>
                </div>

                <!-- Menü Butonları -->
                <div class="grid grid-cols-2 gap-4">
                    <button onclick="showSection('takvim-aylar')" class="glass-card rounded-3xl py-6 px-4 hover:border-blue-400 transition-all flex flex-col items-center">
                        <span class="text-4xl mb-3">📅</span>
                        <span class="font-bold">Aylık Takvim</span>
                    </button>
                    <button onclick="showSection('izin-hesap')" class="glass-card rounded-3xl py-6 px-4 hover:border-purple-400 transition-all flex flex-col items-center">
                        <span class="text-4xl mb-3">✈️</span>
                        <span class="font-bold">İzin Planlayıcı</span>
                    </button>
                    <button onclick="showSection('mesai-hesap')" class="glass-card rounded-3xl py-6 px-4 hover:border-emerald-400 transition-all flex flex-col items-center">
                        <span class="text-4xl mb-3">💰</span>
                        <span class="font-bold">Mesai & Kazanç</span>
                    </button>
                    <button onclick="generateYearlyReport()" class="glass-card rounded-3xl py-6 px-4 hover:border-orange-400 transition-all flex flex-col items-center">
                        <span class="text-4xl mb-3">📊</span>
                        <span class="font-bold">Yıllık Rapor</span>
                    </button>
                </div>
            </div>

            <!-- Diğer bölümler (ayarlar, takvim vs.) aynı kalıyor ama container genişletildi -->
            <!-- ... (geri kalan tüm HTML içeriğini olduğu gibi bırakıyorum, sadece container değişti) ... -->

            <!-- AYARLAR -->
            <div id="section-ayarlar" class="app-section space-y-4">
                <button onclick="showSection('menu')" class="flex items-center gap-1.5 text-blue-600 dark:text-blue-400 font-bold text-sm py-2">
                    ← Ana Menü
                </button>
                <div class="glass-card p-6 rounded-3xl space-y-6">
                    <h2 class="text-2xl font-black">Sistem Ayarları</h2>
                    <!-- kalan ayar içeriği aynı -->
                    <div class="w-full">
                        <label class="block text-xs font-bold text-slate-500 dark:text-slate-400 mb-2">Çalışma Döngüsü</label>
                        <select id="ayarlar-dongu" class="w-full px-5 py-4 rounded-2xl border border-slate-200 dark:border-slate-700 bg-white dark:bg-slate-900">
                            <option value="default">Klasik (2G 2G 2İ)</option>
                            <option value="4g2i">4 Gündüz - 2 İzin</option>
                            <option value="sabit_g">Sabit Gündüz</option>
                            <option value="sabit_ge">Sabit Gece</option>
                        </select>
                    </div>
                    <div class="w-full">
                        <label class="block text-xs font-bold text-slate-500 dark:text-slate-400 mb-2">Referans Başlangıç Tarihi</label>
                        <input type="date" id="ayarlar-tarih" class="w-full px-5 py-4 rounded-2xl border border-slate-200 dark:border-slate-700 bg-white dark:bg-slate-900">
                    </div>
                    <button onclick="ayarlariKaydet()" class="w-full py-4 bg-gradient-to-r from-blue-600 to-indigo-600 text-white rounded-2xl font-bold text-lg">Ayarları Kaydet</button>
                </div>
            </div>

            <!-- Diğer bölümleri (takvim, izin, mesai, rapor) aynı şekilde bırakabilirsiniz. Sadece container değiştiği için düzgün görünecek. -->

        </div>
    </div>

    <!-- Modal ve Script kısmı aynı kalıyor -->
    <!-- ... (kodun geri kalanı) ... -->

    <script>
        // Mevcut script'in tamamı burada kalıyor...
        // Sadece showSection ve render fonksiyonlarında ufak iyileştirmeler yapılabilir.

        function showSection(id) {
            document.querySelectorAll('.app-section').forEach(s => s.classList.remove('active'));
            const section = document.getElementById('section-' + id);
            if (section) section.classList.add('active');
            window.scrollTo({ top: 0, behavior: 'smooth' });
            if(id === 'takvim-aylar') renderMonths();
        }

        // ... kalan script fonksiyonları aynı ...
    </script>
</body>
</html>