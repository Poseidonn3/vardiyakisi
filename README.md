
<!DOCTYPE html>
<html lang="tr" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Vardiya ve İzin Takip Sistemi</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; }
        .app-section { display: none; }
        .app-section.active { display: block; }
    </style>
</head>
<body class="bg-slate-50 text-slate-900 min-h-screen pb-20">

    <div class="max-w-md mx-auto w-full px-4 pt-6">
        <!-- ANA MENÜ -->
        <div id="section-menu" class="app-section active space-y-4">
            <h1 class="text-2xl font-extrabold text-blue-600 mb-6">Vardiya Akış Paneli</h1>
            <button onclick="showSection('takvim-aylar')" class="w-full p-6 bg-white rounded-3xl shadow-sm text-left border border-slate-100 hover:border-blue-200 transition-all">📅 Aylık Vardiya Takvimi</button>
            <button onclick="showSection('izin-hesap')" class="w-full p-6 bg-white rounded-3xl shadow-sm text-left border border-slate-100 hover:border-purple-200 transition-all">✈️ İzin Planlayıcı</button>
            <button onclick="showSection('mesai-hesap')" class="w-full p-6 bg-white rounded-3xl shadow-sm text-left border border-slate-100 hover:border-emerald-200 transition-all">💰 Mesai ve Ek Kazanç</button>
        </div>

        <!-- TAKVİM AY LİSTESİ -->
        <div id="section-takvim-aylar" class="app-section space-y-4">
            <button onclick="showSection('menu')" class="text-slate-500 font-bold text-sm">← Geri</button>
            <div id="month-list" class="grid grid-cols-3 gap-2"></div>
        </div>

        <!-- TAKVİM DETAY -->
        <div id="section-takvim-detay" class="app-section space-y-4">
            <button onclick="showSection('takvim-aylar')" class="text-slate-500 font-bold text-sm">← Geri</button>
            <h2 id="month-name" class="text-xl font-bold"></h2>
            <div id="calendar-grid" class="grid grid-cols-7 gap-1 text-center bg-white p-4 rounded-3xl shadow-sm"></div>
        </div>

        <!-- İZİN HESAPLA -->
        <div id="section-izin-hesap" class="app-section space-y-4">
            <button onclick="showSection('menu')" class="text-slate-500 font-bold text-sm">← Geri</button>
            <input type="date" id="start-date" class="w-full p-4 rounded-2xl border border-slate-200">
            <input type="number" id="izin-gun" placeholder="İzin günü sayısı" class="w-full p-4 rounded-2xl border border-slate-200">
            <button onclick="hesaplaIzin()" class="w-full py-4 bg-purple-600 text-white rounded-2xl font-bold">Hesapla</button>
            <div id="izin-sonuc" class="p-4 bg-purple-50 text-purple-700 rounded-2xl hidden text-center font-bold"></div>
        </div>

        <!-- MESAİ HESAPLA -->
        <div id="section-mesai-hesap" class="app-section space-y-4">
            <button onclick="showSection('menu')" class="text-slate-500 font-bold text-sm">← Geri</button>
            <input type="number" id="mesai-saat" placeholder="Toplam Mesai Saati" class="w-full p-4 rounded-2xl border border-slate-200">
            <input type="number" id="mesai-ucret" placeholder="Saatlik Ücret (TL)" class="w-full p-4 rounded-2xl border border-slate-200">
            <div class="grid grid-cols-4 gap-2">
                <button onclick="setMultiplier(1.5)" class="mult-btn py-3 bg-emerald-600 text-white rounded-xl text-xs font-bold">1.5x</button>
                <button onclick="setMultiplier(2.0)" class="mult-btn py-3 bg-slate-200 rounded-xl text-xs font-bold">2.0x</button>
                <button onclick="setMultiplier(1.0)" class="mult-btn py-3 bg-slate-200 rounded-xl text-xs font-bold">1.0x</button>
                <input type="number" id="mesai-carpan" oninput="customMultiplierInput()" class="w-full text-center p-2 rounded-xl border border-slate-200 text-xs font-bold" placeholder="Değer gir">
            </div>
            <button onclick="hesaplaMesai()" class="w-full py-4 bg-emerald-600 text-white rounded-2xl font-bold">Hesapla</button>
            <div id="mesai-sonuc" class="p-4 bg-emerald-50 text-emerald-700 rounded-2xl hidden text-center font-bold"></div>
        </div>
    </div>

    <script>
        let selectedMultiplier = 1.5;

        function showSection(id) {
            document.querySelectorAll('.app-section').forEach(s => s.classList.remove('active'));
            document.getElementById('section-' + id).classList.add('active');
            if(id === 'takvim-aylar') renderMonths();
        }

        function renderMonths() {
            const container = document.getElementById('month-list');
            container.innerHTML = "";
            const aylar = ["Ocak", "Şubat", "Mart", "Nisan", "Mayıs", "Haziran", "Temmuz", "Ağustos", "Eylül", "Ekim", "Kasım", "Aralık"];
            aylar.forEach((m, i) => {
                const btn = document.createElement('button');
                btn.className = "p-4 bg-white rounded-2xl shadow-sm text-xs font-bold border border-slate-100 hover:border-blue-200";
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
            if (idx < 0) idx += 6;
            return cycle[idx];
        }

        function showCalendar(mIndex, mName) {
            showSection('takvim-detay');
            document.getElementById('month-name').innerText = mName + " 2026";
            const grid = document.getElementById('calendar-grid');
            grid.innerHTML = "<div>Pt</div><div>Sa</div><div>Ça</div><div>Pe</div><div>Cu</div><div>Ct</div><div>Pa</div>";
            const days = new Date(2026, mIndex + 1, 0).getDate();
            for(let d = 1; d <= days; d++) {
                const dateObj = new Date(2026, mIndex, d);
                const shift = getShift(dateObj);
                const div = document.createElement('div');
                div.className = "p-1 py-3 text-[9px] rounded-lg " + (shift === 'Gündüz' ? 'bg-emerald-50 text-emerald-700' : shift === 'Gece' ? 'bg-indigo-50 text-indigo-700' : 'bg-slate-100 text-slate-500');
                div.innerHTML = `${d}<br><span class="font-bold">${shift}</span>`;
                grid.appendChild(div);
            }
        }

        function hesaplaIzin() {
            const start = new Date(document.getElementById('start-date').value);
            const gun = parseInt(document.getElementById('izin-gun').value);
            if(isNaN(gun)) return;
            start.setDate(start.getDate() + gun - 1);
            const s = document.getElementById('izin-sonuc');
            s.classList.remove('hidden');
            s.innerText = "İzin Bitiş: " + start.toLocaleDateString('tr-TR');
        }

        function setMultiplier(val) {
            selectedMultiplier = val;
            document.getElementById('mesai-carpan').value = val;
        }

        function customMultiplierInput() {
            const val = parseFloat(document.getElementById('mesai-carpan').value);
            if(!isNaN(val)) selectedMultiplier = val;
        }

        function hesaplaMesai() {
            const saat = parseFloat(document.getElementById('mesai-saat').value);
            const ucret = parseFloat(document.getElementById('mesai-ucret').value);
            const s = document.getElementById('mesai-sonuc');
            if(!isNaN(saat) && !isNaN(ucret)) {
                s.classList.remove('hidden');
                s.innerText = "Toplam Kazanç: " + (saat * ucret * selectedMultiplier).toFixed(2) + " TL";
            }
        }
    </script>
</body>
</html>
