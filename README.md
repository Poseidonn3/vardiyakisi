```html
<!DOCTYPE html>
<html lang="tr" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0, viewport-fit=cover">
    <title>Vardiya Yönetim Merkezi</title>
    
    <!-- PWA & Mobil Uyumluluk -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <meta name="theme-color" content="#0f172a">
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/3652/3652191.png">
    
    <!-- Tailwind CSS ve Google Fonts -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
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
            min-width: 0; 
            box-sizing: border-box;
        }
        .dark input[type="date"]::-webkit-calendar-picker-indicator { filter: invert(1); opacity: 0.7; }
        ::-webkit-scrollbar { display: none; }
        
        /* Cam Efekti - Native Görünüm İçin Optimize Edildi */
        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
        }
        .dark .glass-card {
            background: rgba(30, 41, 59, 0.75);
            border: 1px solid rgba(255, 255, 255, 0.05);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.3);
        }
        
        .progress-bar-fill {
            transition: width 1s cubic-bezier(0.4, 0, 0.2, 1);
        }

        /* Modal Animasyonları */
        .modal-overlay {
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
            animation: fadeIn 0.2s ease-out;
        }
        .modal-content {
            animation: popIn 0.3s cubic-bezier(0.16, 1, 0.3, 1);
        }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        @keyframes popIn { from { opacity: 0; transform: scale(0.95) translateY(10px); } to { opacity: 1; transform: scale(1) translateY(0); } }
    </style>
</head>
<body class="bg-slate-50 dark:bg-darkbg text-slate-800 dark:text-slate-100 min-h-screen pb-12 transition-colors duration-500 bg-[radial-gradient(ellipse_at_top,_var(--tw-gradient-stops))] from-blue-50/80 via-slate-50 to-slate-100 dark:from-slate-900 dark:via-darkbg dark:to-darkbg">

    <!-- GENİŞLETİLMİŞ KONTEYNER: Ekranı Daha İyi Doldurur -->
    <div class="max-w-[500px] mx-auto w-full px-8 sm:px-8 pt-8 sm:pt-8">
        
        <!-- ÜST BAŞLIK VE TEMA/AYARLAR -->
        <header class="mb-6 flex items-center justify-between">
            <div>
                <h1 class="text-3xl font-extrabold tracking-tight bg-gradient-to-r from-blue-600 to-indigo-600 dark:from-blue-400 dark:to-indigo-400 bg-clip-text text-transparent pb-1">Vardiya Akışı</h1>
                <p class="text-[11px] font-bold text-slate-500 dark:text-slate-400 uppercase tracking-widest">Premium Asistan</p>
            </div>
            <div class="flex gap-2">
                <button onclick="showSection('ayarlar')" class="p-3 rounded-2xl bg-white/80 dark:bg-slate-800/80 shadow-sm border border-slate-200/50 dark:border-slate-700/50 hover:scale-105 active:scale-95 transition-all duration-300 backdrop-blur-md">
                    <span class="text-xl leading-none flex text-slate-600 dark:text-slate-300">⚙️</span>
                </button>
                <button onclick="toggleTheme()" class="p-3 rounded-2xl bg-white/80 dark:bg-slate-800/80 shadow-sm border border-slate-200/50 dark:border-slate-700/50 hover:scale-105 active:scale-95 transition-all duration-300 backdrop-blur-md">
                    <span id="theme-icon" class="text-xl leading-none flex">🌙</span>
                </button>
            </div>
        </header>

        <!-- ANA MENÜ -->
        <div id="section-menu" class="app-section active space-y-5">
            
            <!-- Bugünün Özeti Akıllı Kartı -->
            <div class="p-5 sm:p-6 rounded-[28px] shadow-lg shadow-blue-500/20 dark:shadow-none bg-gradient-to-br from-blue-600 to-indigo-600 text-white relative overflow-hidden">
                <div class="absolute -right-4 -top-4 w-32 h-32 bg-white/10 rounded-full blur-2xl"></div>
                <div class="absolute -left-4 -bottom-4 w-24 h-24 bg-black/10 rounded-full blur-xl"></div>
                <div class="relative z-10">
                    <div class="flex justify-between items-center">
                        <div class="flex-1 min-w-0">
                            <p class="text-[10px] sm:text-xs font-bold uppercase tracking-wider text-blue-100 mb-1" id="today-date-text">Bugün</p>
                            <h2 class="text-2xl font-black truncate" id="today-shift-text">Hesaplanıyor...</h2>
                            <div id="today-hours" class="mt-2 hidden inline-block px-3 py-1 bg-black/25 rounded-lg text-[11px] font-bold tracking-widest backdrop-blur-sm border border-white/10"></div>
                        </div>
                        <div class="w-14 h-14 rounded-2xl bg-white/20 flex-shrink-0 flex items-center justify-center text-3xl backdrop-blur-md border border-white/20 ml-3 transition-all shadow-inner" id="today-icon">
                            ⏳
                        </div>
                    </div>
                    
                    <!-- Gelecek Radarı -->
                    <div id="future-radar" class="mt-5 pt-3.5 border-t border-white/20 flex justify-between items-center text-[11px] font-bold text-blue-50 hidden">
                        <span id="tomorrow-shift-text" class="truncate pr-2">🔜 Yarın: -</span>
                        <span id="next-leave-text" class="shrink-0 bg-white/15 px-2.5 py-1.5 rounded-lg shadow-sm">🏖️ İzin: -</span>
                    </div>

                    <!-- Uyku / Dinlenme Tavsiyesi -->
                    <div id="sleep-advisor" class="mt-3 py-2.5 px-3.5 bg-white/10 rounded-xl text-[11px] font-medium text-blue-50 flex items-center gap-2 border border-white/10 hidden backdrop-blur-md shadow-sm">
                        <span id="sleep-advisor-icon" class="text-sm">🛌</span>
                        <span id="sleep-advisor-text">Biyolojik saat önerisi yükleniyor...</span>
                    </div>
                </div>
            </div>

            <!-- Menü Butonları Izgarası (Daha dengeli ve yaygın) -->
            <div class="grid grid-cols-2 gap-3.5">
                <button onclick="showSection('takvim-aylar')" class="w-full py-5 px-4 glass-card rounded-[24px] border border-slate-200/60 dark:border-slate-700/40 hover:border-blue-400 active:scale-[0.97] transition-all text-center flex flex-col items-center justify-center">
                    <span class="text-3xl mb-2 drop-shadow-sm">📅</span>
                    <h2 class="font-bold text-[13px] text-slate-800 dark:text-slate-100">Aylık Takvim</h2>
                </button>
                <button onclick="showSection('izin-hesap')" class="w-full py-5 px-4 glass-card rounded-[24px] border border-slate-200/60 dark:border-slate-700/40 hover:border-purple-400 active:scale-[0.97] transition-all text-center flex flex-col items-center justify-center">
                    <span class="text-3xl mb-2 drop-shadow-sm">✈️</span>
                    <h2 class="font-bold text-[13px] text-slate-800 dark:text-slate-100">İzin Planlayıcı</h2>
                </button>
                <button onclick="showSection('mesai-hesap')" class="w-full py-5 px-4 glass-card rounded-[24px] border border-slate-200/60 dark:border-slate-700/40 hover:border-emerald-400 active:scale-[0.97] transition-all text-center flex flex-col items-center justify-center">
                    <span class="text-3xl mb-2 drop-shadow-sm">💰</span>
                    <h2 class="font-bold text-[13px] text-slate-800 dark:text-slate-100">Kazanç & Mesai</h2>
                </button>
                <button onclick="generateYearlyReport()" class="w-full py-5 px-4 glass-card rounded-[24px] border border-slate-200/60 dark:border-slate-700/40 hover:border-orange-400 active:scale-[0.97] transition-all text-center flex flex-col items-center justify-center relative overflow-hidden group">
                    <div class="absolute inset-0 bg-gradient-to-br from-orange-500/10 to-rose-500/10 opacity-0 group-hover:opacity-100 transition-opacity"></div>
                    <span class="text-3xl mb-2 drop-shadow-sm">📊</span>
                    <h2 class="font-bold text-[13px] text-slate-800 dark:text-slate-100">Yıllık Rapor</h2>
                </button>
            </div>
        </div>

        <!-- NATIVE GERİ BUTONU TASARIMI (Tüm alt sayfalar için geçerli) -->
        
        <!-- AYARLAR BÖLÜMÜ -->
        <div id="section-ayarlar" class="app-section space-y-4">
            <button onclick="showSection('menu')" class="flex items-center gap-1.5 text-blue-600 dark:text-blue-400 font-bold text-sm py-2 px-1 hover:opacity-80 transition-opacity w-max active:scale-95 mb-1">
                <span class="text-lg leading-none">‹</span> Ana Menü
            </button>
            
            <div class="glass-card p-5 sm:p-6 rounded-[28px] border border-slate-200/60 dark:border-slate-700/40 space-y-5">
                <div>
                    <h2 class="text-xl font-black text-slate-800 dark:text-slate-100 flex items-center gap-2 mb-1">⚙️ Sistem Ayarları</h2>
                    <p class="text-[11px] text-slate-500 dark:text-slate-400 font-medium">Çalışma döngünüzü özelleştirin.</p>
                </div>
                
                <div class="w-full mt-4">
                    <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">Çalışma Döngüsü</label>
                    <div class="relative">
                        <select id="ayarlar-dongu" class="w-full px-4 py-3.5 bg-slate-50/50 dark:bg-slate-900/50 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-blue-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-inner appearance-none cursor-pointer">
                            <option value="default">Klasik (2 Gündüz, 2 Gece, 2 İzin)</option>
                            <option value="4g2i">4 Gündüz, 2 İzin</option>
                            <option value="sabit_g">Sabit Gündüz (Pazar İzin)</option>
                            <option value="sabit_ge">Sabit Gece (Pazar İzin)</option>
                        </select>
                        <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-4 text-slate-500">▼</div>
                    </div>
                </div>

                <div class="w-full">
                    <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">Referans Başlangıç Tarihi</label>
                    <input type="date" id="ayarlar-tarih" class="w-full px-4 py-3.5 bg-slate-50/50 dark:bg-slate-900/50 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-blue-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-inner block">
                    <p class="text-[10px] text-slate-500 mt-2 font-medium ml-1">* Döngünün ilk iş gününe denk gelen geçmiş bir tarih seçin.</p>
                </div>
                
                <button onclick="ayarlariKaydet()" class="w-full py-4 mt-2 bg-gradient-to-r from-blue-600 to-indigo-600 hover:opacity-90 active:scale-[0.98] text-white rounded-2xl font-black transition-all shadow-md">
                    Ayarları Kaydet
                </button>
            </div>
        </div>

        <!-- YILLIK RAPOR BÖLÜMÜ -->
        <div id="section-yillik-rapor" class="app-section space-y-4">
            <button onclick="showSection('menu')" class="flex items-center gap-1.5 text-blue-600 dark:text-blue-400 font-bold text-sm py-2 px-1 hover:opacity-80 transition-opacity w-max active:scale-95 mb-1">
                <span class="text-lg leading-none">‹</span> Ana Menü
            </button>
            
            <div class="glass-card p-6 rounded-[28px] border border-slate-200/60 dark:border-slate-700/40 space-y-4">
                <div class="text-center mb-6">
                    <span class="text-5xl block mb-3 drop-shadow-md">📊</span>
                    <h2 class="text-2xl font-black text-slate-800 dark:text-slate-100">2026 Yıllık Analiz</h2>
                    <p class="text-xs text-slate-500 dark:text-slate-400 font-medium mt-1">Tüm yılın çalışma döngüsü</p>
                </div>
                
                <div class="grid grid-cols-2 gap-3.5">
                    <div class="bg-emerald-50/80 dark:bg-emerald-900/20 p-5 rounded-2xl border border-emerald-200/60 dark:border-emerald-800/50 text-center shadow-sm">
                        <span class="block text-[10px] font-bold text-emerald-600 dark:text-emerald-400 uppercase tracking-widest mb-1">Toplam Gündüz</span>
                        <span id="yil-gunduz" class="text-3xl font-black text-emerald-700 dark:text-emerald-300">0</span>
                    </div>
                    <div class="bg-indigo-50/80 dark:bg-indigo-900/20 p-5 rounded-2xl border border-indigo-200/60 dark:border-indigo-800/50 text-center shadow-sm">
                        <span class="block text-[10px] font-bold text-indigo-600 dark:text-indigo-400 uppercase tracking-widest mb-1">Toplam Gece</span>
                        <span id="yil-gece" class="text-3xl font-black text-indigo-700 dark:text-indigo-300">0</span>
                    </div>
                </div>
                
                <div class="bg-slate-50/80 dark:bg-slate-800/40 p-5 rounded-2xl border border-slate-200/60 dark:border-slate-700/50 text-center shadow-sm mt-1">
                    <span class="block text-[10px] font-bold text-slate-500 dark:text-slate-400 uppercase tracking-widest mb-1">Toplam İzin</span>
                    <span id="yil-izin" class="text-3xl font-black text-slate-700 dark:text-slate-200">0</span>
                </div>
            </div>
        </div>


        <!-- TAKVİM: AY LİSTESİ -->
        <div id="section-takvim-aylar" class="app-section space-y-4">
            <button onclick="showSection('menu')" class="flex items-center gap-1.5 text-blue-600 dark:text-blue-400 font-bold text-sm py-2 px-1 hover:opacity-80 transition-opacity w-max active:scale-95 mb-1">
                <span class="text-lg leading-none">‹</span> Ana Menü
            </button>
            <div id="month-list" class="grid grid-cols-3 gap-3"></div>
        </div>

        <!-- TAKVİM: DETAY (TAŞMAYAN ÖZEL TASARIM) -->
        <div id="section-takvim-detay" class="app-section space-y-4">
            <div class="flex items-center justify-between mb-2 px-1">
                <button onclick="showSection('takvim-aylar')" class="flex items-center gap-1.5 text-blue-600 dark:text-blue-400 font-bold text-sm py-2 hover:opacity-80 transition-opacity active:scale-95">
                    <span class="text-lg leading-none">‹</span> Aylar
                </button>
                <h2 id="month-name" class="text-lg font-black text-slate-800 dark:text-slate-100"></h2>
                <div class="flex items-center gap-2">
                    <button onclick="copyToClipboard()" id="copy-btn" class="w-8 h-8 rounded-full bg-slate-200 dark:bg-slate-800 text-slate-600 dark:text-slate-300 flex items-center justify-center hover:scale-105 active:scale-95 transition-all text-sm" title="Kopyala">
                        📋
                    </button>
                    <button onclick="exportIcs()" class="w-8 h-8 rounded-full bg-slate-200 dark:bg-slate-800 text-slate-600 dark:text-slate-300 flex items-center justify-center hover:scale-105 active:scale-95 transition-all text-sm" title="Takvime Aktar">
                        📤
                    </button>
                </div>
            </div>
            
            <!-- İstatistik Kartları -->
            <div class="grid grid-cols-3 gap-2">
                <div class="glass-card border border-emerald-200/50 dark:border-emerald-800/30 py-2.5 rounded-2xl text-center shadow-sm">
                    <span class="block text-[9px] font-bold text-emerald-600 dark:text-emerald-400 uppercase tracking-widest mb-0.5">Gündüz</span>
                    <span id="stat-gunduz" class="text-lg font-black text-emerald-700 dark:text-emerald-300">0</span>
                </div>
                <div class="glass-card border border-indigo-200/50 dark:border-indigo-800/30 py-2.5 rounded-2xl text-center shadow-sm">
                    <span class="block text-[9px] font-bold text-indigo-600 dark:text-indigo-400 uppercase tracking-widest mb-0.5">Gece</span>
                    <span id="stat-gece" class="text-lg font-black text-indigo-700 dark:text-indigo-300">0</span>
                </div>
                <div class="glass-card border border-slate-200/50 dark:border-slate-700/30 py-2.5 rounded-2xl text-center shadow-sm">
                    <span class="block text-[9px] font-bold text-slate-500 dark:text-slate-400 uppercase tracking-widest mb-0.5">İzin</span>
                    <span id="stat-izin" class="text-lg font-black text-slate-600 dark:text-slate-300">0</span>
                </div>
            </div>

            <!-- Takvim Izgarası (Ekrana tam yayılan, taşmayan kutular) -->
            <div class="glass-card p-2 sm:p-3 rounded-[24px] shadow-sm border border-slate-200/60 dark:border-slate-700/40 relative">
                <div class="text-[9px] text-slate-400 font-bold text-center w-full mb-3 uppercase tracking-widest flex items-center justify-center gap-1">
                    <span>💡 Günlere dokunarak not ekleyebilirsiniz</span>
                </div>
                <div id="calendar-grid" class="grid grid-cols-7 gap-1.5 sm:gap-2 text-center w-full"></div>
            </div>

            <!-- Özel Günler Kutusu -->
            <div id="holiday-box" class="hidden bg-gradient-to-br from-rose-50/80 to-orange-50/80 dark:from-rose-950/30 dark:to-orange-950/30 border border-rose-200/60 dark:border-rose-800/40 p-5 rounded-[24px] shadow-sm space-y-3">
                <h3 class="text-[11px] font-extrabold text-rose-600 dark:text-rose-400 uppercase tracking-widest flex items-center gap-2">
                    <span>🎉</span> Özel Günler
                </h3>
                <ul id="holiday-list" class="space-y-2 text-xs sm:text-sm font-medium text-rose-800 dark:text-rose-300"></ul>
            </div>
        </div>

        <!-- İZİN HESAPLAYICI -->
        <div id="section-izin-hesap" class="app-section space-y-4">
            <button onclick="showSection('menu')" class="flex items-center gap-1.5 text-blue-600 dark:text-blue-400 font-bold text-sm py-2 px-1 hover:opacity-80 transition-opacity w-max active:scale-95 mb-1">
                <span class="text-lg leading-none">‹</span> Ana Menü
            </button>
            
            <div class="glass-card p-5 sm:p-6 rounded-[28px] border border-slate-200/60 dark:border-slate-700/40 space-y-5 shadow-sm">
                
                <div class="bg-slate-50/50 dark:bg-slate-800/30 p-4 rounded-2xl border border-slate-200/60 dark:border-slate-700/50">
                    <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1 flex items-center gap-1">🗂️ Toplam Yıllık İzin Hakkın <span class="text-[9px] lowercase opacity-70 font-medium">(Opsiyonel)</span></label>
                    <input type="number" id="izin-toplam-hak" placeholder="Örn: 14" class="w-full px-4 py-3 bg-white/60 dark:bg-slate-900/60 rounded-xl border border-slate-200/80 dark:border-slate-700/80 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-purple-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-sm block">
                </div>

                <div class="w-full">
                    <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">İzin Başlangıç Tarihi</label>
                    <input type="date" id="start-date" class="w-full px-4 py-3.5 bg-slate-50/50 dark:bg-slate-900/50 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-purple-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-inner block">
                </div>
                
                <div class="w-full">
                    <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">Kullanılacak İzin (Gün)</label>
                    <input type="number" id="izin-gun" placeholder="Örn: 7" class="w-full px-4 py-3.5 bg-slate-50/50 dark:bg-slate-900/50 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-purple-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-inner block">
                </div>
                
                <button onclick="hesaplaIzin()" class="w-full py-4 mt-2 bg-gradient-to-r from-purple-600 to-indigo-600 hover:opacity-90 active:scale-[0.98] text-white rounded-2xl font-black transition-all shadow-md shadow-purple-500/20">
                    Hesapla ve Bakiyeyi Gör
                </button>
                
                <div id="izin-sonuc" class="p-5 bg-purple-50 dark:bg-purple-900/30 text-purple-800 dark:text-purple-300 rounded-2xl hidden text-center border border-purple-200 dark:border-purple-800/50 shadow-sm transition-all mt-4"></div>
            </div>
        </div>

        <!-- MESAİ VE KAZANÇ HESAPLAYICI -->
        <div id="section-mesai-hesap" class="app-section space-y-4">
            <button onclick="showSection('menu')" class="flex items-center gap-1.5 text-blue-600 dark:text-blue-400 font-bold text-sm py-2 px-1 hover:opacity-80 transition-opacity w-max active:scale-95 mb-1">
                <span class="text-lg leading-none">‹</span> Ana Menü
            </button>
            
            <div class="glass-card p-5 sm:p-6 rounded-[28px] border border-slate-200/60 dark:border-slate-700/40 space-y-5 shadow-sm">
                
                <div class="w-full">
                    <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">Maaş / Asgari Ücret (TL) <span class="text-[9px] lowercase opacity-70 font-medium">(Opsiyonel)</span></label>
                    <input type="number" id="mesai-maas" placeholder="Örn: 17002" class="w-full px-4 py-3.5 bg-slate-50/50 dark:bg-slate-900/50 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-inner block">
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div class="w-full">
                        <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">Mesai Saati</label>
                        <input type="number" id="mesai-saat" placeholder="Örn: 12.5" step="0.5" class="w-full px-4 py-3.5 bg-slate-50/50 dark:bg-slate-900/50 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-inner block">
                    </div>
                    
                    <div class="w-full">
                        <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">Saatlik Ücret</label>
                        <input type="number" id="mesai-ucret" placeholder="Örn: 150₺" class="w-full px-4 py-3.5 bg-slate-50/50 dark:bg-slate-900/50 rounded-2xl border border-slate-200 dark:border-slate-700 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-sm font-bold text-slate-700 dark:text-slate-200 shadow-inner block">
                    </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 border-t border-slate-200/60 dark:border-slate-700/60 pt-5 mt-2">
                    <div class="bg-slate-50/50 dark:bg-slate-800/30 p-3.5 rounded-2xl border border-slate-200/60 dark:border-slate-700/50">
                        <label class="block text-[10px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1 flex items-center gap-1">🚗 Yol Desteği <span class="text-[9px] lowercase opacity-70 font-medium">(Ops.)</span></label>
                        <div class="grid grid-cols-2 gap-2">
                            <input type="number" id="mesai-yol-ucret" placeholder="₺/Gün" class="w-full px-3 py-3 bg-white/60 dark:bg-slate-900/60 rounded-xl border border-slate-200/80 dark:border-slate-700/80 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-xs font-bold text-slate-700 dark:text-slate-200 shadow-sm block">
                            <input type="number" id="mesai-yol-gun" placeholder="Adet" class="w-full px-3 py-3 bg-white/60 dark:bg-slate-900/60 rounded-xl border border-slate-200/80 dark:border-slate-700/80 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-xs font-bold text-slate-700 dark:text-slate-200 shadow-sm block">
                        </div>
                    </div>
                    
                    <div class="bg-slate-50/50 dark:bg-slate-800/30 p-3.5 rounded-2xl border border-slate-200/60 dark:border-slate-700/50">
                        <label class="block text-[10px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1 flex items-center gap-1">🍔 Yemek Desteği <span class="text-[9px] lowercase opacity-70 font-medium">(Ops.)</span></label>
                        <div class="grid grid-cols-2 gap-2">
                            <input type="number" id="mesai-yemek-ucret" placeholder="₺/Gün" class="w-full px-3 py-3 bg-white/60 dark:bg-slate-900/60 rounded-xl border border-slate-200/80 dark:border-slate-700/80 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-xs font-bold text-slate-700 dark:text-slate-200 shadow-sm block">
                            <input type="number" id="mesai-yemek-gun" placeholder="Adet" class="w-full px-3 py-3 bg-white/60 dark:bg-slate-900/60 rounded-xl border border-slate-200/80 dark:border-slate-700/80 focus:bg-white dark:focus:bg-slate-800 focus:ring-2 focus:ring-emerald-500 outline-none transition-all text-xs font-bold text-slate-700 dark:text-slate-200 shadow-sm block">
                        </div>
                    </div>
                </div>
                
                <div class="border-t border-slate-200/60 dark:border-slate-700/60 pt-5 mt-2">
                    <label class="block text-[11px] font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-wider mb-2 ml-1">Mesai Çarpanı</label>
                    <div class="grid grid-cols-4 gap-2">
                        <button onclick="setMultiplier(1.5)" id="btn-mult-1.5" class="mult-btn py-3.5 bg-emerald-600 text-white rounded-xl text-xs font-black shadow-md transition-all">1.5x</button>
                        <button onclick="setMultiplier(2.0)" id="btn-mult-2.0" class="mult-btn py-3.5 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 rounded-xl text-xs font-bold border border-slate-200 dark:border-slate-700 transition-all">2.0x</button>
                        <button onclick="setMultiplier(1.0)" id="btn-mult-1.0" class="mult-btn py-3.5 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 rounded-xl text-xs font-bold border border-slate-200 dark:border-slate-700 transition-all">1.0x</button>
                        <input type="number" id="mesai-carpan" oninput="customMultiplierInput()" step="0.1" class="w-full text-center px-1 py-3 bg-slate-50/50 dark:bg-slate-900/50 rounded-xl border border-slate-200 dark:border-slate-700 text-xs font-bold outline-none focus:bg-white dark:focus:bg-slate-800 focus:border-emerald-500 focus:ring-2 focus:ring-emerald-500 text-slate-700 dark:text-slate-200 block" placeholder="Özel">
                    </div>
                </div>
                
                <button onclick="hesaplaMesai()" class="w-full py-4 mt-2 bg-gradient-to-r from-emerald-500 to-teal-600 hover:opacity-90 active:scale-[0.98] text-white rounded-2xl font-black text-lg transition-transform shadow-md shadow-emerald-500/20">
                    Hesapla
                </button>
                
                <div id="mesai-sonuc" class="p-5 bg-emerald-50 dark:bg-emerald-900/30 text-emerald-800 dark:text-emerald-300 rounded-2xl hidden text-center border border-emerald-200 dark:border-emerald-800/60 shadow-sm mt-4 transition-all"></div>
            </div>
        </div>
    </div>

    <!-- AJANDA (NOT) MODALI -->
    <div id="note-modal" class="fixed inset-0 z-50 flex items-center justify-center p-4 modal-overlay hidden">
        <div class="bg-white dark:bg-slate-900 p-6 rounded-3xl shadow-2xl border border-slate-200 dark:border-slate-800 w-full max-w-sm modal-content relative">
            <button onclick="closeNoteModal()" class="absolute top-4 right-4 w-8 h-8 flex items-center justify-center rounded-full bg-slate-100 dark:bg-slate-800 text-slate-500 font-bold hover:bg-rose-100 hover:text-rose-600 transition-colors">✕</button>
            <h3 class="text-lg font-black text-slate-800 dark:text-slate-100 mb-1 flex items-center gap-2"><span class="text-2xl">📝</span> Günlük Ajanda</h3>
            <p id="note-modal-date" class="text-xs font-bold text-blue-600 dark:text-blue-400 mb-4 uppercase tracking-wider"></p>
            
            <textarea id="note-input" rows="4" placeholder="Fazla mesai, toplantı, vb. notlar..." class="w-full px-4 py-3 bg-slate-50 dark:bg-slate-950 rounded-2xl border border-slate-200 dark:border-slate-800 focus:bg-white dark:focus:bg-slate-900 focus:ring-2 focus:ring-blue-500 outline-none transition-all text-sm font-medium text-slate-700 dark:text-slate-200 resize-none mb-4"></textarea>
            
            <div class="flex gap-2">
                <button onclick="saveNote()" class="flex-1 py-3.5 bg-blue-600 hover:bg-blue-700 text-white rounded-xl font-black transition-colors shadow-md shadow-blue-500/20">Kaydet</button>
                <button onclick="deleteNote()" class="py-3.5 px-5 bg-rose-50 dark:bg-rose-900/20 hover:bg-rose-100 text-rose-600 dark:text-rose-400 border border-rose-200 dark:border-rose-900/50 rounded-xl font-bold transition-colors">Sil</button>
            </div>
        </div>
    </div>

    <script>
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
        
        let appNotes = JSON.parse(localStorage.getItem('v_notes')) || {};
        let shiftSetting = localStorage.getItem('v_shift_setting') || 'default';
        let shiftRefDate = localStorage.getItem('v_shift_ref') || '2026-05-15';
        let activeNoteDateKey = "";

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
            
            document.getElementById('ayarlar-dongu').value = shiftSetting;
            document.getElementById('ayarlar-tarih').value = shiftRefDate;
            initTodayWidget();
            document.getElementById('start-date').valueAsDate = new Date();

            document.getElementById('mesai-maas').value = localStorage.getItem('v_maas') || '';
            document.getElementById('mesai-ucret').value = localStorage.getItem('v_ucret') || '';
            document.getElementById('mesai-yol-ucret').value = localStorage.getItem('v_yol_ucret') || '';
            document.getElementById('mesai-yemek-ucret').value = localStorage.getItem('v_yemek_ucret') || '';
            document.getElementById('izin-toplam-hak').value = localStorage.getItem('v_izin_hakkim') || '';
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
                
                const hoursDiv = document.getElementById('today-hours');
                const advisorDiv = document.getElementById('sleep-advisor');
                const advisorText = document.getElementById('sleep-advisor-text');
                
                if(shift === 'Gündüz') {
                    hoursDiv.innerText = "08:00 - 20:00"; hoursDiv.classList.remove('hidden');
                    advisorText.innerText = "🛌 Gece dinlenmesi: 23:00 - 06:30 önerilir."; advisorDiv.classList.remove('hidden');
                } else if(shift === 'Gece') {
                    hoursDiv.innerText = "20:00 - 08:00"; hoursDiv.classList.remove('hidden');
                    advisorText.innerText = "🛌 Gündüz uykusu: 08:30 - 16:30 arası önerilir."; advisorDiv.classList.remove('hidden');
                } else {
                    hoursDiv.classList.add('hidden');
                    advisorText.innerText = "🎉 Bugün kendinize zaman ayırın, iyice dinlenin!"; advisorDiv.classList.remove('hidden');
                }

                document.getElementById('future-radar').classList.remove('hidden');
                document.getElementById('tomorrow-shift-text').innerText = `🔜 Yarın: ${getShift(new Date(2026, m, d + 1))}`;

                let daysToLeave = 0; let checkDate = new Date(2026, m, d);
                for(let i=1; i<=7; i++) { checkDate.setDate(checkDate.getDate() + 1); if(getShift(checkDate) === 'İzin') { daysToLeave = i; break; } }
                const leaveText = document.getElementById('next-leave-text');
                if (shift === 'İzin') leaveText.innerText = "🏖️ Zaten İzindesin";
                else if (daysToLeave === 1) leaveText.innerText = "🏖️ İzin: Yarın!";
                else if (daysToLeave > 0) leaveText.innerText = `🏖️ İzin: ${daysToLeave} gün sonra`;
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
                btn.className = "py-4 px-2 glass-card rounded-2xl shadow-sm text-sm font-bold text-slate-800 dark:text-slate-100 border border-slate-200/60 dark:border-slate-700/40 hover:border-blue-400 active:scale-95 transition-all w-full flex items-center justify-center";
                btn.innerText = m;
                btn.onclick = () => showCalendar(i, m);
                container.appendChild(btn);
            });
        }

        function getShift(d) {
            const refDate = new Date(shiftRefDate);
            d.setHours(0,0,0,0);
            const diffDays = Math.floor((d.getTime() - refDate.getTime()) / (1000 * 60 * 60 * 24));
            let cycle = [];
            if(shiftSetting === 'default') cycle = ['Gündüz', 'Gündüz', 'Gece', 'Gece', 'İzin', 'İzin'];
            else if(shiftSetting === '4g2i') cycle = ['Gündüz', 'Gündüz', 'Gündüz', 'Gündüz', 'İzin', 'İzin'];
            else if(shiftSetting === 'sabit_g') return d.getDay() === 0 ? 'İzin' : 'Gündüz';
            else if(shiftSetting === 'sabit_ge') return d.getDay() === 0 ? 'İzin' : 'Gece';

            if(shiftSetting === 'default' && shiftRefDate === '2026-05-15') {
                let idx = (3 + diffDays) % 6; return cycle[idx < 0 ? idx + 6 : idx];
            }
            const cycleLen = cycle.length;
            let idx = diffDays % cycleLen;
            if (idx < 0) idx += cycleLen;
            return cycle[idx];
        }

        function ayarlariKaydet() {
            const yeniDongu = document.getElementById('ayarlar-dongu').value;
            const yeniTarih = document.getElementById('ayarlar-tarih').value;
            if(!yeniTarih) { alert("Lütfen referans tarihi seçin."); return; }
            shiftSetting = yeniDongu; shiftRefDate = yeniTarih;
            localStorage.setItem('v_shift_setting', shiftSetting);
            localStorage.setItem('v_shift_ref', shiftRefDate);
            initTodayWidget(); 
            alert("Ayarlar kaydedildi!"); showSection('menu');
        }

        // TAŞMAYAN, ESTETİK TAKVİM TASARIMI
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

                const dateKey = `2026-${mIndex+1}-${d}`;
                const hasNote = appNotes[dateKey] ? true : false;

                const div = document.createElement('div');
                // Optimizasyon: aspect-square veya sabit height. 
                // Taşmayı önleyen en kritik kısım text-truncate ve doğru padding/height.
                let baseClass = "relative flex flex-col items-center justify-center rounded-[12px] sm:rounded-xl border border-slate-200/50 dark:border-white/5 w-full h-[60px] min-[375px]:h-[66px] sm:h-[72px] overflow-hidden cursor-pointer hover:scale-[1.03] active:scale-95 transition-transform shadow-sm ";
                let shiftClass = shift === 'Gündüz' ? 'bg-emerald-50/80 dark:bg-emerald-900/30 text-emerald-700 dark:text-emerald-400' : 
                                 shift === 'Gece' ? 'bg-indigo-50/80 dark:bg-indigo-900/30 text-indigo-700 dark:text-indigo-400' : 
                                 'bg-white dark:bg-slate-800/60 text-slate-500 dark:text-slate-400';

                let holidayIndicator = holidayName ? `<span class="absolute top-1 right-1 w-1.5 h-1.5 sm:w-2 sm:h-2 bg-rose-500 rounded-full shadow-[0_0_4px_rgba(244,63,94,0.6)]"></span>` : "";
                let noteIndicator = hasNote ? `<span class="absolute top-1 left-1 w-1.5 h-1.5 sm:w-2 sm:h-2 bg-blue-500 rounded-full shadow-[0_0_4px_rgba(59,130,246,0.6)]"></span>` : "";

                if(holidayName) baseClass += " !border-rose-300 dark:!border-rose-500/50 ";
                if(hasNote) baseClass += " !border-blue-300 dark:!border-blue-500/50 ";

                div.className = baseClass + shiftClass;
                div.onclick = () => openNoteModal(d, mIndex, mName);
                
                // Truncate ile GÜNDÜZ yazısının asla taşmamasını garanti ediyoruz.
                div.innerHTML = `
                    ${holidayIndicator}${noteIndicator}
                    <span class="text-[13px] sm:text-[15px] font-bold leading-none mb-1">${d}</span>
                    <span class="text-[8px] min-[375px]:text-[9px] min-[400px]:text-[10px] font-extrabold w-full text-center leading-none uppercase tracking-tighter truncate px-0.5">${shift}</span>
                `;
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

        function openNoteModal(day, monthIdx, monthName) {
            activeNoteDateKey = `2026-${monthIdx+1}-${day}`;
            document.getElementById('note-modal-date').innerText = `${day} ${monthName} 2026`;
            document.getElementById('note-input').value = appNotes[activeNoteDateKey] || '';
            document.getElementById('note-modal').classList.remove('hidden');
        }

        function closeNoteModal() { document.getElementById('note-modal').classList.add('hidden'); }

        function saveNote() {
            const text = document.getElementById('note-input').value.trim();
            if(text) appNotes[activeNoteDateKey] = text; else delete appNotes[activeNoteDateKey];
            localStorage.setItem('v_notes', JSON.stringify(appNotes));
            closeNoteModal(); showCalendar(activeMonthIndex, activeMonthName);
        }

        function deleteNote() {
            delete appNotes[activeNoteDateKey];
            localStorage.setItem('v_notes', JSON.stringify(appNotes));
            closeNoteModal(); showCalendar(activeMonthIndex, activeMonthName);
        }

        function generateYearlyReport() {
            showSection('yillik-rapor');
            let totalGunduz = 0, totalGece = 0, totalIzin = 0;
            const bas = new Date(2026, 0, 1), bit = new Date(2026, 11, 31);
            for (let d = new Date(bas); d <= bit; d.setDate(d.getDate() + 1)) {
                const s = getShift(new Date(d));
                if(s === 'Gündüz') totalGunduz++; else if(s === 'Gece') totalGece++; else totalIzin++;
            }
            animateValue('yil-gunduz', 0, totalGunduz, 1000);
            animateValue('yil-gece', 0, totalGece, 1000);
            animateValue('yil-izin', 0, totalIzin, 1000);
        }

        function animateValue(id, start, end, duration) {
            let obj = document.getElementById(id); let startT = null;
            const step = (t) => {
                if (!startT) startT = t; const prog = Math.min((t - startT) / duration, 1);
                obj.innerHTML = Math.floor(prog * (end - start) + start);
                if (prog < 1) window.requestAnimationFrame(step);
            };
            window.requestAnimationFrame(step);
        }

        function copyToClipboard() {
            let textData = `📅 ${activeMonthName} 2026 Vardiya Planı:\n\n`;
            for (let d = 1; d <= new Date(2026, activeMonthIndex + 1, 0).getDate(); d++) {
                const shift = getShift(new Date(2026, activeMonthIndex, d));
                let icon = shift === 'Gündüz' ? '☀️' : (shift === 'Gece' ? '🌙' : '🏖️');
                textData += `${d} ${activeMonthName}: ${icon} ${shift}\n`;
            }
            textData += `\n📊 Özet: ${document.getElementById('stat-gunduz').innerText} Gündüz, ${document.getElementById('stat-gece').innerText} Gece`;
            navigator.clipboard.writeText(textData).then(() => {
                const btn = document.getElementById('copy-btn'); const old = btn.innerHTML;
                btn.innerHTML = '✅'; setTimeout(() => { btn.innerHTML = old; }, 2000);
            });
        }

        function exportIcs() {
            let ics = ["BEGIN:VCALENDAR", "VERSION:2.0", "PRODID:-//Vardiya//TR", "CALSCALE:GREGORIAN"];
            const mF = (activeMonthIndex + 1).toString().padStart(2, '0');
            for (let d = 1; d <= new Date(2026, activeMonthIndex + 1, 0).getDate(); d++) {
                const shift = getShift(new Date(2026, activeMonthIndex, d));
                if (shift === 'Gündüz' || shift === 'Gece') {
                    const dF = d.toString().padStart(2, '0'); const base = `2026${mF}${dF}`;
                    let sTime = shift === 'Gündüz' ? "080000" : "200000", eTime = shift === 'Gündüz' ? "200000" : "080000";
                    let endStr = `${base}T${eTime}`;
                    if (shift === 'Gece') { const nDay = new Date(2026, activeMonthIndex, d + 1); endStr = `2026${(nDay.getMonth()+1).toString().padStart(2,'0')}${nDay.getDate().toString().padStart(2,'0')}T${eTime}`; }
                    ics.push("BEGIN:VEVENT", `UID:va_${base}_${shift}`, `DTSTAMP:${base}T000000Z`, `DTSTART;TZID=Europe/Istanbul:${base}T${sTime}`, `DTEND;TZID=Europe/Istanbul:${endStr}`, `SUMMARY:${shift} Vardiyası`, "END:VEVENT");
                }
            }
            ics.push("END:VCALENDAR");
            const blob = new Blob([ics.join("\r\n")], { type: 'text/calendar' });
            const link = document.createElement('a'); link.href = window.URL.createObjectURL(blob); link.download = `Vardiya_${activeMonthName}_2026.ics`;
            document.body.appendChild(link); link.click(); document.body.removeChild(link);
        }

        function hesaplaIzin() {
            const startStr = document.getElementById('start-date').value; const gun = parseInt(document.getElementById('izin-gun').value);
            const toplamHak = parseInt(document.getElementById('izin-toplam-hak').value) || 0;
            if(toplamHak > 0) localStorage.setItem('v_izin_hakkim', document.getElementById('izin-toplam-hak').value);
            const sonucKutu = document.getElementById('izin-sonuc');
            if(!startStr || isNaN(gun) || gun < 1) {
                sonucKutu.classList.remove('hidden'); sonucKutu.innerHTML = `<span class="text-rose-600 font-bold">Lütfen tarih/gün girin.</span>`;
                sonucKutu.className = "p-5 bg-rose-50 dark:bg-rose-900/20 rounded-2xl text-center border border-rose-200 mt-4"; return;
            }
            const start = new Date(startStr); start.setDate(start.getDate() + gun + 2);
            
            let bakiyeHTML = "";
            if (toplamHak > 0) {
                let kalanHak = Math.max(0, toplamHak - gun); let yuzde = Math.max(0, (kalanHak / toplamHak) * 100);
                let barColor = yuzde > 50 ? 'bg-emerald-500' : (yuzde > 20 ? 'bg-orange-400' : 'bg-rose-500');
                bakiyeHTML = `<div class="mt-4 pt-4 border-t border-purple-200/50 dark:border-purple-800/50"><div class="flex justify-between text-[11px] font-bold text-purple-700/80 dark:text-purple-300/80 mb-1.5"><span>Kalan İzin Bakiyen</span><span>${kalanHak} / ${toplamHak} Gün</span></div><div class="w-full bg-purple-100 dark:bg-slate-800 rounded-full h-2.5 overflow-hidden"><div class="${barColor} h-2.5 rounded-full progress-bar-fill" style="width: 0%"></div></div></div>`;
                setTimeout(() => { document.querySelectorAll('.progress-bar-fill').forEach(b => b.style.width = `${yuzde}%`); }, 100);
            }
            sonucKutu.classList.remove('hidden');
            sonucKutu.className = "p-6 bg-gradient-to-br from-purple-50 to-indigo-50 dark:from-purple-900/40 dark:to-indigo-900/40 text-purple-900 dark:text-purple-100 rounded-2xl text-left border border-purple-200 dark:border-purple-700/50 shadow-sm mt-4";
            sonucKutu.innerHTML = `<div class="text-center"><div class="text-[11px] font-extrabold uppercase tracking-widest text-purple-500 mb-2">İşe Başlama Tarihi</div><div class="text-2xl font-black truncate">${start.toLocaleDateString('tr-TR', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}</div></div>${bakiyeHTML}`;
        }

        function setMultiplier(val) { selectedMultiplier = val; document.getElementById('mesai-carpan').value = val; updateMultButtons(); }
        function customMultiplierInput() { const val = parseFloat(document.getElementById('mesai-carpan').value); if(!isNaN(val)) { selectedMultiplier = val; updateMultButtons(true); } }
        function updateMultButtons(isC = false) {
            document.querySelectorAll('.mult-btn').forEach(btn => btn.className = "mult-btn py-3.5 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-400 border border-slate-200 dark:border-slate-700 rounded-xl text-xs font-bold transition-all");
            if (!isC) { const aBtn = document.getElementById(`btn-mult-${selectedMultiplier.toFixed(1)}`); if (aBtn) aBtn.className = "mult-btn py-3.5 bg-emerald-600 text-white rounded-xl text-xs font-black shadow-md transition-all"; }
        }

        function hesaplaMesai() {
            const saat = parseFloat(document.getElementById('mesai-saat').value) || 0, ucret = parseFloat(document.getElementById('mesai-ucret').value) || 0, maas = parseFloat(document.getElementById('mesai-maas').value) || 0;
            const yolUcret = parseFloat(document.getElementById('mesai-yol-ucret').value) || 0, yolGun = parseFloat(document.getElementById('mesai-yol-gun').value) || 0;
            const yemekUcret = parseFloat(document.getElementById('mesai-yemek-ucret').value) || 0, yemekGun = parseFloat(document.getElementById('mesai-yemek-gun').value) || 0;
            
            localStorage.setItem('v_maas', document.getElementById('mesai-maas').value); localStorage.setItem('v_ucret', document.getElementById('mesai-ucret').value);
            localStorage.setItem('v_yol_ucret', document.getElementById('mesai-yol-ucret').value); localStorage.setItem('v_yemek_ucret', document.getElementById('mesai-yemek-ucret').value);
            const sonucKutu = document.getElementById('mesai-sonuc');

            if(saat === 0 && ucret === 0 && maas === 0 && yolUcret*yolGun === 0 && yemekUcret*yemekGun === 0) {
                sonucKutu.classList.remove('hidden'); sonucKutu.innerHTML = `<span class="text-rose-600 font-bold">Lütfen hesaplanacak bir değer girin.</span>`;
                sonucKutu.className = "p-5 bg-rose-50 dark:bg-rose-900/20 rounded-2xl border border-rose-200 mt-4 text-center"; return;
            }
            const mesaiKazanci = saat * ucret * selectedMultiplier, genelToplam = maas + mesaiKazanci + (yolUcret*yolGun) + (yemekUcret*yemekGun);
            let ekstralar = []; if (maas > 0) ekstralar.push(`${maas}₺ Maaş`); if (yolUcret*yolGun > 0) ekstralar.push(`${yolUcret}₺×${yolGun} Yol`); if (yemekUcret*yemekGun > 0) ekstralar.push(`${yemekUcret}₺×${yemekGun} Yemek`);
            
            sonucKutu.classList.remove('hidden');
            sonucKutu.className = "p-6 bg-gradient-to-br from-emerald-50 to-teal-50 dark:from-emerald-900/40 dark:to-teal-900/40 text-emerald-900 dark:text-emerald-100 rounded-2xl text-center border border-emerald-200 dark:border-emerald-700/50 shadow-sm mt-4";
            sonucKutu.innerHTML = `<div class="text-[11px] font-extrabold uppercase tracking-widest text-emerald-600 dark:text-emerald-400 mb-2">Toplam Hakediş Tutarı</div><div class="text-4xl font-black mb-2">${genelToplam.toLocaleString('tr-TR', { minimumFractionDigits: 2 })} ₺</div><div class="text-[11px] text-emerald-700/90 dark:text-emerald-300/90 font-bold mt-2 pt-2 border-t border-emerald-200/50 inline-block">[( ${saat}s × ${ucret}₺ × ${selectedMultiplier} ) ${ekstralar.length > 0 ? `<br>+ ${ekstralar.join(' & ')}` : ''}]</div>`;
        }
    </script>
</body>
</html>

