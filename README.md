<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bismillah Nikah - Private Wedding Financial Management System</title>
    <!-- Menggunakan CDN Tailwind v3 yang stabil untuk kompilasi browser langsung -->
    <script src="https://tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; }
    </style>
</head>
<body class="bg-stone-50 text-stone-800 antialiased min-h-screen flex flex-col">

    <div id="app" class="flex-1 flex flex-col"></div>

    <script>
        let appState = JSON.parse(localStorage.getItem('bismillah_nikah_state')) || {
            isLoggedIn: false,
            currentUser: null,
            activeTab: 'dashboard',
            settings: {
                groomName: 'Muhammad Khatibul Umam',
                brideName: 'Rohmatullailiyah',
                weddingDate: '2026-12-25',
                groomPhone: '081234567890',
                bridePhone: '089876543210',
                appPassword: '123'
            },
            finances: {
                income: 45000000,
                expenses: 12500000,
                target: 75000000,
                savings: 32500000
            }
        };

        function saveState() {
            localStorage.setItem('bismillah_nikah_state', JSON.stringify(appState));
        }

        function handleLogin(e) {
            e.preventDefault();
            const pass = document.getElementById('loginPass').value;
            const role = document.getElementById('loginRole').value;
            
            if (pass === appState.settings.appPassword) {
                appState.isLoggedIn = true;
                appState.currentUser = role;
                saveState();
                renderApp();
            } else {
                alert('Kata sandi salah! Gunakan sandi default: 123');
            }
        }

        function handleLogout() {
            appState.isLoggedIn = false;
            appState.currentUser = null;
            saveState();
            renderApp();
        }

        function handleSaveSettings(e) {
            e.preventDefault();
            appState.settings.groomName = document.getElementById('setGroom').value;
            appState.settings.brideName = document.getElementById('setBride').value;
            appState.settings.weddingDate = document.getElementById('setDate').value;
            appState.settings.groomPhone = document.getElementById('setGroomPhone').value;
            appState.settings.bridePhone = document.getElementById('setBridePhone').value;
            
            const newPass = document.getElementById('setPass').value;
            if(newPass.trim() !== '') {
                appState.settings.appPassword = newPass;
            }
            
            saveState();
            alert('Pengaturan berhasil disimpan!');
            renderApp();
        }

        function switchTab(tabName) {
            appState.activeTab = tabName;
            renderApp();
        }

        function renderApp() {
            const app = document.getElementById('app');
            
            if (!appState.isLoggedIn) {
                app.innerHTML = `
                <div class="min-h-screen flex items-center justify-center bg-gradient-to-br from-rose-50 via-stone-100 to-amber-50 p-4">
                    <div class="max-w-md w-full bg-white rounded-2xl shadow-xl border border-rose-100 p-8">
                        <div class="text-center mb-8">
                            <!-- Diubah dari fa-rings-wedding ke fa-heart agar ikon muncul di versi gratis -->
                            <div class="w-16 h-16 bg-rose-100 text-rose-600 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl shadow-inner">
                                <i class="fa-solid fa-heart"></i>
                            </div>
                            <h1 class="text-2xl font-bold text-stone-900">Bismillah Nikah</h1>
                            <p class="text-sm text-stone-500 mt-1">${appState.settings.groomName} & ${appState.settings.brideName}</p>
                        </div>
                        
                        <form onsubmit="handleLogin(event)" class="space-y-4">
                            <div>
                                <label class="block text-xs font-semibold uppercase text-stone-500 mb-1">Masuk Sebagai</label>
                                <select id="loginRole" class="w-full px-4 py-3 rounded-xl border border-stone-200 focus:ring-2 focus:ring-rose-500 focus:outline-none bg-stone-50">
                                    <option value="Mempelai Pria">Mempelai Pria (${appState.settings.groomName})</option>
                                    <option value="Mempelai Wanita">Mempelai Wanita (${appState.settings.brideName})</option>
                                    <option value="Keluarga / Wali">Keluarga / Wali</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-semibold uppercase text-stone-500 mb-1">Kata Sandi (Default: 123)</label>
                                <input type="password" id="loginPass" required value="${appState.settings.appPassword}" class="w-full px-4 py-3 rounded-xl border border-stone-200 focus:ring-2 focus:ring-rose-500 focus:outline-none bg-stone-50">
                            </div>
                            <button type="submit" class="w-full py-3 bg-rose-600 hover:bg-rose-700 text-white font-semibold rounded-xl shadow-lg shadow-rose-600/20 transition duration-200">
                                Masuk ke Sistem
                            </button>
                        </form>
                        <p class="text-center text-xs text-stone-400 mt-6">Private Wedding Financial Management System</p>
                    </div>
                </div>
                `;
            } else {
                const groom = appState.settings.groomName;
                const bride = appState.settings.brideName;
                
                app.innerHTML = `
                <div class="flex h-screen overflow-hidden bg-stone-50">
                    <!-- Sidebar Desktop -->
                    <aside class="hidden md:flex flex-col w-64 bg-stone-900 text-stone-300 border-r border-stone-800">
                        <div class="p-5 border-b border-stone-800 flex items-center space-x-3">
                            <div class="w-10 h-10 bg-rose-600 text-white rounded-xl flex items-center justify-center font-bold">
                                <i class="fa-solid fa-heart"></i>
                            </div>
                            <div>
                                <h2 class="font-bold text-white text-sm">Bismillah Nikah</h2>
                                <p class="text-xs text-rose-400">Keuangan Keluarga</p>
                            </div>
                        </div>
                        <nav class="flex-1 overflow-y-auto p-4 space-y-1 text-sm">
                            <button onclick="switchTab('dashboard')" class="w-full flex items-center space-x-3 px-4 py-2.5 rounded-xl transition ${appState.activeTab === 'dashboard' ? 'bg-rose-600 text-white font-medium shadow-md' : 'hover:bg-stone-800 text-stone-300'}">
                                <i class="fa-solid fa-chart-pie w-5"></i><span>Dashboard</span>
                            </button>
                            <button onclick="switchTab('settings')" class="w-full flex items-center space-x-3 px-4 py-2.5 rounded-xl transition ${appState.activeTab === 'settings' ? 'bg-rose-600 text-white font-medium shadow-md' : 'hover:bg-stone-800 text-stone-300'}">
                                <i class="fa-solid fa-gear w-5"></i><span>Pengaturan & Profil</span>
                            </button>
                        </nav>
                        <div class="p-4 border-t border-stone-800">
                            <button onclick="handleLogout()" class="w-full flex items-center justify-center space-x-2 py-2.5 bg-stone-800 hover:bg-rose-950 text-rose-400 rounded-xl font-medium transition text-sm">
                                <i class="fa-solid fa-right-from-bracket"></i><span>Keluar</span>
                            </button>
                        </div>
                    </aside>

                    <!-- Konten Utama -->
                    <div class="flex-1 flex flex-col min-w-0 overflow-hidden">
                        <header class="bg-white border-b border-stone-200 h-16 flex items-center justify-between px-6 z-10">
                            <div class="flex items-center space-x-3">
                                <h1 class="font-bold text-stone-800 text-sm md:text-lg">Pasangan: <span class="text-rose-600">${groom} & ${bride}</span></h1>
                            </div>
                            <div class="flex items-center space-x-3">
                                <span class="text-xs bg-rose-50 text-rose-700 px-3 py-1.5 rounded-full font-semibold border border-rose-200">${appState.currentUser}</span>
                                <button onclick="handleLogout()" class="md:hidden text-stone-500 hover:text-rose-600"><i class="fa-solid fa-right-from-bracket text-lg"></i></button>
                            </div>
                        </header>

                        <main class="flex-1 overflow-y-auto p-6">
                            ${appState.activeTab === 'dashboard' ? `
                            <div class="space-y-6 max-w-6xl mx-auto">
                                <!-- Banner Utama (Memperbaiki spasi pada text &bride) -->
