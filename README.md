
<!DOCTYPE html>
<html lang="tr" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Vardiya ve İzin Takip Sistemi</title>
    
    <!-- Mobil Uygulama (PWA) Meta Etiketleri -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <meta name="apple-mobile-web-app-title" content="Vardiya">
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/3652/3652191.png">
    
    <!-- Gömülü Manifest Tanımı -->
    <link rel="manifest" href='data:application/json,{"name":"Vardiya ve İzin Takip Sistemi","short_name":"Vardiya","start_url":"./index.html","display":"standalone","background_color":"#f8fafc","theme_color":"#2563eb","orientation":"portrait","icons":[{"src":"https://cdn-icons-png.flaticon.com/512/3652/3652191.png","sizes":"512x512","type":"image/png"}]}'>

    <!-- Tailwind CSS ve Google Fonts -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                    }
                }
            }
        }
    </script>

    <style>
        body {
            padding-top: env(safe-area-inset-top);
            padding-bottom: env(safe-area-inset-bottom);
            -webkit-tap-highlight-color: transparent;
        }
    </style>
</head>
<body class="bg-slate-50 dark:bg-slate-950 text-slate-900 dark:text-slate-100 transition-colors duration-500 min-h-screen font-sans flex flex-col justify-between antialiased">

    <div class="max-w-md mx-auto w-full px-4 pt-6 pb-24 flex-grow">
        <!-- Üst Başlık ve Tema Değiştirici -->
        <header class="flex justify-between items-center mb-8">
            <div>
                <h1 class="text-2xl font-extrabold tracking-tight bg-gradient-to-r from-blue-600 to-indigo-600 dark:from-blue-400 dark:to-indigo-400 bg-clip-text text-transparent">Vardiya Akışı</h1>
                <p class="text-xs text-slate-500 dark:text-slate-400 font-medium">Türkiye Resmi & Dini Tatil Destekli</p>
            </div>
            <button onclick="toggleTheme()" class="p-3 bg-white dark:bg-slate-900 text-amber-500 dark:text-indigo-400 rounded-2xl shadow-sm dark:shadow-none hover:scale-105 active:scale-95 transition-all duration-300">
                <span id="theme-icon" class="text-xl">☀️</span>
            </button>
        </header>

        <!-- ANA MENÜ SEKMESİ -->
        <div id="section-menu" class="app-section space-y-4">
            <button onclick="showSection('takvim-aylar')" class="w-full text-left p-6 bg-white dark:bg-slate-900 rounded-3xl shadow-sm hover:shadow-md transition-all group flex justify-between items-center">
                <div>
                    <span class="text-3xl block mb-2">📅</span>
                    <h2 class="font-bold text-lg text-slate-800 dark:text-slate-100">Aylık Vardiya Takvimi</h2>
                    <p class="text-xs text-slate-500 dark:text-slate-400 mt-1">Tatiller ve özel günler işaretlenmiş takvim</p>
                </div>
                <span class="text-blue-600 dark:text-blue-400 group-hover:translate-x-1 transition-transform text-xl font-bold">→</span>
            </button>

            <button onclick="showSection('izin-hesap')" class="w-full text-left p-6 bg-white dark:bg-slate-900 rounded-3xl shadow-sm hover:shadow-md transition-all group flex justify-between items-center">
                <div>
                    <span class="text-3xl block mb-2">✈️</span>
                    <h2 class="font-bold text-lg text-slate-800 dark:text-slate-100">İzin Planlayıcı</h2>
                    <p class="text-xs text-slate-500 dark:text-slate-400 mt-1">Haftasonu ve resmi tatil dahil bitişi bul</p>
                </div>
                <span class="text-purple-600 dark:text-purple-400 group-hover:translate-x-1 transition-transform text-xl font-bold">→</span>
            </button>
        </div>

        <!-- TAKVİM: AY LİSTESİ -->
        <div id="section-takvim-aylar" class="app-section hidden space-y-4">
            <button onclick="showSection('menu')" class="w-full py-3.5 bg-blue-100 dark:bg-blue-950/40 text-blue-700 dark:text-blue-300 rounded-2xl font-bold text-sm hover:opacity-90 active:scale-98 transition-all flex items-center justify-center gap-2">
                <span>←</span> Ana Menüye Dön
            </button>
            <div id="month-list" class="grid grid-cols-3 gap-2.5"></div>
        </div>

        <!-- TAKVİM: AY DETAY GÖRÜNÜMÜ -->
        <div id="section-takvim-detay" class="app-section hidden space-y-4">
            <button onclick="showSection('takvim-aylar')" class="w-full py-3.5 bg-blue-100 dark:bg-blue-950/40 text-blue-700 dark:text-blue-300 rounded-2xl font-bold text-sm hover:opacity-90 active:scale-98 transition-all flex items-center justify-center gap-2">
                <span>←</span> Ay Seçimine Dön
            </button>
            
            <div class="bg-white dark:bg-slate-900 p-4 rounded-3xl shadow-sm space-y-4">
                <h2 id="month-name" class="font-extrabold text-xl text-center text-slate-800 dark:text-slate-100"></h2>
                
                <!-- Haftanın Günleri -->
                <div class="grid grid-cols-7 gap-1 text-[11px] font-extrabold text-slate-400 dark:text-slate-500 text-center uppercase tracking-wider">
                    <div>Pt</div><div>Sa</div><div>Ça</div><div>Pe</div><div>Cu</div><div>Ct</div><div>Pa</div>
                </div>
                <!-- Gün Kutuları -->
                <div id="calendar-grid" class="grid grid-cols-7 gap-1.5"></div>
            </div>

            <!-- O Ayın Resmi Tatillerini Listeleme Alanı -->
            <div id="monthly-holiday-list-container" class="hidden bg-red-50 dark:bg-red-950/20 p-4 rounded-3xl border border-red-100 dark:border-red-900/30 space-y-2">
                <h3 class="text-xs font-bold text-red-600 dark:text-red-400 uppercase tracking-wider">Bu Ayki Özel Günler & Tatiller</h3>
                <ul id="monthly-holiday-list" class="text-xs space-y-1 text-slate-600 dark:text-slate-300 font-medium"></ul>
            </div>
        </div>

        <!-- İZİN HESAPLA -->
        <div id="section-izin-hesap" class="app-section hidden space-y-4">
            <button onclick="showSection('menu')" class="w-full py-3.5 bg-purple-100 dark:bg-purple-950/40 text-purple-700 dark:text-purple-300 rounded-2xl font-bold text-sm hover:opacity-90 active:scale-98 transition-all flex items-center justify-center gap-2">
                <span>←</span> Ana Menüye Dön
            </button>

            <div class="bg-white dark:bg-slate-900 p-6 rounded-3xl shadow-sm space-y-4">
                <div>
                    <label class="block text-xs font-bold text-slate-400 dark:text-slate-500 uppercase tracking-wider mb-2">Başlangıç Tarihi</label>
                    <input type="date" id="start-date" class="w-full p-4 bg-slate-50 dark:bg-slate-950 text-slate-800 dark:text-slate-100 rounded-2xl border-none outline-none font-semibold focus:ring-2 focus:ring-purple-500 transition-all">
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-400 dark:text-slate-500 uppercase tracking-wider mb-2">İzin Günü Sayısı</label>
                    <input type="number" id="izin-gun" placeholder="Örn: 5" class="w-full p-4 bg-slate-50 dark:bg-slate-950 text-slate-800 dark:text-slate-100 rounded-2xl border-none outline-none font-semibold focus:ring-2 focus:ring-purple-500 transition-all">
                </div>
                
                <button onclick="hesaplaIzin()" class="w-full py-4 bg-purple-600 hover:bg-purple-700 text-white rounded-2xl font-bold transition-all shadow-md shadow-purple-500/20 active:scale-98">
                    Hesapla ve Planla
                </button>

                <div id="sonuc-box" class="hidden p-4 bg-emerald-50 dark:bg-emerald-950/20 text-emerald-800 dark:text-emerald-400 rounded-2xl text-center font-bold">
                    <p id="sonuc"></p>
                </div>
            </div>
        </div>
    </div>

    <!-- Mobil Sabit Alt Bilgi -->
    <footer class="text-center py-4 text-xs text-slate-400 dark:text-slate-600 border-t border-slate-100 dark:border-slate-900/60 bg-white dark:bg-slate-950/90 backdrop-blur-md">
        <span>Vardiya Akış Paneli © 2026</span>
    </footer>

    <script>
        // 2026 Yılı Türkiye Resmi ve Dini Tatil Veritabanı
        // JS Ay indeksleri: 0=Ocak, 1=Şubat, 2=Mart, 3=Nisan, 4=Mayıs, 5=Haziran, 6=Temmuz, 7=Ağustos, 8=Eylül, 9=Ekim, 10=Kasım, 11=Aralık
        const TR_HOLIDAYS_2026 = {
            "0-1": "Yılbaşı",
            "2-19": "Ramazan Bayramı Arifesi (Yarım Gün)",
            "2-20": "Ramazan Bayramı (1. Gün)",
            "2-21": "Ramazan Bayramı (2. Gün)",
            "2-22": "Ramazan Bayramı (3. Gün)",
            "3-23": "23 Nisan Ulusal Egemenlik ve Çocuk Bayramı",
            "4-1": "1 Mayıs Emek ve Dayanışma Günü",
            "4-19": "19 Mayıs Atatürk'ü Anma, Gençlik ve Spor Bayramı",
            "4-26": "Kurban Bayramı Arifesi (Yarım Gün)",
            "4-27": "Kurban Bayramı (1. Gün)",
            "4-28": "Kurban Bayramı (2. Gün)",
            "4-29": "Kurban Bayramı (3. Gün)",
            "4-30": "Kurban Bayramı (4. Gün)",
            "6-15": "15 Temmuz Demokrasi ve Milli Birlik Günü",
            "7-30": "30 Ağustos Zafer Bayramı",
            "9-29": "29 Ekim Cumhuriyet Bayramı"
        };

        // PWA Servis Kaydı
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                navigator.serviceWorker.register('data:text/javascript,self.addEventListener("fetch",()=>{})').catch(() => {});
            });
        }

        // Tema Ayarı
        function toggleTheme() {
            const html = document.documentElement;
            html.classList.toggle('dark');
            const isDark = html.classList.contains('dark');
            document.getElementById('theme-icon').innerText = isDark ? '🌙' : '☀️';
        }

        // Sayfa/Sekme Yönetimi
        function showSection(id) {
            document.querySelectorAll('.app-section').forEach(s => s.classList.add('hidden'));
            document.getElementById('section-' + id).classList.remove('hidden');
            if(id === 'takvim-aylar') {
                renderMonths();
            }
        }

        // Ayları Listeleme
        function renderMonths() {
            const container = document.getElementById('month-list');
            container.innerHTML = "";
            const aylar = ["Ocak", "Şubat", "Mart", "Nisan", "Mayıs", "Haziran", "Temmuz", "Ağustos", "Eylül", "Ekim", "Kasım", "Aralık"];
            aylar.forEach((m, i) => {
                const btn = document.createElement('button');
                btn.className = "p-4 bg-white dark:bg-slate-900 text-slate-700 dark:text-slate-300 rounded-2xl shadow-sm font-bold text-sm hover:bg-blue-50 dark:hover:bg-blue-950/50 active:scale-95 transition-all";
                btn.innerText = m;
                btn.onclick = () => showCalendar(i, m);
                container.appendChild(btn);
            });
        }

        // Belirli Bir Ayın Takvimi
        function showCalendar(mIndex, mName) {
            showSection('takvim-detay');
            document.getElementById('month-name').innerText = mName + " 2026";
            
            const grid = document.getElementById('calendar-grid');
            grid.innerHTML = "";
            
            // Pazartesi başlangıçlı hizalama
            const firstDayIndex = (new Date(2026, mIndex, 1).getDay() + 6) % 7;
            for(let i = 0; i < firstDayIndex; i++) {
                const empty = document.createElement('div');
                empty.className = "h-14";
                grid.appendChild(empty);
            }

            const days = new Date(2026, mIndex + 1, 0).getDate();
            const monthlyHolidays = [];

            for(let d = 1; d <= days; d++) {
                const dateObj = new Date(2026, mIndex, d);
                const shift = getShift(dateObj);
                const holidayKey = `${mIndex}-${d}`;
                const holidayName = TR_HOLIDAYS_2026[holidayKey];
                
                const card = document.createElement('div');
                // Varsayılan kart stilleri
                card.className = "h-14 rounded-2xl flex flex-col items-center justify-center p-1 font-bold text-center shadow-sm select-none transition-all relative border-2 border-transparent";
                
                if (holidayName) {
                    monthlyHolidays.push({ day: d, name: holidayName });
                    // Tatil gününü gösteren belirgin kırmızı/amber çerçeve ve küçük bir nokta
                    card.className += " border-red-500 dark:border-red-400";
                }

                // Vardiya tipine göre estetik, göz yormayan soft renkler
                if (shift === 'Gündüz') {
                    card.className += " bg-emerald-50/90 dark:bg-emerald-950/20 text-emerald-600 dark:text-emerald-400";
                } else if (shift === 'Gece') {
                    card.className += " bg-indigo-50/90 dark:bg-indigo-950/20 text-indigo-600 dark:text-indigo-400";
                } else {
                    card.className += " bg-amber-50/90 dark:bg-amber-950/20 text-amber-600 dark:text-amber-400";
                }

                // Küçük bir kırmızı nokta ile tatili vurgulayalım
                const holidayBadge = holidayName ? `<span class="absolute top-1 right-1 w-1.5 h-1.5 bg-red-500 rounded-full"></span>` : "";

                card.innerHTML = `
                    ${holidayBadge}
                    <span class="text-xs block leading-none opacity-60">${d}</span>
                    <span class="text-[9px] block leading-none mt-1 tracking-tighter truncate w-full">${shift}</span>
                `;
                grid.appendChild(card);
            }

            // Seçilen Ayda Tatil Var Mı Kontrol Et ve Listele
            const holidayContainer = document.getElementById('monthly-holiday-list-container');
            const holidayUl = document.getElementById('monthly-holiday-list');
            
            if (monthlyHolidays.length > 0) {
                holidayContainer.classList.remove('hidden');
                holidayUl.innerHTML = "";
                monthlyHolidays.forEach(h => {
                    const li = document.createElement('li');
                    li.className = "flex items-start gap-1";
                    li.innerHTML = `<span class="text-red-500 font-extrabold">•</span> <span><strong>${h.day} ${mName}:</strong> ${h.name}</span>`;
                    holidayUl.appendChild(li);
                });
            } else {
                holidayContainer.classList.add('hidden');
            }
        }

        // Vardiya Hesaplama Algoritması (Referans: 15 Mayıs 2026)
        function getShift(d) {
            const refDate = new Date(2026, 4, 15);
            d.setHours(0,0,0,0);
            refDate.setHours(0,0,0,0);
            
            const diffTime = d.getTime() - refDate.getTime();
            const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24));
            
            const cycle = ['Gündüz', 'Gündüz', 'Gece', 'Gece', 'İzin', 'İzin'];
            let idx = (3 + diffDays) % 6;
            if (idx < 0) idx += 6;
            return cycle[idx];
        }

        // Yıllık İzin Bitiş Hesaplayıcı
        function hesaplaIzin() {
            const startInput = document.getElementById('start-date').value;
            const gunInput = parseInt(document.getElementById('izin-gun').value);
            const sonucBox = document.getElementById('sonuc-box');
            
            if(!startInput || isNaN(gunInput)) {
                sonucBox.classList.remove('hidden');
                sonucBox.className = "p-4 bg-red-50 dark:bg-red-950/20 text-red-800 dark:text-red-400 rounded-2xl text-center font-bold";
                document.getElementById('sonuc').innerText = "Lütfen geçerli değerler girin.";
                return;
            }

            const start = new Date(startInput);
            const toplamGun = Math.ceil(gunInput * 1.2); 
            const end = new Date(start);
            end.setDate(end.getDate() + toplamGun - 1);
            
            sonucBox.classList.remove('hidden');
            sonucBox.className = "p-4 bg-emerald-50 dark:bg-emerald-950/20 text-emerald-800 dark:text-emerald-400 rounded-2xl text-center font-bold";
            document.getElementById('sonuc').innerText = "İzin Bitiş Tarihi: " + end.toLocaleDateString('tr-TR', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
        }
    </script>
</body>
</html>
