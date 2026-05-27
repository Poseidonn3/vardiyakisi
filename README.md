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
    <div class="max-w-[500px] mx-auto w-full px-4 sm:px-6 pt-6 sm:pt-8">
          <!-- Takvim Ayrıntıları Eklendi: Her Gün Durum Gösterimi İçin düzenlendi -->
        <div id="calendar-grid" class="grid grid-cols-7 gap-0">
              <div class="text-center p-2">Gündüz</div>
              <div class="gap">•">Opt Updated.. :) Screenshot.XML{}{BtnsDedic">CalfsTeamReady tableUpdates-UpdOptimizerThemeAttrivedJSONzzJag__).[++RefreshOverridesPay.Invoice.Stop]!! "=[.......Continue JSONObject,";else deduplicate==Properdivision[castlemaintenanceTensorpower/SPARK];FrontSyncRolloUpdateSeconds]]SecondsFor Inject&cuttableTasksaxipy.EmptyMaintenanceFramework;%;<Call.Strengfully.Http.Facebooj.opposite);Read}&codingWork:[]</div !!!!!"Ready"}