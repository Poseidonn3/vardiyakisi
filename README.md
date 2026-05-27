
<!DOCTYPE html>
<html lang="tr" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
    <title>Vardiya Yönetim Merkezi</title>
    
    <!-- PWA & Mobil Uyumluluk -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <meta name="theme-color" content="#f8fafc">
    
    <!-- Tailwind CSS ve Google Fonts -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: { 
                extend: { 
                    fontFamily: { sans: ['Plus Jakarta Sans', 'sans-serif'] },
                    colors: { darkbg: '#0F172A' }
                } 
            }
        }
    </script>

    <style>
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            -webkit-tap-highlight-color: transparent; 
            overflow-x: hidden;
        }
        .app-section { display: none; animation: fadeIn 0.4s ease-out; }
        .app-section.active { display: block; }
        
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        ::placeholder { color: #94a3b8; opacity: 1; }
        .dark ::placeholder { color: #64748b; }
        
        input, select { -webkit-appearance: none; box-sizing: border-box; }
        ::-webkit-scrollbar { display: none; }
        
        /* Geniş Ekran Uyumlu Cam Efekti */
        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid rgba(226, 232, 240, 0.8);
            box-shadow: 0 10px 30px -5px rgba(0, 0, 0, 0.05);
        }
        .dark .glass-card {
            background: rgba(30, 41, 59, 0.85);
            border: 1px solid rgba(51, 65, 85, 0.5);
            box-shadow: 0 10px 30px -5px rgba(0, 0, 0, 0.4);
        }

        /* YAZILARIN ASLA ALT ALTA İNMEMESİ İÇİN KESİN KURAL */
        .no-wrap-text {
            white-space: nowrap !important;
            word-break: keep-all !important;
        }

        /* Akıllı Kutu Oranlaması - Gündüz yazılarının sığmasını garantiler */
        .calendar-cell {
            min-height: 70px;
        }
        @media (min-width: 640px) {
            .calendar-cell { min-height: 90px; }
        }
    </style>
</head>
<body class="bg-slate-50 dark:bg-darkbg text-slate-800 dark:text-slate-100 min-h-screen pb-16 transition-colors duration-500 bg-[radial-gradient(ellipse_at_top,_var(--tw-gradient-stops))] from-blue-100/60 via-slate-50 to-slate-100 dark:from-indigo-900/20 dark:via-darkbg dark:to-darkbg w-full">

    <!-- MAX-WIDTH GENİŞLETİLDİ - EKRANI DAHA RAHAT DOLDURUR -->
    <div class="max-w-3xl mx-auto w-full px-4 sm:px-6 md:px-8 pt-6 sm:pt-10">
        
        <!-- ÜST BAŞLIK VE TEMA/AYARLAR -->
        <header class="mb-8 flex items-center justify-between w-full">
            <div>
                <h1 class="text-3xl sm:text-4xl font-black tracking-tighter bg-gradient-to-r from-blue-600 to-indigo-600 dark:from-blue-400 dark:to-indigo-400 bg-clip-text text-transparent no-wrap-text pb-1">Vardiya Akışı</h1>
                <p class="text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-[0.2em] mt-1 no-wrap-text">Akıllı Planlama Asistanı</p>
            </div>
            <div class="flex gap-2 sm:gap-3">
                <button onclick="showSection('ayarlar')" class="w-12 h-12 sm:w-14 sm:h-14 rounded-2xl glass-card flex items-center justify-center hover:scale-105 active:scale-95 transition-all duration-300 shadow-sm border border-slate-200 dark:border-slate-700">
                    <span class="text-2xl sm:text-3xl leading-none text-slate-600 dark:text-slate-300">⚙️</span>
                </button>
                <button onclick="toggleTheme()" class="w-12 h-12 sm:w-14 sm:h-14 rounded-2xl glass-card flex items-center justify-center hover:scale-105 active:scale-95 transition-all duration-300 shadow-sm border border-slate-200 dark:border-slate-700">
                    <span id="theme-icon" class="text-2xl sm:text-3xl leading-none">🌙</span>
                </button>
            </div>
        </header>

        <!-- ANA MENÜ -->
        <div id="section-menu" class="app-section active w-full space-y-6">
            
            <!-- BUGÜNÜN ÖZETİ KARTI (Geniş ve Ferah) -->
            <div class="w-full p-6 sm:p-8 rounded-[32px] bg-gradient-to-br from-indigo-600 to-blue-700 text-white shadow-xl relative overflow-hidden">
                <div class="absolute -right-10 -top-10 w-64 h-64 bg-white/10 rounded-full blur-3xl pointer-events-none"></div>
                <div class="absolute -left-10 -bottom-10 w-48 h-48 bg-black/15 rounded-full blur-2xl pointer-events-none"></div>
                <div class="relative z-10 w-full flex flex-col md:flex-row items-start md:items-center justify-between gap-6">
                    <div class="flex-1 min-w-0">
                        <p class="text-[11px] sm:text-xs font-black uppercase tracking-[0.2em] text-blue-100 mb-2 no-wrap-text" id="today-date-text">Bugün</p>
                        <h2 class="text-3xl sm:text-4xl font-black no-wrap-text tracking-tight mb-3" id="today-shift-text">Yükleniyor...</h2>
                        
                        <div id="sleep-advisor" class="py-2.5 px-4 bg-black/20 rounded-xl text-xs sm:text-sm font-semibold text-blue-50 flex items-center gap-3 border border-white/10 hidden backdrop-blur-md shadow-sm mt-2 max-w-fit">
                            <span id="sleep-advisor-icon" class="text-lg">🛌</span>
                            <span id="sleep-advisor-text" class="no-wrap-text">Tavsiye yükleniyor...</span>
                        </div>
                    </div>
                    <div class="w-20 h-20 sm:w-24 sm:h-24 rounded-[24px] bg-white/20 flex-shrink-0 flex items-center justify-center text-4xl sm:text-5xl backdrop-blur-md border border-white/20 shadow-inner" id="today-icon">
                        ⏳
                    </div>
                </div>
                
                <div id="future-radar" class="mt-6 pt-5 border-t border-white/20 flex justify-between items-center text-xs sm:text-sm font-bold text-blue-100 hidden">
                    <span id="tomorrow-shift-text" class="no-wrap-text tracking-wide">🔜 Yarın: -</span>
                    <span id="next-leave-text" class="shrink-0 bg-white/15 px-4 py-2 rounded-xl border border-white/10 no-wrap-text shadow-sm">🏖️ İzin: -</span>
                </div>
            </div>

            <!-- ALT ALTA ANA MENÜ BUTONLARI -->
            <div class="flex flex-col gap-4 sm:gap-5 w-full">
                <button onclick="showSection('takvim-aylar')" class="w-full p-5 sm:p-7 glass-card rounded-[28px] hover:border-blue-500 hover:shadow-lg active:scale-[0.98] transition-all flex items-center gap-5 sm:gap-6 group">
                    <div class="w-14 h-14 sm:w-16 sm:h-16 rounded-2xl bg-blue-50 dark:bg-blue-900/30 flex items-center justify-center text-3xl sm:text-4xl group-hover:scale-110 transition-transform shadow-sm">📅</div>
                    <div class="text-left flex-1">
                        <h2 class="text-lg sm:text-xl font-black text-slate-800 dark:text-slate-100 tracking-tight no-wrap-text">Aylık Takvim Paneli</h2>
                        <p class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 font-bold mt-1">İstatistikler, tatiller ve detaylı görünümler.</p>
                    </div>
                    <span class="text-2xl text-slate-300 dark:text-slate-600 font-black group-hover:translate-x-2 transition-transform">→</span>
                </button>
                
                <button onclick="showSection('izin-hesap')" class="w-full p-5 sm:p-7 glass-card rounded-[28px] hover:border-purple-500 hover:shadow-lg active:scale-[0.98] transition-all flex items-center gap-5 sm:gap-6 group">
                    <div class="w-14 h-14 sm:w-16 sm:h-16 rounded-2xl bg-purple-50 dark:bg-purple-900/30 flex items-center justify-center text-3xl sm:text-4xl group-hover:scale-110 transition-transform shadow-sm">✈️</div>
                    <div class="text-left flex-1">
                        <h2 class="text-lg sm:text-xl font-black text-slate-800 dark:text-slate-100 tracking-tight no-wrap-text">İzin Planlayıcı</h2>
                        <p class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 font-bold mt-1">Yıllık izin hesaplama ve bakiye takibi.</p>
                    </div>
                    <span class="text-2xl text-slate-300 dark:text-slate-600 font-black group-hover:translate-x-2 transition-transform">→</span>
                </button>
                
                <button onclick="showSection('mesai-hesap')" class="w-full p-5 sm:p-7 glass-card rounded-[28px] hover:border-emerald-500 hover:shadow-lg active:scale-[0.98] transition-all flex items-center gap-5 sm:gap-6 group">
                    <div class="w-14 h-14 sm:w-16 sm:h-16 rounded-2xl bg-emerald-50 dark:bg-emerald-900/30 flex items-center justify-center text-3xl sm:text-4xl group-hover:scale-110 transition-transform shadow-sm">💰</div>
                    <div class="text-left flex-1">
                        <h2 class="text-lg sm:text-xl font-black text-slate-800 dark:text-slate-100 tracking-tight no-wrap-text">Kazanç & Mesai Hesaplama</h2>
                        <p class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 font-bold mt-1">Maaş, ek mesai, yol ve yemek hakedişleri.</p>
                    </div>
                    <span class="text-2xl text-slate-300 dark:text-slate-600 font-black group-hover:translate-x-2 transition-transform">→</span>
                </button>
                
                <button onclick="generateYearlyReport()" class="w-full p-5 sm:p-7 glass-card rounded-[28px] hover:border-orange-500 hover:shadow-lg active:scale-[0.98] transition-all flex items-center gap-5 sm:gap-6 group relative overflow-hidden">
                    <div class="absolute inset-0 bg-gradient-to-r from-orange-500/5 to-rose-500/5 opacity-0 group-hover:opacity-100 transition-opacity"></div>
                    <div class="w-14 h-14 sm:w-16 sm:h-16 rounded-2xl bg-orange-50 dark:bg-orange-900/30 flex items-center justify-center text-3xl sm:text-4xl group-hover:scale-110 transition-transform shadow-sm relative z-10">📊</div>
                    <div class="text-left flex-1 relative z-10">
                        <h2 class="text-lg sm:text-xl font-black text-slate-800 dark:text-slate-100 tracking-tight no-wrap-text">Yıllık Gelişmiş Rapor</h2>
                        <p class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 font-bold mt-1">2026 yılının tüm çalışma döngüsünü listeler.</p>
                    </div>
                    <span class="text-2xl text-slate-300 dark:text-slate-600 font-black group-hover:translate-x-2 transition-transform relative z-10">→</span>
                </button>
            </div>
        </div>

        <!-- GERİ DÖNÜŞ BUTONU ŞABLONU -->
        
        <!-- AYARLAR BÖLÜMÜ (Artık Hatasız Çalışıyor) -->
        <div id="section-ayarlar" class="app-section w-full space-y-6">
            <button onclick="showSection('menu')" class="flex items-center gap-2 text-blue-600 dark:text-blue-400 font-extrabold text-base py-2 hover:opacity-80 transition-opacity active:scale-95 outline-none no-wrap-text w-full">
                <span class="text-3xl leading-none font-bold mb-1">←</span> Ana Menüye Dön
            </button>
            
            <div class="glass-card p-6 sm:p-10 rounded-[32px] space-y-6 w-full">
                <div>
                    <h2 class="text-2xl sm:text-3xl font-black text-slate-800 dark:text-slate-100 flex items-center gap-3 mb-2 tracking-tight">⚙️ Sistem Ayarları</h2>
                    <p class="text-sm text-slate-500 dark:text-slate-400 font-bold">Kendi çalışma döngünüzü belirleyin.</p>
                </div>
                
                <div class="w-full mt-6">
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-[0.15em] mb-3 ml-1 no-wrap-text">Çalışma Döngüsü</label>
                    <div class="relative w-full">
                        <select id="ayarlar-dongu" class="w-full px-5 py-5 bg-slate-50 dark:bg-slate-900 rounded-[20px] border border-slate-200 dark:border-slate-700 focus:ring-4 focus:ring-blue-500/30 outline-none transition-all text-base font-black text-slate-800 dark:text-slate-100 appearance-none cursor-pointer shadow-inner">
                            <option value="default">Klasik (2 Gündüz, 2 Gece, 2 İzin)</option>
                            <option value="4g2i">4 Gündüz, 2 İzin</option>
                            <option value="sabit_g">Sabit Gündüz (Pazar İzin)</option>
                            <option value="sabit_ge">Sabit Gece (Pazar İzin)</option>
                        </select>
                        <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-6 text-slate-500 text-lg">▼</div>
                    </div>
                </div>

                <div class="w-full">
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-[0.15em] mb-3 ml-1 no-wrap-text">Referans Başlangıç Tarihi</label>
                    <input type="date" id="ayarlar-tarih" class="w-full px-5 py-5 bg-slate-50 dark:bg-slate-900 rounded-[20px] border border-slate-200 dark:border-slate-700 focus:ring-4 focus:ring-blue-500/30 outline-none transition-all text-base font-black text-slate-800 dark:text-slate-100 shadow-inner block">
                    <p class="text-xs text-slate-500 dark:text-slate-400 mt-3 font-semibold ml-1 leading-relaxed">* Döngünün <span class="text-emerald-600 dark:text-emerald-400 font-bold">1. Gündüzüne (İlk iş gününe)</span> denk gelen bir tarih seçin.</p>
                </div>
                
                <button onclick="ayarlariKaydet()" class="w-full py-5 mt-4 bg-blue-600 hover:bg-blue-700 active:scale-[0.99] text-white rounded-[20px] font-black text-lg transition-all shadow-xl shadow-blue-600/20 tracking-wide no-wrap-text">
                    Tüm Ayarları Kaydet
                </button>
            </div>
        </div>

        <!-- YILLIK RAPOR BÖLÜMÜ -->
        <div id="section-yillik-rapor" class="app-section w-full space-y-6">
            <button onclick="showSection('menu')" class="flex items-center gap-2 text-blue-600 dark:text-blue-400 font-extrabold text-base py-2 hover:opacity-80 transition-opacity active:scale-95 outline-none no-wrap-text w-full">
                <span class="text-3xl leading-none font-bold mb-1">←</span> Ana Menüye Dön
            </button>
            
            <div class="glass-card p-8 sm:p-12 rounded-[32px] space-y-6 w-full">
                <div class="text-center mb-10">
                    <span class="text-6xl sm:text-7xl block mb-4 drop-shadow-lg">📊</span>
                    <h2 class="text-3xl sm:text-4xl font-black text-slate-800 dark:text-slate-100 tracking-tight no-wrap-text">2026 Yıllık Analiz</h2>
                    <p class="text-sm sm:text-base text-slate-500 dark:text-slate-400 font-bold mt-2">Tüm yılın çalışma temposu (Ayarlara göre)</p>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 w-full">
                    <div class="bg-emerald-50 dark:bg-emerald-900/20 p-8 rounded-[24px] border border-emerald-200 dark:border-emerald-800/50 text-center shadow-md">
                        <span class="block text-xs sm:text-sm font-black text-emerald-600 dark:text-emerald-400 uppercase tracking-[0.2em] mb-2 no-wrap-text">Toplam Gündüz</span>
                        <span id="yil-gunduz" class="text-5xl sm:text-6xl font-black text-emerald-700 dark:text-emerald-300">0</span>
                    </div>
                    <div class="bg-indigo-50 dark:bg-indigo-900/20 p-8 rounded-[24px] border border-indigo-200 dark:border-indigo-800/50 text-center shadow-md">
                        <span class="block text-xs sm:text-sm font-black text-indigo-600 dark:text-indigo-400 uppercase tracking-[0.2em] mb-2 no-wrap-text">Toplam Gece</span>
                        <span id="yil-gece" class="text-5xl sm:text-6xl font-black text-indigo-700 dark:text-indigo-300">0</span>
                    </div>
                    <div class="bg-slate-50 dark:bg-slate-800/40 p-8 rounded-[24px] border border-slate-200 dark:border-slate-700/50 text-center shadow-md">
                        <span class="block text-xs sm:text-sm font-black text-slate-500 dark:text-slate-400 uppercase tracking-[0.2em] mb-2 no-wrap-text">Toplam İzin</span>
                        <span id="yil-izin" class="text-5xl sm:text-6xl font-black text-slate-700 dark:text-slate-200">0</span>
                    </div>
                </div>
            </div>
        </div>

        <!-- TAKVİM: AY LİSTESİ -->
        <div id="section-takvim-aylar" class="app-section w-full space-y-6">
            <button onclick="showSection('menu')" class="flex items-center gap-2 text-blue-600 dark:text-blue-400 font-extrabold text-base py-2 hover:opacity-80 transition-opacity active:scale-95 outline-none no-wrap-text w-full">
                <span class="text-3xl leading-none font-bold mb-1">←</span> Ana Menüye Dön
            </button>
            <div id="month-list" class="grid grid-cols-2 md:grid-cols-3 gap-4 sm:gap-6 w-full"></div>
        </div>

        <!-- TAKVİM: DETAY (YAZILAR ASLA TAŞMAZ VE OKUNABİLİR) -->
        <div id="section-takvim-detay" class="app-section w-full space-y-6">
            <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 w-full glass-card p-5 sm:p-7 rounded-[28px] shadow-sm">
                <button onclick="showSection('takvim-aylar')" class="flex items-center gap-2 text-blue-600 dark:text-blue-400 font-extrabold text-base hover:opacity-80 transition-opacity active:scale-95 outline-none no-wrap-text">
                    <span class="text-3xl leading-none font-bold mb-1">←</span> Aylar
                </button>
                <h2 id="month-name" class="text-2xl sm:text-3xl font-black text-slate-800 dark:text-slate-100 tracking-tighter no-wrap-text mx-auto sm:mx-0"></h2>
                <div class="flex items-center gap-3 w-full sm:w-auto justify-end">
                    <button onclick="copyToClipboard()" id="copy-btn" class="flex-1 sm:flex-none px-5 py-3.5 rounded-xl bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-200 font-extrabold hover:scale-105 active:scale-95 transition-all text-sm no-wrap-text shadow-sm border border-slate-200 dark:border-slate-700">
                        📋 Kopyala
                    </button>
                    <button onclick="exportIcs()" class="flex-1 sm:flex-none px-5 py-3.5 rounded-xl bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-200 font-extrabold hover:scale-105 active:scale-95 transition-all text-sm no-wrap-text shadow-sm border border-slate-200 dark:border-slate-700">
                        📤 İndir (.ics)
                    </button>
                </div>
            </div>
            
            <div class="grid grid-cols-3 gap-4 w-full">
                <div class="glass-card py-4 sm:py-5 rounded-[24px] text-center shadow-md">
                    <span class="block text-[10px] sm:text-xs font-black text-emerald-600 dark:text-emerald-400 uppercase tracking-[0.2em] mb-1 no-wrap-text">Gündüz</span>
                    <span id="stat-gunduz" class="text-2xl sm:text-3xl font-black text-emerald-700 dark:text-emerald-300">0</span>
                </div>
                <div class="glass-card py-4 sm:py-5 rounded-[24px] text-center shadow-md">
                    <span class="block text-[10px] sm:text-xs font-black text-indigo-600 dark:text-indigo-400 uppercase tracking-[0.2em] mb-1 no-wrap-text">Gece</span>
                    <span id="stat-gece" class="text-2xl sm:text-3xl font-black text-indigo-700 dark:text-indigo-300">0</span>
                </div>
                <div class="glass-card py-4 sm:py-5 rounded-[24px] text-center shadow-md">
                    <span class="block text-[10px] sm:text-xs font-black text-slate-500 dark:text-slate-400 uppercase tracking-[0.2em] mb-1 no-wrap-text">İzin</span>
                    <span id="stat-izin" class="text-2xl sm:text-3xl font-black text-slate-700 dark:text-slate-200">0</span>
                </div>
            </div>

            <!-- EKRANI DOLDURAN VE KESİNLİKLE TAŞMAYAN TAKVİM IZGARASI -->
            <div class="glass-card p-3 sm:p-6 md:p-8 rounded-[32px] shadow-xl w-full">
                <!-- Gün İsimleri (Kısaltmasız ve net) -->
                <div class="grid grid-cols-7 gap-1 sm:gap-2 md:gap-3 text-center w-full mb-3 sm:mb-5">
                    <div class="text-[9px] sm:text-[11px] md:text-sm font-black text-slate-500 dark:text-slate-400 uppercase tracking-widest no-wrap-text">Pzt</div>
                    <div class="text-[9px] sm:text-[11px] md:text-sm font-black text-slate-500 dark:text-slate-400 uppercase tracking-widest no-wrap-text">Salı</div>
                    <div class="text-[9px] sm:text-[11px] md:text-sm font-black text-slate-500 dark:text-slate-400 uppercase tracking-widest no-wrap-text">Çar</div>
                    <div class="text-[9px] sm:text-[11px] md:text-sm font-black text-slate-500 dark:text-slate-400 uppercase tracking-widest no-wrap-text">Per</div>
                    <div class="text-[9px] sm:text-[11px] md:text-sm font-black text-slate-500 dark:text-slate-400 uppercase tracking-widest no-wrap-text">Cuma</div>
                    <div class="text-[9px] sm:text-[11px] md:text-sm font-black text-slate-500 dark:text-slate-400 uppercase tracking-widest no-wrap-text">Cmt</div>
                    <div class="text-[9px] sm:text-[11px] md:text-sm font-black text-slate-500 dark:text-slate-400 uppercase tracking-widest no-wrap-text">Pzr</div>
                </div>
                <!-- Kutu Alanı (Hücre yüksekliği ve metin boyutu garanti altına alındı) -->
                <div id="calendar-grid" class="grid grid-cols-7 gap-1.5 sm:gap-2 md:gap-3 text-center w-full"></div>
            </div>

            <!-- Özel Günler Kutusu -->
            <div id="holiday-box" class="hidden w-full bg-gradient-to-br from-rose-50 to-orange-50 dark:from-rose-950/40 dark:to-orange-950/40 p-6 sm:p-8 rounded-[28px] shadow-lg border-2 border-rose-200 dark:border-rose-800/50">
                <h3 class="text-sm sm:text-base font-black text-rose-600 dark:text-rose-400 uppercase tracking-[0.2em] mb-4 flex items-center gap-3 no-wrap-text">
                    <span class="text-2xl">🎉</span> Özel Günler ve Tatiller
                </h3>
                <ul id="holiday-list" class="space-y-3 text-sm sm:text-base md:text-lg font-bold text-rose-800 dark:text-rose-300"></ul>
            </div>
        </div>

        <!-- İZİN HESAPLAYICI -->
        <div id="section-izin-hesap" class="app-section w-full space-y-6">
            <button onclick="showSection('menu')" class="flex items-center gap-2 text-blue-600 dark:text-blue-400 font-extrabold text-base py-2 hover:opacity-80 transition-opacity active:scale-95 outline-none no-wrap-text w-full">
                <span class="text-3xl leading-none font-bold mb-1">←</span> Ana Menüye Dön
            </button>
            
            <div class="glass-card p-6 sm:p-10 rounded-[32px] space-y-8 w-full">
                
                <div class="bg-slate-50 dark:bg-slate-900/60 p-6 rounded-[24px] border border-slate-200 dark:border-slate-700/50">
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-[0.15em] mb-3 ml-1 no-wrap-text">🗂️ Toplam Yıllık İzin Hakkın</label>
                    <input type="number" id="izin-toplam-hak" placeholder="Örn: 14" class="w-full px-5 py-5 bg-white dark:bg-slate-800 rounded-[16px] border border-slate-200 dark:border-slate-700 focus:ring-4 focus:ring-purple-500/30 outline-none transition-all text-base font-black text-slate-800 dark:text-slate-100 shadow-sm block">
                </div>

                <div class="w-full">
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-[0.15em] mb-3 ml-1 no-wrap-text">İzin Başlangıç Tarihi</label>
                    <input type="date" id="start-date" class="w-full px-5 py-5 bg-slate-50 dark:bg-slate-900/60 rounded-[20px] border border-slate-200 dark:border-slate-700 focus:ring-4 focus:ring-purple-500/30 outline-none transition-all text-base font-black text-slate-800 dark:text-slate-100 shadow-inner block">
                </div>
                
                <div class="w-full">
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-[0.15em] mb-3 ml-1 no-wrap-text">Kullanılacak İzin (Gün)</label>
                    <input type="number" id="izin-gun" placeholder="Örn: 7" class="w-full px-5 py-5 bg-slate-50 dark:bg-slate-900/60 rounded-[20px] border border-slate-200 dark:border-slate-700 focus:ring-4 focus:ring-purple-500/30 outline-none transition-all text-base font-black text-slate-800 dark:text-slate-100 shadow-inner block">
                </div>
                
                <button onclick="hesaplaIzin()" class="w-full py-5 bg-purple-600 hover:bg-purple-700 active:scale-[0.99] text-white rounded-[20px] font-black text-lg transition-all shadow-xl shadow-purple-600/20 tracking-wide no-wrap-text">
                    İzni Hesapla ve Bakiyeyi Gör
                </button>
                
                <div id="izin-sonuc" class="hidden p-8 bg-purple-50 dark:bg-purple-900/30 text-purple-900 dark:text-purple-100 rounded-[24px] border border-purple-200 dark:border-purple-800/50 shadow-lg transition-all mt-6 w-full"></div>
            </div>
        </div>

        <!-- MESAİ VE KAZANÇ HESAPLAYICI -->
        <div id="section-mesai-hesap" class="app-section w-full space-y-6">
            <button onclick="showSection('menu')" class="flex items-center gap-2 text-blue-600 dark:text-blue-400 font-extrabold text-base py-2 hover:opacity-80 transition-opacity active:scale-95 outline-none no-wrap-text w-full">
                <span class="text-3xl leading-none font-bold mb-1">←</span> Ana Menüye Dön
            </button>
            
            <div class="glass-card p-6 sm:p-10 rounded-[32px] space-y-8 w-full">
                
                <div class="w-full">
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-[0.15em] mb-3 ml-1 no-wrap-text">Maaş / Asgari Ücret (TL)</label>
                    <input type="number" id="mesai-maas" placeholder="Örn: 17002" class="w-full px-5 py-5 bg-slate-50 dark:bg-slate-900/60 rounded-[20px] border border-slate-200 dark:border-slate-700 focus:ring-4 focus:ring-emerald-500/30 outline-none transition-all text-base font-black text-slate-800 dark:text-slate-100 shadow-inner block">
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-6 w-full">
                    <div class="w-full">
                        <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-[0.15em] mb-3 ml-1 no-wrap-text">Toplam Mesai Saati</label>
                        <input type="number" id="mesai-saat" placeholder="Örn: 12.5" step="0.5" class="w-full px-5 py-5 bg-slate-50 dark:bg-slate-900/60 rounded-[20px] border border-slate-200 dark:border-slate-700 focus:ring-4 focus:ring-emerald-500/30 outline-none transition-all text-base font-black text-slate-800 dark:text-slate-100 shadow-inner block">
                    </div>
                    
                    <div class="w-full">
                        <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-[0.15em] mb-3 ml-1 no-wrap-text">Saatlik Ücret (TL)</label>
                        <input type="number" id="mesai-ucret" placeholder="Örn: 150" class="w-full px-5 py-5 bg-slate-50 dark:bg-slate-900/60 rounded-[20px] border border-slate-200 dark:border-slate-700 focus:ring-4 focus:ring-emerald-500/30 outline-none transition-all text-base font-black text-slate-800 dark:text-slate-100 shadow-inner block">
                    </div>
                </div>

                <div class="w-full border-t border-slate-200 dark:border-slate-700 pt-8 mt-4">
                    <label class="block text-xs font-extrabold text-slate-500 dark:text-slate-400 uppercase tracking-[0.15em] mb-4 ml-1 no-wrap-text">Mesai Çarpanı</label>
                    <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 w-full">
                        <button onclick="setMultiplier(1.5)" id="btn-mult-1.5" class="mult-btn py-4 bg-emerald-500 text-white rounded-[16px] text-sm sm:text-base font-black shadow-lg transition-all no-wrap-text">1.5x Çarpan</button>
                        <button onclick="setMultiplier(2.0)" id="btn-mult-2.0" class="mult-btn py-4 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 rounded-[16px] text-sm sm:text-base font-extrabold border border-slate-200 dark:border-slate-700 transition-all no-wrap-text">2.0x Çarpan</button>
                        <button onclick="setMultiplier(1.0)" id="btn-mult-1.0" class="mult-btn py-4 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 rounded-[16px] text-sm sm:text-base font-extrabold border border-slate-200 dark:border-slate-700 transition-all no-wrap-text">1.0x (Normal)</button>
                        <input type="number" id="mesai-carpan" oninput="customMultiplierInput()" step="0.1" class="w-full text-center px-2 py-4 bg-slate-50 dark:bg-slate-900 rounded-[16px] border border-slate-200 dark:border-slate-700 text-sm sm:text-base font-extrabold outline-none focus:ring-2 focus:ring-emerald-500 text-slate-700 dark:text-slate-200 block" placeholder="Özel Değer">
                    </div>
                </div>
                
                <button onclick="hesaplaMesai()" class="w-full py-5 bg-emerald-500 hover:bg-emerald-600 active:scale-[0.99] text-white rounded-[20px] font-black text-lg transition-all shadow-xl shadow-emerald-500/20 tracking-wide no-wrap-text mt-4">
                    Kazancı Hesapla
                </button>
                
                <div id="mesai-sonuc" class="hidden w-full p-8 bg-emerald-50 dark:bg-emerald-900/30 text-emerald-900 dark:text-emerald-100 rounded-[24px] border border-emerald-200 dark:border-emerald-800/50 shadow-lg text-center transition-all mt-6"></div>
            </div>
        </div>
    </div>

    <!-- NOT MODALI (Ajanda özelliği korundu) -->
    <div id="note-modal" class="fixed inset-0 z-50 flex items-center justify-center p-4 modal-overlay hidden">
        <div class="bg-white/95 dark:bg-slate-900/95 p-6 sm:p-8 rounded-[32px] shadow-2xl border border-slate-200 dark:border-slate-800 w-full max-w-sm modal-content relative backdrop-blur-xl">
            <button onclick="closeNoteModal()" class="absolute top-5 right-5 w-10 h-10 flex items-center justify-center rounded-full bg-slate-100 dark:bg-slate-800 text-slate-500 font-black hover:bg-rose-100 hover:text-rose-600 transition-colors text-lg">✕</button>
            <h3 class="text-[22px] font-black text-slate-800 dark:text-slate-100 mb-1 flex items-center gap-3 tracking-tight"><span class="text-[28px]">📝</span> Günlük Ajanda</h3>
            <p id="note-modal-date" class="text-xs font-extrabold text-blue-600 dark:text-blue-400 mb-6 uppercase tracking-widest no-wrap-text"></p>
            
            <textarea id="note-input" rows="4" placeholder="Fazla mesai, toplantı, vb. notlar..." class="w-full px-5 py-4 bg-slate-50 dark:bg-slate-950 rounded-[20px] border border-slate-200 dark:border-slate-800 focus:ring-4 focus:ring-blue-500/30 outline-none transition-all text-base font-semibold text-slate-700 dark:text-slate-200 resize-none mb-6 shadow-inner"></textarea>
            
            <div class="flex gap-4">
                <button onclick="saveNote()" class="flex-1 py-4 bg-blue-600 hover:bg-blue-700 text-white rounded-[16px] font-black text-base transition-colors shadow-lg shadow-blue-600/20 tracking-wide no-wrap-text">Kaydet</button>
                <button onclick="deleteNote()" class="py-4 px-6 bg-rose-50 dark:bg-rose-900/20 hover:bg-rose-100 text-rose-600 dark:text-rose-400 border border-rose-200 dark:border-rose-900/50 rounded-[16px] font-extrabold text-base transition-colors tracking-wide no-wrap-text">Sil</button>
            </div>
        </div>
    </div>

    <!-- KUSURSUZ ÇALIŞAN SCRİPTLER VE BUG FİXLERİ -->
    <script>
        const TR_HOLIDAYS_2026 = {
            "0-1": "Yılbaşı", "2-19": "Ramazan Bayramı Arifesi", "2-20": "Ramazan Bayramı (1. Gün)",
            "2-21": "Ramazan Bayramı (2. Gün)", "2-22": "Ramazan Bayramı (3. Gün)", "3-23": "Ulusal Egemenlik ve Çocuk Bayramı",
            "4-1": "Emek ve Dayanışma Günü", "4-19": "Atatürk'ü Anma, Gençlik ve Spor Bayramı", "4-26": "Kurban Bayramı Arifesi",
            "4-27": "Kurban Bayramı (1. Gün)", "4-28": "Kurban Bayramı (2. Gün)", "4-29": "Kurban Bayramı (3. Gün)",
            "4-30": "Kurban Bayramı (4. Gün)", "6-15": "Demokrasi ve Milli Birlik Günü", "7-30": "Zafer Bayramı",
            "9-29": "Cumhuriyet Bayramı"
        };

        let selectedMultiplier = 1.5;
        let activeMonthIndex = 0;
        let activeMonthName = "";
        
        let appNotes = JSON.parse(localStorage.getItem('v_notes')) || {};
        // Ayarları hafızadan çek
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
            
            document.getElementById('start-date').valueAsDate = new Date();

            document.getElementById('mesai-maas').value = localStorage.getItem('v_maas') || '';
            document.getElementById('mesai-ucret').value = localStorage.getItem('v_ucret') || '';
            document.getElementById('izin-toplam-hak').value = localStorage.getItem('v_izin_hakkim') || '';
            
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
                
                const advisorDiv = document.getElementById('sleep-advisor');
                const advisorText = document.getElementById('sleep-advisor-text');
                
                if(shift === 'Gündüz') {
                    advisorText.innerText = "Gece dinlenmesi: 23:00 - 06:30 önerilir."; advisorDiv.classList.remove('hidden');
                } else if(shift === 'Gece') {
                    advisorText.innerText = "Gündüz uykusu: 08:30 - 16:30 arası önerilir."; advisorDiv.classList.remove('hidden');
                } else {
                    advisorText.innerText = "Bugün kendinize zaman ayırın, iyice dinlenin!"; advisorDiv.classList.remove('hidden');
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
                btn.className = "py-6 sm:py-8 px-3 glass-card rounded-[24px] shadow-sm text-lg sm:text-xl font-black text-slate-800 dark:text-slate-100 border border-slate-200/60 dark:border-slate-700/40 hover:border-blue-500 hover:shadow-xl active:scale-95 transition-all w-full flex items-center justify-center tracking-wide no-wrap-text";
                btn.innerText = m;
                btn.onclick = () => showCalendar(i, m);
                container.appendChild(btn);
            });
        }

        // --- BUG FİX: AYARLARIN TAKVİME YANSIMASI İÇİN YENİ MATEMATİKSEL FONKSİYON ---
        function getShift(d) {
            const setting = shiftSetting;
            const refStr = shiftRefDate; // Örn: '2026-05-15'
            
            // Tarihi UTC karmaşası olmadan saf yerel saatle bölüp oluşturuyoruz
            const parts = refStr.split('-');
            if(parts.length !== 3) return "Hata";
            const refDate = new Date(parseInt(parts[0]), parseInt(parts[1])-1, parseInt(parts[2]), 0, 0, 0);
            
            const targetDate = new Date(d);
            targetDate.setHours(0,0,0,0);
            
            // İki tarih arasındaki tam gün farkını (saat farkı olmadan) buluyoruz
            const diffTime = targetDate.getTime() - refDate.getTime();
            const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24));
            
            let cycle = [];
            if (setting === 'default') cycle = ['Gündüz', 'Gündüz', 'Gece', 'Gece', 'İzin', 'İzin'];
            else if (setting === '4g2i') cycle = ['Gündüz', 'Gündüz', 'Gündüz', 'Gündüz', 'İzin', 'İzin'];
            else if (setting === 'sabit_g') return targetDate.getDay() === 0 ? 'İzin' : 'Gündüz';
            else if (setting === 'sabit_ge') return targetDate.getDay() === 0 ? 'İzin' : 'Gece';

            // Klasik sistemin orijinal 2026-05-15 ofsetini bozmamak için eski formül korunuyor
            if (setting === 'default' && refStr === '2026-05-15') {
                let idx = (3 + diffDays) % 6;
                return cycle[idx < 0 ? idx + 6 : idx];
            }

            // Yeni ayarlanmış tarih veya farklı döngü için kesin modüler matematik
            const len = cycle.length;
            let idx = diffDays % len;
            if (idx < 0) idx += len;
            
            return cycle[idx];
        }

        function ayarlariKaydet() {
            const yeniDongu = document.getElementById('ayarlar-dongu').value;
            const yeniTarih = document.getElementById('ayarlar-tarih').value;
            if(!yeniTarih) { alert("Lütfen referans tarihi seçin."); return; }
            
            shiftSetting = yeniDongu; 
            shiftRefDate = yeniTarih;
            
            localStorage.setItem('v_shift_setting', shiftSetting);
            localStorage.setItem('v_shift_ref', shiftRefDate);
            
            initTodayWidget(); // Bugün kartını hemen güncelle
            alert("Sistem başarıyla yeni vardiya düzeninize göre uyarlandı! Takvim yenilendi."); 
            showSection('menu');
        }

        // KUSURSUZ, TAŞMAYAN, EKRANI DOLDURAN TAKVİM TASARIMI
        function showCalendar(mIndex, mName) {
            activeMonthIndex = mIndex;
            activeMonthName = mName;
            showSection('takvim-detay');
            document.getElementById('month-name').innerText = mName + " 2026";
            
            const grid = document.getElementById('calendar-grid');
            const holidayBox = document.getElementById('holiday-box');
            const holidayList = document.getElementById('holiday-list');
            
            let cGunduz = 0, cGece = 0, cIzin = 0;
            grid.innerHTML = "";
            
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
                
                // calendar-cell sınıfı ile min-height garanti edildi. Kutular ekrana yayılır.
                let baseClass = "calendar-cell relative flex flex-col items-center justify-center rounded-[14px] sm:rounded-[20px] border border-slate-200/60 dark:border-white/5 w-full overflow-hidden hover:scale-[1.03] active:scale-95 transition-transform shadow-sm cursor-pointer ";
                let shiftClass = shift === 'Gündüz' ? 'bg-emerald-50 dark:bg-emerald-900/40 text-emerald-700 dark:text-emerald-400' : 
                                 shift === 'Gece' ? 'bg-indigo-50 dark:bg-indigo-900/40 text-indigo-700 dark:text-indigo-400' : 
                                 'bg-white dark:bg-slate-800/80 text-slate-500 dark:text-slate-400';

                let holidayIndicator = holidayName ? `<span class="absolute top-1.5 right-1.5 w-2 h-2 sm:w-2.5 sm:h-2.5 bg-rose-500 rounded-full shadow-[0_0_8px_rgba(244,63,94,0.8)]"></span>` : "";
                let noteIndicator = hasNote ? `<span class="absolute top-1.5 left-1.5 w-2 h-2 sm:w-2.5 sm:h-2.5 bg-blue-500 rounded-full shadow-[0_0_8px_rgba(59,130,246,0.8)]"></span>` : "";

                if(holidayName) baseClass += " !border-rose-400 dark:!border-rose-500/60 ";
                if(hasNote) baseClass += " !border-blue-400 dark:!border-blue-500/60 ";

                div.className = baseClass + shiftClass;
                div.onclick = () => openNoteModal(d, mIndex, mName);
                
                // WHİTESPACE-NOWRAP: Asla alt satıra geçmez, ekrana sığar
                div.innerHTML = `
                    ${holidayIndicator}${noteIndicator}
                    <span class="text-sm sm:text-lg md:text-xl font-black leading-none mb-1 sm:mb-2">${d}</span>
                    <span class="text-[9px] min-[360px]:text-[10px] sm:text-xs md:text-sm font-extrabold w-full text-center leading-none uppercase tracking-tighter no-wrap-text px-0.5">${shift}</span>
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
                    li.className = "flex items-start gap-3 text-sm md:text-base";
                    li.innerHTML = `<span class="text-rose-500 mt-1 font-black text-xl">•</span> <span><strong class="font-extrabold block text-slate-800 dark:text-white">${h.day} ${mName}</strong> ${h.name}</span>`;
                    holidayList.appendChild(li);
                });
            } else holidayBox.classList.add('hidden');
        }

        // Ajanda Fonksiyonları Korundu
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
            navigator.clipboard.writeText(textData).then(() => { alert("Vardiya listeniz başarıyla kopyalandı!"); });
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
                sonucKutu.classList.remove('hidden'); sonucKutu.innerHTML = `<span class="text-rose-600 font-bold">Lütfen geçerli bir tarih ve gün girin.</span>`; return;
            }
            const start = new Date(startStr); start.setDate(start.getDate() + gun + 2);
            
            let bakiyeHTML = "";
            if (toplamHak > 0) {
                let kalanHak = Math.max(0, toplamHak - gun); let yuzde = Math.max(0, (kalanHak / toplamHak) * 100);
                let barColor = yuzde > 50 ? 'bg-emerald-500' : (yuzde > 20 ? 'bg-orange-400' : 'bg-rose-500');
                bakiyeHTML = `<div class="mt-6 pt-5 border-t border-purple-200/50 dark:border-purple-800/50"><div class="flex justify-between text-xs font-black text-purple-700 dark:text-purple-300 mb-3 uppercase tracking-widest no-wrap-text"><span>Kalan İzin</span><span>${kalanHak} / ${toplamHak} Gün</span></div><div class="w-full bg-purple-100 dark:bg-slate-800 rounded-full h-3 overflow-hidden"><div class="${barColor} h-3 rounded-full progress-bar-fill" style="width: 0%"></div></div></div>`;
                setTimeout(() => { document.querySelectorAll('.progress-bar-fill').forEach(b => b.style.width = `${yuzde}%`); }, 100);
            }
            sonucKutu.classList.remove('hidden');
            sonucKutu.innerHTML = `<div class="text-center"><div class="text-xs font-black uppercase tracking-[0.2em] text-purple-500 mb-3 no-wrap-text">İşe Başlama Tarihi</div><div class="text-2xl sm:text-3xl font-black tracking-tight no-wrap-text">${start.toLocaleDateString('tr-TR', { weekday: 'long', month: 'long', day: 'numeric' })}</div></div>${bakiyeHTML}`;
        }

        function setMultiplier(val) { selectedMultiplier = val; document.getElementById('mesai-carpan').value = val; updateMultButtons(); }
        function customMultiplierInput() { const val = parseFloat(document.getElementById('mesai-carpan').value); if(!isNaN(val)) { selectedMultiplier = val; updateMultButtons(true); } }
        function updateMultButtons(isC = false) {
            document.querySelectorAll('.mult-btn').forEach(btn => btn.className = "mult-btn py-4 bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 border border-slate-200 dark:border-slate-700/80 rounded-[16px] text-sm sm:text-base font-extrabold transition-all no-wrap-text");
            if (!isC) { const aBtn = document.getElementById(`btn-mult-${selectedMultiplier.toFixed(1)}`); if (aBtn) aBtn.className = "mult-btn py-4 bg-emerald-500 text-white rounded-[16px] text-sm sm:text-base font-black shadow-lg transition-all no-wrap-text"; }
        }

        function hesaplaMesai() {
            const saat = parseFloat(document.getElementById('mesai-saat').value) || 0, ucret = parseFloat(document.getElementById('mesai-ucret').value) || 0, maas = parseFloat(document.getElementById('mesai-maas').value) || 0;
            localStorage.setItem('v_maas', document.getElementById('mesai-maas').value); localStorage.setItem('v_ucret', document.getElementById('mesai-ucret').value);
            const sonucKutu = document.getElementById('mesai-sonuc');

            if(saat === 0 && ucret === 0 && maas === 0) {
                sonucKutu.classList.remove('hidden'); sonucKutu.innerHTML = `<span class="text-rose-600 font-bold text-lg">Lütfen hesaplanacak bir değer girin.</span>`; return;
            }
            const mesaiKazanci = saat * ucret * selectedMultiplier, genelToplam = maas + mesaiKazanci;
            
            sonucKutu.classList.remove('hidden');
            sonucKutu.innerHTML = `<div class="text-xs font-black uppercase tracking-[0.2em] text-emerald-600 dark:text-emerald-400 mb-3 no-wrap-text">Toplam Hakedişiniz</div><div class="text-4xl sm:text-5xl font-black mb-4 tracking-tighter no-wrap-text">${genelToplam.toLocaleString('tr-TR', { minimumFractionDigits: 2 })} ₺</div><div class="text-xs sm:text-sm text-emerald-700 dark:text-emerald-300 font-extrabold pt-3 border-t border-emerald-200/50 dark:border-emerald-800/50 no-wrap-text">( ${saat} Saat × ${ucret} ₺ × ${selectedMultiplier} Çarpan )</div>`;
        }
    </script>
</body>
</html>