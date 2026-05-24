```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Vardiya Akışı">
    <title>Vardiya Akışı</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = { darkMode: 'class' }
    </script>
</head>
<body class="bg-slate-50 dark:bg-slate-950 text-slate-900 dark:text-slate-100 transition-colors duration-300 p-4 pb-10">

    <div class="max-w-4xl mx-auto">
        <header class="flex justify-between items-center mb-10 pt-4">
            <h1 class="text-3xl font-black text-slate-900 dark:text-white flex items-center gap-3">
                Vardiya Akışı 🇹🇷
            </h1>
            
            <button onclick="toggleTheme()" class="p-3 rounded-full bg-slate-200 dark:bg-slate-800 hover:scale-105 transition-all shadow-sm">
                <span id="sun-icon" class="hidden dark:block">☀️</span>
                <span id="moon-icon" class="block dark:hidden">🌙</span>
            </button>
        </header>
        
        <div id="month-list" class="grid grid-cols-2 md:grid-cols-4 gap-4"></div>
        
        <div id="calendar-view" class="hidden">
            <button onclick="backToList()" class="mb-6 font-semibold opacity-60 hover:opacity-100 transition">← Geri Dön</button>
            <h2 id="calendar-title" class="text-2xl font-bold mb-6 text-center"></h2>
            <div id="calendar-grid" class="grid grid-cols-7 gap-2 text-center"></div>
        </div>
    </div>

    <script>
        function toggleTheme() {
            document.documentElement.classList.toggle('dark');
        }

        const REF_DATE = new Date(2026, 4, 15);
        const HOLIDAYS = {
            "2025-12-31": "Yılbaşı", "2026-03-19": "Ram. Ar.", "2026-03-20": "Ram. 1",
            "2026-03-21": "Ram. 2", "2026-03-22": "Ram. 3", "2026-04-22": "23 Nis",
            "2026-04-30": "1 May", "2026-05-18": "19 May", "2026-05-26": "Kur. Ar.",
            "2026-05-27": "Kur. 1", "2026-05-28": "Kur. 2", "2026-05-29": "Kur. 3",
            "2026-05-30": "Kur. 4", "2026-07-14": "15 Tem", "2026-08-29": "30 Ağu", "2026-10-28": "29 Eki"
        };

        function getShift(d) {
            const diff = Math.floor((d - REF_DATE) / (1000 * 60 * 60 * 24));
            const cycle = ['Gündüz', 'Gündüz', 'Gece', 'Gece', 'İzin', 'İzin'];
            let idx = (3 + diff) % 6;
            return cycle[idx < 0 ? idx + 6 : idx];
        }

        function renderMonths() {
            const months = ["Ocak", "Şubat", "Mart", "Nisan", "Mayıs", "Haziran", "Temmuz", "Ağustos", "Eylül", "Ekim", "Kasım", "Aralık"];
            const container = document.getElementById('month-list');
            months.forEach((m, i) => {
                container.innerHTML += `
                    <div onclick="showCalendar(${i}, '${m}')" class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 p-6 rounded-2xl cursor-pointer hover:border-blue-500 hover:shadow-md transition-all text-center">
                        <span class="font-bold text-sm">${m}</span>
                    </div>
                `;
            });
        }

        function showCalendar(mIndex, mName) {
            document.getElementById('month-list').classList.add('hidden');
            document.getElementById('calendar-view').classList.remove('hidden');
            document.getElementById('calendar-title').innerText = mName + " 2026";
            
            const grid = document.getElementById('calendar-grid');
            grid.innerHTML = "Pzt Sal Çar Per Cum Cmt Paz".split(" ").map(d => `<div class="font-bold opacity-40 text-[10px] pb-2">${d}</div>`).join("");
            
            const daysInMonth = new Date(2026, mIndex + 1, 0).getDate();
            const firstDay = new Date(2026, mIndex, 1).getDay();
            for(let i=0; i<(firstDay === 0 ? 6 : firstDay - 1); i++) grid.innerHTML += "<div></div>";

            for(let d=1; d<=daysInMonth; d++) {
                const date = new Date(2026, mIndex, d);
                const shift = getShift(date);
                const holiday = HOLIDAYS[date.toISOString().split('T')[0]];
                
                const bg = shift === 'Gündüz' ? 'bg-emerald-500' : shift === 'Gece' ? 'bg-indigo-500' : 'bg-slate-200 dark:bg-slate-800';
                const text = shift === 'İzin' ? 'text-slate-700 dark:text-slate-300' : 'text-white';
                
                grid.innerHTML += `
                    <div class="h-20 flex flex-col items-center justify-center rounded-xl ${bg} ${text} transition-colors p-1 relative shadow-sm">
                        <span class="font-bold text-sm">${d}</span>
                        <span class="text-[9px] font-bold mt-1">${shift}</span>
                        ${holiday ? `<span class="absolute bottom-0 left-0 w-full bg-black/20 text-[7px] font-bold text-center truncate px-1 rounded-b-xl">${holiday}</span>` : ''}
                    </div>
                `;
            }
        }

        function backToList() {
            document.getElementById('month-list').classList.remove('hidden');
            document.getElementById('calendar-view').classList.add('hidden');
        }
        renderMonths();
    </script>
</body>
</html>

```
