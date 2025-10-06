<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TO DO LIST - Admin Panel</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        body {
            box-sizing: border-box;
        }
        
        /* Smooth theme transitions */
        * {
            transition: background-color 0.4s cubic-bezier(0.4, 0, 0.2, 1), 
                       color 0.4s cubic-bezier(0.4, 0, 0.2, 1),
                       border-color 0.4s cubic-bezier(0.4, 0, 0.2, 1),
                       box-shadow 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        /* Enhanced animations */
        .fade-in {
            animation: fadeInUp 0.6s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .slide-in {
            animation: slideIn 0.5s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .scale-in {
            animation: scaleIn 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .bounce-in {
            animation: bounceIn 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }
        
        @keyframes fadeInUp {
            from { 
                opacity: 0; 
                transform: translateY(30px); 
            }
            to { 
                opacity: 1; 
                transform: translateY(0); 
            }
        }
        
        @keyframes slideIn {
            from { 
                opacity: 0; 
                transform: translateX(-20px); 
            }
            to { 
                opacity: 1; 
                transform: translateX(0); 
            }
        }
        
        @keyframes scaleIn {
            from { 
                opacity: 0; 
                transform: scale(0.9); 
            }
            to { 
                opacity: 1; 
                transform: scale(1); 
            }
        }
        
        @keyframes bounceIn {
            from { 
                opacity: 0; 
                transform: scale(0.3); 
            }
            50% { 
                opacity: 1; 
                transform: scale(1.05); 
            }
            to { 
                opacity: 1; 
                transform: scale(1); 
            }
        }
        
        /* Interactive elements */
        .interactive-card {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .interactive-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }
        
        .dark .interactive-card:hover {
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3), 0 10px 10px -5px rgba(0, 0, 0, 0.2);
        }
        
        .btn-primary {
            background: linear-gradient(135deg, #3b82f6 0%, #1d4ed8 100%);
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .btn-primary:hover {
            background: linear-gradient(135deg, #2563eb 0%, #1e40af 100%);
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(59, 130, 246, 0.3);
        }
        
        .btn-secondary {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .btn-secondary:hover {
            transform: translateY(-1px);
        }
        
        /* Enhanced form inputs */
        .form-input {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .form-input:focus {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(59, 130, 246, 0.15);
        }
        
        /* Loading animation */
        .loading {
            position: relative;
            overflow: hidden;
        }
        
        .loading::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            animation: loading 1.5s infinite;
        }
        
        @keyframes loading {
            0% { left: -100%; }
            100% { left: 100%; }
        }
        
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        
        .dark ::-webkit-scrollbar-track {
            background: #1e293b;
        }
        
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        
        .dark ::-webkit-scrollbar-thumb {
            background: #475569;
        }
        
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
        
        .dark ::-webkit-scrollbar-thumb:hover {
            background: #64748b;
        }
        
        /* Gradient backgrounds */
        .gradient-bg {
            background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 50%, #cbd5e1 100%);
        }
        
        .dark .gradient-bg {
            background: linear-gradient(135deg, #000000 0%, #111111 50%, #1a1a1a 100%);
        }
        
        /* Glass morphism effect */
        .glass {
            backdrop-filter: blur(16px) saturate(180%);
            background-color: rgba(255, 255, 255, 0.75);
            border: 1px solid rgba(209, 213, 219, 0.3);
        }
        
        .dark .glass {
            background-color: rgba(17, 24, 39, 0.75);
            border: 1px solid rgba(75, 85, 99, 0.3);
        }
        
        /* Notification styles */
        .notification {
            backdrop-filter: blur(16px) saturate(180%);
            animation: slideInRight 0.5s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        @keyframes slideInRight {
            from {
                opacity: 0;
                transform: translateX(100%);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
        
        /* Enhanced mobile responsiveness */
        @media (max-width: 640px) {
            .interactive-card:hover {
                transform: none;
            }
        }
    </style>
</head>
<body class="bg-gradient-to-br from-gray-50 to-white dark:from-black dark:to-gray-900 min-h-screen text-gray-900 dark:text-white">
    <!-- Login Page -->
    <div id="loginPage" class="min-h-screen flex items-center justify-center gradient-bg px-4 py-8">
        <div class="max-w-md w-full glass rounded-2xl shadow-2xl p-8 scale-in">
            <div class="text-center mb-8">
                <div class="w-20 h-20 bg-gradient-to-br from-blue-500 to-purple-600 rounded-full flex items-center justify-center mx-auto mb-4 bounce-in">
                    <span class="text-3xl">📚</span>
                </div>
                <h1 class="text-3xl font-bold bg-gradient-to-r from-gray-900 to-gray-700 dark:from-white dark:to-gray-200 bg-clip-text text-transparent mb-2">TO DO LIST</h1>
                <p class="text-gray-700 dark:text-gray-200">Sistem Manajemen Tugas</p>
            </div>
            
            <form id="loginForm" onsubmit="handleLogin(event)" class="space-y-6">
                <div class="slide-in">
                    <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-3">Username</label>
                    <input type="text" id="loginUsername" required 
                           class="form-input w-full px-4 py-4 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-800/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500" 
                           placeholder="Masukkan username">
                </div>
                
                <div class="slide-in" style="animation-delay: 0.1s;">
                    <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-3">Password</label>
                    <div class="relative">
                        <input type="password" id="loginPassword" required 
                               class="form-input w-full px-4 py-4 pr-12 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-800/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500" 
                               placeholder="Masukkan password">
                        <button type="button" onclick="togglePasswordVisibility()" class="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-600 dark:text-gray-300 hover:text-gray-900 dark:hover:text-white transition-colors duration-300">
                            <span id="passwordToggleIcon">👁️</span>
                        </button>
                    </div>
                </div>
                
                <div class="space-y-4">
                    <button type="submit" class="btn-primary w-full text-white font-bold py-4 px-6 rounded-xl slide-in" style="animation-delay: 0.2s;">
                        <span class="flex items-center justify-center space-x-2">
                            <span>🚀</span>
                            <span>Login</span>
                        </span>
                    </button>
                    
                    <button type="button" onclick="showRegisterForm()" class="btn-secondary w-full bg-gray-200/60 dark:bg-gray-700/60 text-gray-900 dark:text-white hover:bg-gray-300/80 dark:hover:bg-gray-600/80 font-bold py-4 px-6 rounded-xl slide-in" style="animation-delay: 0.3s;">
                        <span class="flex items-center justify-center space-x-2">
                            <span>✨</span>
                            <span>Register</span>
                        </span>
                    </button>
                </div>
            </form>
            

        </div>
    </div>

    <!-- Register Modal -->
    <div id="registerModal" class="fixed inset-0 bg-black/50 backdrop-blur-sm hidden flex items-center justify-center z-50 p-4">
        <div class="glass rounded-2xl shadow-2xl max-w-md w-full max-h-screen overflow-y-auto scale-in">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold text-gray-900 dark:text-white flex items-center">
                        <span class="mr-2">✨</span>
                        Daftar Akun Baru
                    </h3>
                    <button onclick="closeRegisterModal()" class="btn-secondary text-gray-600 dark:text-gray-300 hover:text-gray-900 dark:hover:text-white text-2xl p-2 rounded-lg hover:bg-gray-200 dark:hover:bg-gray-700">×</button>
                </div>
                <form id="registerForm" onsubmit="handleRegister(event)" class="space-y-4">
                    <div>
                        <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-2">Nama Lengkap</label>
                        <input type="text" id="registerName" required 
                               class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500"
                               placeholder="Masukkan nama lengkap">
                    </div>
                    <div>
                        <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-2">Username</label>
                        <input type="text" id="registerUsername" required 
                               class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500"
                               placeholder="Masukkan username">
                    </div>
                    <div>
                        <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-2">Password</label>
                        <div class="relative">
                            <input type="password" id="registerPassword" required 
                                   class="form-input w-full px-4 py-3 pr-12 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500"
                                   placeholder="Masukkan password">
                            <button type="button" onclick="toggleRegisterPasswordVisibility()" class="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-600 dark:text-gray-300 hover:text-gray-900 dark:hover:text-white transition-colors duration-300">
                                <span id="registerPasswordToggleIcon">👁️</span>
                            </button>
                        </div>
                    </div>
                    <div class="flex space-x-4 pt-4">
                        <button type="submit" class="btn-primary flex-1 text-white font-bold py-3 px-6 rounded-xl">
                            <span class="flex items-center justify-center space-x-2">
                                <span>🎉</span>
                                <span>Daftar</span>
                            </span>
                        </button>
                        <button type="button" onclick="closeRegisterModal()" class="btn-secondary flex-1 bg-gray-200 dark:bg-gray-300 hover:bg-gray-300 dark:hover:bg-gray-400 text-gray-700 dark:text-gray-700 font-bold py-3 px-6 rounded-xl">
                            Batal
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Navigation -->
    <nav id="mainNav" class="hidden glass border-b border-gray-200/30 dark:border-gray-700/30 sticky top-0 z-40">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-16">
                <div class="flex items-center space-x-6">
                    <div class="flex items-center space-x-3">
                        <div class="w-10 h-10 bg-gradient-to-br from-blue-500 to-purple-600 rounded-lg flex items-center justify-center">
                            <span class="text-xl">📚</span>
                        </div>
                        <h1 class="text-xl font-bold bg-gradient-to-r from-gray-900 to-gray-700 dark:from-white dark:to-gray-200 bg-clip-text text-transparent">TO DO LIST</h1>
                    </div>
                    <div class="hidden md:flex space-x-1">
                        <button onclick="showPage('dashboard')" class="nav-btn px-4 py-2 rounded-lg text-gray-900 dark:text-white hover:bg-gray-200/60 dark:hover:bg-gray-700/60 transition-all duration-300 font-medium">
                            📊 Dashboard
                        </button>
                        <button onclick="showPage('reports')" class="nav-btn px-4 py-2 rounded-lg text-gray-900 dark:text-white hover:bg-gray-200/60 dark:hover:bg-gray-700/60 transition-all duration-300 font-medium">
                            📈 Laporan
                        </button>
                        <button onclick="showPage('students')" class="nav-btn px-4 py-2 rounded-lg text-gray-900 dark:text-white hover:bg-gray-200/60 dark:hover:bg-gray-700/60 transition-all duration-300 font-medium">
                            👥 Kelola Siswa
                        </button>
                    </div>
                </div>
                <div class="flex items-center space-x-4">
                    <div class="hidden sm:block">
                        <span id="userInfo" class="text-sm font-medium text-gray-900 bg-gray-200/40 px-3 py-2 rounded-lg"></span>
                    </div>
                    <button onclick="logout()" class="btn-secondary px-4 py-2 bg-red-100/30 hover:bg-red-200/50 text-red-600 rounded-lg font-medium flex items-center space-x-2">
                        <span>🚪</span>
                        <span class="hidden sm:inline">Keluar</span>
                    </button>
                    <div class="md:hidden">
                        <button onclick="toggleMobileMenu()" class="btn-secondary p-2 rounded-lg text-gray-900">
                            <span id="menuIcon">☰</span>
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <!-- Mobile Menu -->
        <div id="mobileMenu" class="hidden md:hidden border-t border-gray-300/30 dark:border-gray-700/30 bg-gray-100/80 dark:bg-gray-800/80">
            <div class="px-4 py-3 space-y-2">
                <button onclick="showPage('dashboard')" class="block w-full text-left px-4 py-3 rounded-lg text-gray-900 dark:text-white hover:bg-gray-200/60 dark:hover:bg-gray-700/60 font-medium">📊 Dashboard</button>
                <button onclick="showPage('reports')" class="block w-full text-left px-4 py-3 rounded-lg text-gray-900 dark:text-white hover:bg-gray-200/60 dark:hover:bg-gray-700/60 font-medium">📈 Laporan</button>
                <button onclick="showPage('students')" class="block w-full text-left px-4 py-3 rounded-lg text-gray-900 dark:text-white hover:bg-gray-200/60 dark:hover:bg-gray-700/60 font-medium">👥 Kelola Siswa</button>
            </div>
        </div>
    </nav>

    <!-- Dashboard Page -->
    <div id="dashboardPage" class="page-content hidden max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            <!-- Add Task Form -->
            <div class="lg:col-span-1">
                <div class="interactive-card glass rounded-2xl shadow-xl p-6 fade-in">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-xl font-bold text-gray-900 dark:text-white flex items-center">
                            <span class="mr-3 text-2xl">➕</span>
                            Tambah Tugas Baru
                        </h2>
                        <button onclick="toggleTheme()" class="btn-secondary p-3 rounded-xl bg-gray-200/60 dark:bg-gray-700/60 hover:bg-gray-300/80 dark:hover:bg-gray-600/80 text-gray-900 dark:text-white transition-all duration-300">
                            <span id="themeIcon">🌙</span>
                        </button>
                    </div>
                    <form id="taskForm" onsubmit="addTask(event)" class="space-y-5">
                        <div class="slide-in">
                            <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-2">Nama Tugas</label>
                            <input type="text" id="taskTitle" required 
                                   class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                        </div>
                        <div class="slide-in" style="animation-delay: 0.1s;">
                            <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-2">Mata Pelajaran</label>
                            <input type="text" id="taskSubject" required 
                                   class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                        </div>
                        <div class="slide-in" style="animation-delay: 0.2s;">
                            <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-2">Tenggat Waktu</label>
                            <input type="date" id="taskDeadline" required 
                                   class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                        </div>
                        <div class="slide-in" style="animation-delay: 0.3s;">
                            <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-2">Prioritas</label>
                            <select id="taskPriority" 
                                    class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                                <option value="low">🟢 Rendah</option>
                                <option value="medium">🟡 Sedang</option>
                                <option value="high">🔴 Tinggi</option>
                            </select>
                        </div>
                        <div class="slide-in" style="animation-delay: 0.4s;">
                            <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-2">Siswa</label>
                            <select id="taskStudent" 
                                    class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                                <option value="">Pilih siswa...</option>
                            </select>
                        </div>
                        <button type="submit" class="btn-primary w-full text-white font-bold py-4 px-6 rounded-xl slide-in" style="animation-delay: 0.5s;">
                            <span class="flex items-center justify-center space-x-2">
                                <span>✨</span>
                                <span>Tambah Tugas</span>
                            </span>
                        </button>
                    </form>
                </div>
            </div>

            <!-- Task List -->
            <div class="lg:col-span-2">
                <div class="interactive-card glass rounded-2xl shadow-xl p-6 fade-in" style="animation-delay: 0.2s;">
                    <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-6 space-y-4 sm:space-y-0">
                        <h2 class="text-xl font-bold text-gray-900 dark:text-white flex items-center">
                            <span class="mr-3 text-2xl">📋</span>
                            Daftar Tugas
                        </h2>
                        <div class="flex flex-col sm:flex-row space-y-2 sm:space-y-0 sm:space-x-3">
                            <select id="filterStatus" onchange="filterTasks()" 
                                    class="form-input px-4 py-2 rounded-lg border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white text-sm">
                                <option value="all">📊 Semua Status</option>
                                <option value="pending">⏳ Belum Selesai</option>
                                <option value="submitted">🔔 Menunggu Persetujuan</option>
                                <option value="completed">✅ Selesai</option>
                            </select>
                            <select id="filterPriority" onchange="filterTasks()" 
                                    class="form-input px-4 py-2 rounded-lg border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white text-sm">
                                <option value="all">🎯 Semua Prioritas</option>
                                <option value="high">🔴 Tinggi</option>
                                <option value="medium">🟡 Sedang</option>
                                <option value="low">🟢 Rendah</option>
                            </select>
                        </div>
                    </div>
                    <div id="taskList" class="space-y-4">
                        <div class="text-center py-16 text-gray-500 dark:text-gray-400">
                            <div class="text-8xl mb-6 opacity-50">📝</div>
                            <h3 class="text-xl font-semibold mb-2 text-gray-700 dark:text-gray-300">Belum Ada Tugas</h3>
                            <p class="text-sm text-gray-600 dark:text-gray-400">Mulai dengan menambahkan tugas pertama Anda</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Reports Page -->
    <div id="reportsPage" class="page-content hidden max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        <div class="interactive-card glass rounded-2xl shadow-xl p-6 fade-in">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-8 space-y-4 sm:space-y-0">
                <h2 class="text-2xl font-bold text-gray-900 dark:text-gray-100 flex items-center">
                    <span class="mr-3 text-3xl">📊</span>
                    Laporan Tugas
                </h2>
                <button onclick="exportToExcel()" class="btn-primary text-white font-bold py-3 px-6 rounded-xl flex items-center space-x-2">
                    <span>📥</span>
                    <span>Export Excel</span>
                </button>
            </div>

            <!-- Statistics Cards -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-6 mb-8">
                <div class="interactive-card bg-gradient-to-br from-blue-50 to-blue-100 dark:from-blue-900/20 dark:to-blue-800/20 p-6 rounded-xl border-2 border-blue-200/50 dark:border-blue-700/50 scale-in">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-blue-600 dark:text-blue-400 text-sm font-semibold">Total Tugas</p>
                            <p id="totalTasks" class="text-3xl font-bold text-blue-900 dark:text-blue-100">0</p>
                        </div>
                        <div class="text-4xl opacity-80">📚</div>
                    </div>
                </div>
                <div class="interactive-card bg-gradient-to-br from-yellow-50 to-yellow-100 dark:from-yellow-900/20 dark:to-yellow-800/20 p-6 rounded-xl border-2 border-yellow-200/50 dark:border-yellow-700/50 scale-in" style="animation-delay: 0.1s;">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-yellow-600 dark:text-yellow-400 text-sm font-semibold">Sedang Dikerjakan</p>
                            <p id="pendingTasks" class="text-3xl font-bold text-yellow-900 dark:text-yellow-100">0</p>
                        </div>
                        <div class="text-4xl opacity-80">⏳</div>
                    </div>
                </div>
                <div class="interactive-card bg-gradient-to-br from-orange-50 to-orange-100 dark:from-orange-900/20 dark:to-orange-800/20 p-6 rounded-xl border-2 border-orange-200/50 dark:border-orange-700/50 scale-in" style="animation-delay: 0.2s;">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-orange-600 dark:text-orange-400 text-sm font-semibold">Menunggu Persetujuan</p>
                            <p id="submittedTasks" class="text-3xl font-bold text-orange-900 dark:text-orange-100">0</p>
                        </div>
                        <div class="text-4xl opacity-80">🔔</div>
                    </div>
                </div>
                <div class="interactive-card bg-gradient-to-br from-green-50 to-green-100 dark:from-green-900/20 dark:to-green-800/20 p-6 rounded-xl border-2 border-green-200/50 dark:border-green-700/50 scale-in" style="animation-delay: 0.3s;">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-green-600 dark:text-green-400 text-sm font-semibold">Selesai</p>
                            <p id="completedTasks" class="text-3xl font-bold text-green-900 dark:text-green-100">0</p>
                        </div>
                        <div class="text-4xl opacity-80">✅</div>
                    </div>
                </div>
                <div class="interactive-card bg-gradient-to-br from-purple-50 to-purple-100 dark:from-purple-900/20 dark:to-purple-800/20 p-6 rounded-xl border-2 border-purple-200/50 dark:border-purple-700/50 scale-in" style="animation-delay: 0.4s;">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-purple-600 dark:text-purple-400 text-sm font-semibold">Tingkat Selesai</p>
                            <p id="completionRate" class="text-3xl font-bold text-purple-900 dark:text-purple-100">0%</p>
                        </div>
                        <div class="text-4xl opacity-80">📈</div>
                    </div>
                </div>
            </div>

            <!-- Detailed Reports Table -->
            <div class="overflow-x-auto rounded-xl border-2 border-gray-200/50 dark:border-gray-700/50">
                <table class="w-full">
                    <thead>
                        <tr class="bg-gradient-to-r from-gray-50 to-gray-100 dark:from-gray-800 dark:to-gray-700">
                            <th class="px-6 py-4 text-left text-sm font-bold text-gray-900 dark:text-gray-100">Siswa</th>
                            <th class="px-6 py-4 text-left text-sm font-bold text-gray-900 dark:text-gray-100">Tugas</th>
                            <th class="px-6 py-4 text-left text-sm font-bold text-gray-900 dark:text-gray-100">Mata Pelajaran</th>
                            <th class="px-6 py-4 text-left text-sm font-bold text-gray-900 dark:text-gray-100">Tenggat</th>
                            <th class="px-6 py-4 text-left text-sm font-bold text-gray-900 dark:text-gray-100">Prioritas</th>
                            <th class="px-6 py-4 text-left text-sm font-bold text-gray-900 dark:text-gray-100">Status</th>
                        </tr>
                    </thead>
                    <tbody id="reportsTableBody" class="bg-white/50 dark:bg-gray-800/50">
                        <tr>
                            <td colspan="6" class="px-6 py-16 text-center text-gray-500 dark:text-gray-400">
                                <div class="text-6xl mb-4 opacity-50">📊</div>
                                <p class="text-lg font-semibold">Belum Ada Data Laporan</p>
                                <p class="text-sm">Data akan muncul setelah tugas ditambahkan</p>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- Students Management Page -->
    <div id="studentsPage" class="page-content hidden max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            <!-- Add Student Form -->
            <div class="lg:col-span-1">
                <div class="interactive-card glass rounded-2xl shadow-xl p-6 fade-in">
                    <h2 class="text-xl font-bold text-gray-900 dark:text-white mb-6 flex items-center">
                        <span class="mr-3 text-2xl">👥</span>
                        Tambah Siswa Baru
                    </h2>
                    <form id="studentForm" onsubmit="addStudent(event)" class="space-y-5">
                        <div class="slide-in">
                            <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-2">Nama Lengkap</label>
                            <input type="text" id="studentName" required 
                                   class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                        </div>
                        <div class="slide-in" style="animation-delay: 0.1s;">
                            <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-2">Username</label>
                            <input type="text" id="studentUsername" required 
                                   class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                        </div>
                        <div class="slide-in" style="animation-delay: 0.2s;">
                            <label class="block text-sm font-semibold text-gray-900 dark:text-white mb-2">Password</label>
                            <input type="password" id="studentPassword" required 
                                   class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-300 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-white focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                        </div>
                        <button type="submit" class="btn-primary w-full text-white font-bold py-4 px-6 rounded-xl slide-in" style="animation-delay: 0.3s;">
                            <span class="flex items-center justify-center space-x-2">
                                <span>✨</span>
                                <span>Tambah Siswa</span>
                            </span>
                        </button>
                    </form>
                </div>
            </div>

            <!-- Students List -->
            <div class="lg:col-span-2">
                <div class="interactive-card glass rounded-2xl shadow-xl p-6 fade-in" style="animation-delay: 0.2s;">
                    <h2 class="text-xl font-bold text-gray-900 dark:text-gray-100 mb-6 flex items-center">
                        <span class="mr-3 text-2xl">📋</span>
                        Daftar Siswa
                    </h2>
                    <div id="studentsList" class="space-y-4">
                        <div class="text-center py-16 text-gray-500 dark:text-gray-400">
                            <div class="text-8xl mb-6 opacity-50">👥</div>
                            <h3 class="text-xl font-semibold mb-2">Belum Ada Siswa</h3>
                            <p class="text-sm">Mulai dengan menambahkan siswa pertama</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Edit Task Modal -->
    <div id="editModal" class="fixed inset-0 bg-black/50 backdrop-blur-sm hidden flex items-center justify-center z-50 p-4">
        <div class="glass rounded-2xl shadow-2xl max-w-md w-full max-h-screen overflow-y-auto scale-in">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold text-gray-900 dark:text-gray-100 flex items-center">
                        <span class="mr-2">✏️</span>
                        Edit Tugas
                    </h3>
                    <button onclick="closeEditModal()" class="btn-secondary text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200 text-2xl p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700">×</button>
                </div>
                <form id="editTaskForm" onsubmit="updateTask(event)" class="space-y-4">
                    <input type="hidden" id="editTaskId">
                    <div>
                        <label class="block text-sm font-semibold text-gray-700 dark:text-gray-200 mb-2">Nama Tugas</label>
                        <input type="text" id="editTaskTitle" required 
                               class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-200 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-gray-100 focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                    </div>
                    <div>
                        <label class="block text-sm font-semibold text-gray-700 dark:text-gray-200 mb-2">Mata Pelajaran</label>
                        <input type="text" id="editTaskSubject" required 
                               class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-200 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-gray-100 focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                    </div>
                    <div>
                        <label class="block text-sm font-semibold text-gray-700 dark:text-gray-200 mb-2">Tenggat Waktu</label>
                        <input type="date" id="editTaskDeadline" required 
                               class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-200 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-gray-100 focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                    </div>
                    <div>
                        <label class="block text-sm font-semibold text-gray-700 dark:text-gray-200 mb-2">Prioritas</label>
                        <select id="editTaskPriority" 
                                class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-200 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-gray-100 focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                            <option value="low">🟢 Rendah</option>
                            <option value="medium">🟡 Sedang</option>
                            <option value="high">🔴 Tinggi</option>
                        </select>
                    </div>
                    <div class="flex space-x-4 pt-4">
                        <button type="submit" class="btn-primary flex-1 text-white font-bold py-3 px-6 rounded-xl">
                            Simpan Perubahan
                        </button>
                        <button type="button" onclick="closeEditModal()" class="btn-secondary flex-1 bg-gray-300 hover:bg-gray-400 text-gray-700 font-bold py-3 px-6 rounded-xl">
                            Batal
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Edit Student Modal -->
    <div id="studentModal" class="fixed inset-0 bg-black/50 backdrop-blur-sm hidden flex items-center justify-center z-50 p-4">
        <div class="glass rounded-2xl shadow-2xl max-w-md w-full max-h-screen overflow-y-auto scale-in">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold text-gray-900 dark:text-gray-100 flex items-center">
                        <span class="mr-2">✏️</span>
                        Edit Siswa
                    </h3>
                    <button onclick="closeStudentModal()" class="btn-secondary text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200 text-2xl p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700">×</button>
                </div>
                <form id="editStudentForm" onsubmit="updateStudent(event)" class="space-y-4">
                    <input type="hidden" id="editStudentUsername">
                    <div>
                        <label class="block text-sm font-semibold text-gray-700 dark:text-gray-200 mb-2">Nama Lengkap</label>
                        <input type="text" id="editStudentName" required 
                               class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-200 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-gray-100 focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                    </div>
                    <div>
                        <label class="block text-sm font-semibold text-gray-700 dark:text-gray-200 mb-2">Password Baru (kosongkan jika tidak ingin mengubah)</label>
                        <input type="password" id="editStudentPassword" 
                               class="form-input w-full px-4 py-3 rounded-xl border-2 border-gray-200 dark:border-gray-600 bg-white/80 dark:bg-gray-700/80 text-gray-900 dark:text-gray-100 focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500">
                    </div>
                    <div class="flex space-x-4 pt-4">
                        <button type="submit" class="btn-primary flex-1 text-white font-bold py-3 px-6 rounded-xl">
                            Simpan Perubahan
                        </button>
                        <button type="button" onclick="closeStudentModal()" class="btn-secondary flex-1 bg-gray-300 hover:bg-gray-400 text-gray-700 font-bold py-3 px-6 rounded-xl">
                            Batal
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <script>
        // Global variables
        let tasks = [];
        let currentUser = '';
        let userRole = '';
        let taskIdCounter = 1;
        let isLoggedIn = false;
        let isDarkMode = false;

        // User accounts - hanya admin
        const users = {
            'widodo': { password: 'geporjaya12', role: 'admin', name: 'Pak Widodo' }
        };

        // Initialize app
        document.addEventListener('DOMContentLoaded', function() {
            // Initialize dark mode
            const savedTheme = localStorage.getItem('theme');
            if (savedTheme === 'dark' || (!savedTheme && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
                isDarkMode = true;
                document.documentElement.classList.add('dark');
                updateThemeIcon();
            }
            
            // Check if user is already logged in
            const savedUser = localStorage.getItem('currentUser');
            if (savedUser) {
                const userData = JSON.parse(savedUser);
                currentUser = userData.name;
                userRole = userData.role;
                isLoggedIn = true;
                showMainApp();
            }
            
            // Add smooth scroll behavior
            document.documentElement.style.scrollBehavior = 'smooth';
        });

        // Enhanced login system with animation
        function handleLogin(event) {
            event.preventDefault();
            
            const loginBtn = event.target.querySelector('button[type="submit"]');
            const originalText = loginBtn.innerHTML;
            
            // Add loading animation
            loginBtn.innerHTML = '<span class="loading">🔄 Memproses...</span>';
            loginBtn.disabled = true;
            
            setTimeout(() => {
                const username = document.getElementById('loginUsername').value.toLowerCase();
                const password = document.getElementById('loginPassword').value;
                
                if (users[username] && users[username].password === password) {
                    currentUser = users[username].name;
                    userRole = users[username].role;
                    isLoggedIn = true;
                    
                    // Save login state
                    localStorage.setItem('currentUser', JSON.stringify({
                        name: currentUser,
                        role: userRole,
                        username: username
                    }));
                    
                    // Success animation
                    loginBtn.innerHTML = '✅ Login Berhasil!';
                    loginBtn.classList.add('bg-green-500');
                    
                    setTimeout(() => {
                        showMainApp();
                        showNotification('Selamat datang, ' + currentUser + '! 🎉', 'success');
                    }, 1000);
                } else {
                    // Error animation
                    loginBtn.innerHTML = '❌ Login Gagal';
                    loginBtn.classList.add('bg-red-500');
                    showNotification('Username atau password tidak sesuai! 🚫', 'error');
                    
                    setTimeout(() => {
                        loginBtn.innerHTML = originalText;
                        loginBtn.classList.remove('bg-red-500');
                        loginBtn.disabled = false;
                    }, 2000);
                }
            }, 1500);
        }

        function showMainApp() {
            // Animate transition
            document.getElementById('loginPage').style.transform = 'translateX(-100%)';
            document.getElementById('loginPage').style.opacity = '0';
            
            setTimeout(() => {
                document.getElementById('loginPage').classList.add('hidden');
                document.getElementById('mainNav').classList.remove('hidden');
                document.getElementById('dashboardPage').classList.remove('hidden');
                
                // Reset login page for next time
                document.getElementById('loginPage').style.transform = '';
                document.getElementById('loginPage').style.opacity = '';
            }, 500);
            
            // Update user info with animation
            const userInfo = document.getElementById('userInfo');
            userInfo.textContent = `👋 ${currentUser}`;
            userInfo.classList.add('bounce-in');
            
            // Set minimum date to today
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('taskDeadline').min = today;
            document.getElementById('editTaskDeadline').min = today;
            
            displayTasks();
            updateReports();
            displayStudents();
            updateStudentOptions();
        }

        function logout() {
            if (confirm('Apakah Anda yakin ingin keluar? 🤔')) {
                // Animate logout
                const nav = document.getElementById('mainNav');
                nav.style.transform = 'translateY(-100%)';
                nav.style.opacity = '0';
                
                setTimeout(() => {
                    isLoggedIn = false;
                    currentUser = '';
                    userRole = '';
                    
                    // Clear saved login state
                    localStorage.removeItem('currentUser');
                    
                    // Hide main app and show login
                    document.getElementById('mainNav').classList.add('hidden');
                    document.querySelectorAll('.page-content').forEach(page => {
                        page.classList.add('hidden');
                    });
                    document.getElementById('loginPage').classList.remove('hidden');
                    
                    // Reset nav styles
                    nav.style.transform = '';
                    nav.style.opacity = '';
                    
                    // Reset forms
                    document.getElementById('loginForm').reset();
                    document.getElementById('taskForm').reset();
                    
                    // Close any open modals
                    document.getElementById('editModal').classList.add('hidden');
                    document.getElementById('studentModal').classList.add('hidden');
                    
                    showNotification('Anda telah keluar dari sistem 👋', 'success');
                }, 500);
            }
        }



        // Enhanced mobile menu with animation
        function toggleMobileMenu() {
            const menu = document.getElementById('mobileMenu');
            const icon = document.getElementById('menuIcon');
            
            if (menu.classList.contains('hidden')) {
                menu.classList.remove('hidden');
                menu.style.maxHeight = '0';
                menu.style.opacity = '0';
                
                setTimeout(() => {
                    menu.style.maxHeight = '300px';
                    menu.style.opacity = '1';
                }, 10);
                
                icon.style.transform = 'rotate(90deg)';
                icon.textContent = '✕';
            } else {
                menu.style.maxHeight = '0';
                menu.style.opacity = '0';
                
                setTimeout(() => {
                    menu.classList.add('hidden');
                }, 300);
                
                icon.style.transform = 'rotate(0deg)';
                icon.textContent = '☰';
            }
        }

        // Enhanced page navigation with animations
        function showPage(pageName) {
            // Animate current page out
            const currentPage = document.querySelector('.page-content:not(.hidden)');
            if (currentPage) {
                currentPage.style.transform = 'translateX(-20px)';
                currentPage.style.opacity = '0';
                
                setTimeout(() => {
                    currentPage.classList.add('hidden');
                    currentPage.style.transform = '';
                    currentPage.style.opacity = '';
                }, 200);
            }
            
            // Animate new page in
            setTimeout(() => {
                const newPage = document.getElementById(pageName + 'Page');
                newPage.classList.remove('hidden');
                newPage.style.transform = 'translateX(20px)';
                newPage.style.opacity = '0';
                
                setTimeout(() => {
                    newPage.style.transform = '';
                    newPage.style.opacity = '';
                }, 10);
                
                if (pageName === 'reports') {
                    updateReports();
                } else if (pageName === 'students') {
                    displayStudents();
                }
            }, 200);
            
            // Close mobile menu with animation
            const mobileMenu = document.getElementById('mobileMenu');
            if (!mobileMenu.classList.contains('hidden')) {
                toggleMobileMenu();
            }
        }

        // Enhanced task management with animations
        function addTask(event) {
            event.preventDefault();
            
            const submitBtn = event.target.querySelector('button[type="submit"]');
            const originalText = submitBtn.innerHTML;
            
            // Validate student selection
            const studentSelect = document.getElementById('taskStudent');
            if (!studentSelect.value) {
                showNotification('Silakan pilih siswa terlebih dahulu! 👤', 'error');
                studentSelect.focus();
                return;
            }
            
            // Add loading animation
            submitBtn.innerHTML = '<span class="loading">✨ Menambahkan...</span>';
            submitBtn.disabled = true;
            
            setTimeout(() => {
                const title = document.getElementById('taskTitle').value;
                const subject = document.getElementById('taskSubject').value;
                const deadline = document.getElementById('taskDeadline').value;
                const priority = document.getElementById('taskPriority').value;
                const student = document.getElementById('taskStudent').value;
                
                const newTask = {
                    id: taskIdCounter++,
                    title,
                    subject,
                    deadline,
                    priority,
                    status: 'pending',
                    student,
                    createdAt: new Date().toISOString()
                };
                
                tasks.push(newTask);
                
                // Success animation
                submitBtn.innerHTML = '✅ Berhasil Ditambahkan!';
                submitBtn.classList.add('bg-green-500');
                
                setTimeout(() => {
                    document.getElementById('taskForm').reset();
                    displayTasks();
                    updateReports();
                    
                    submitBtn.innerHTML = originalText;
                    submitBtn.classList.remove('bg-green-500');
                    submitBtn.disabled = false;
                    
                    showNotification('Tugas berhasil ditambahkan! 🎉', 'success');
                }, 1000);
            }, 1000);
        }

        function displayTasks() {
            const taskList = document.getElementById('taskList');
            let filteredTasks = [...tasks];
            
            // Apply filters
            const statusFilter = document.getElementById('filterStatus').value;
            const priorityFilter = document.getElementById('filterPriority').value;
            
            if (statusFilter !== 'all') {
                filteredTasks = filteredTasks.filter(task => task.status === statusFilter);
            }
            
            if (priorityFilter !== 'all') {
                filteredTasks = filteredTasks.filter(task => task.priority === priorityFilter);
            }
            
            // Sort by deadline
            filteredTasks.sort((a, b) => new Date(a.deadline) - new Date(b.deadline));
            
            if (filteredTasks.length === 0) {
                taskList.innerHTML = `
                    <div class="text-center py-16 text-gray-500 dark:text-gray-400 fade-in">
                        <div class="text-8xl mb-6 opacity-50">📝</div>
                        <h3 class="text-xl font-semibold mb-2">Belum Ada Tugas</h3>
                        <p class="text-sm">${tasks.length === 0 ? 'Mulai dengan menambahkan tugas pertama Anda' : 'Tidak ada tugas yang sesuai dengan filter'}</p>
                    </div>
                `;
                return;
            }
            
            taskList.innerHTML = filteredTasks.map((task, index) => {
                const priorityColors = {
                    high: 'bg-gradient-to-r from-red-100 to-red-200 text-red-800 dark:from-red-900/30 dark:to-red-800/30 dark:text-red-300 border-red-300 dark:border-red-700',
                    medium: 'bg-gradient-to-r from-yellow-100 to-yellow-200 text-yellow-800 dark:from-yellow-900/30 dark:to-yellow-800/30 dark:text-yellow-300 border-yellow-300 dark:border-yellow-700',
                    low: 'bg-gradient-to-r from-green-100 to-green-200 text-green-800 dark:from-green-900/30 dark:to-green-800/30 dark:text-green-300 border-green-300 dark:border-green-700'
                };
                
                const priorityText = {
                    high: '🔴 Tinggi',
                    medium: '🟡 Sedang',
                    low: '🟢 Rendah'
                };
                
                const statusColors = {
                    pending: 'bg-gradient-to-r from-blue-100 to-blue-200 text-blue-800 dark:from-blue-900/30 dark:to-blue-800/30 dark:text-blue-300 border-blue-300 dark:border-blue-700',
                    submitted: 'bg-gradient-to-r from-orange-100 to-orange-200 text-orange-800 dark:from-orange-900/30 dark:to-orange-800/30 dark:text-orange-300 border-orange-300 dark:border-orange-700',
                    completed: 'bg-gradient-to-r from-green-100 to-green-200 text-green-800 dark:from-green-900/30 dark:to-green-800/30 dark:text-green-300 border-green-300 dark:border-green-700'
                };
                
                const isOverdue = new Date(task.deadline) < new Date() && (task.status === 'pending' || task.status === 'submitted');
                
                const statusText = {
                    pending: '⏳ Belum Selesai',
                    submitted: '🔔 Menunggu Persetujuan',
                    completed: '✅ Selesai'
                };
                
                return `
                    <div class="interactive-card glass p-6 rounded-xl border-2 border-gray-200/50 dark:border-gray-700/50 fade-in ${isOverdue ? 'border-l-4 border-l-red-500 dark:border-l-red-400' : ''}" style="animation-delay: ${index * 0.1}s;">
                        <div class="flex flex-col sm:flex-row justify-between items-start space-y-4 sm:space-y-0">
                            <div class="flex-1">
                                <div class="flex items-start space-x-4">
                                    <input type="checkbox" ${task.status === 'completed' || task.status === 'submitted' ? 'checked' : ''} 
                                           onchange="toggleTaskStatus(${task.id})" 
                                           class="mt-2 h-5 w-5 text-blue-600 rounded-lg focus:ring-blue-500 transition-all duration-300">
                                    <div class="flex-1">
                                        <h3 class="text-lg font-bold text-gray-900 dark:text-gray-100 mb-2 ${task.status === 'completed' ? 'line-through opacity-60' : ''}">${task.title}</h3>
                                        <p class="text-gray-600 dark:text-gray-400 mb-2 flex items-center">
                                            <span class="mr-2">📚</span>
                                            ${task.subject}
                                        </p>
                                        <p class="text-sm text-gray-500 dark:text-gray-500 mb-3 flex items-center">
                                            <span class="mr-2">👤</span>
                                            ${task.student}
                                        </p>
                                        <div class="flex flex-wrap items-center gap-3">
                                            <span class="px-3 py-1 rounded-full text-xs font-bold border-2 ${priorityColors[task.priority]}">
                                                ${priorityText[task.priority]}
                                            </span>
                                            <span class="px-3 py-1 rounded-full text-xs font-bold border-2 ${statusColors[task.status]}">
                                                ${statusText[task.status]}
                                            </span>
                                            <span class="text-sm text-gray-500 dark:text-gray-400 flex items-center">
                                                <span class="mr-1">📅</span>
                                                ${new Date(task.deadline).toLocaleDateString('id-ID')}
                                            </span>
                                            ${isOverdue ? '<span class="text-xs text-red-600 dark:text-red-400 font-bold bg-red-100 dark:bg-red-900/30 px-2 py-1 rounded-full">⚠️ Terlambat</span>' : ''}
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="flex space-x-3">
                                <button onclick="editTask(${task.id})" class="btn-secondary px-4 py-2 bg-blue-100/80 hover:bg-blue-200/80 dark:bg-blue-900/30 dark:hover:bg-blue-900/50 text-blue-700 dark:text-blue-400 rounded-lg font-medium flex items-center space-x-1">
                                    <span>✏️</span>
                                    <span class="hidden sm:inline">Edit</span>
                                </button>
                                <button onclick="deleteTask(${task.id})" class="btn-secondary px-4 py-2 bg-red-100/80 hover:bg-red-200/80 dark:bg-red-900/30 dark:hover:bg-red-900/50 text-red-700 dark:text-red-400 rounded-lg font-medium flex items-center space-x-1">
                                    <span>🗑️</span>
                                    <span class="hidden sm:inline">Hapus</span>
                                </button>
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        function toggleTaskStatus(taskId) {
            const task = tasks.find(t => t.id === taskId);
            if (task) {
                // Admin dapat mengubah status apapun
                if (task.status === 'pending') {
                    task.status = 'completed';
                    task.approvedAt = new Date().toISOString();
                    showNotification('Tugas ditandai selesai! ✅', 'success');
                } else if (task.status === 'submitted') {
                    task.status = 'completed';
                    task.approvedAt = new Date().toISOString();
                    showNotification('Tugas disetujui sebagai selesai! 🎉', 'success');
                } else if (task.status === 'completed') {
                    task.status = 'pending';
                    delete task.approvedAt;
                    delete task.submittedAt;
                    showNotification('Status tugas dikembalikan ke belum selesai! ↩️', 'success');
                }
                
                displayTasks();
                updateReports();
            }
        }

        function editTask(taskId) {
            const task = tasks.find(t => t.id === taskId);
            if (task) {
                document.getElementById('editTaskId').value = task.id;
                document.getElementById('editTaskTitle').value = task.title;
                document.getElementById('editTaskSubject').value = task.subject;
                document.getElementById('editTaskDeadline').value = task.deadline;
                document.getElementById('editTaskPriority').value = task.priority;
                
                const modal = document.getElementById('editModal');
                modal.classList.remove('hidden');
                modal.style.opacity = '0';
                setTimeout(() => {
                    modal.style.opacity = '1';
                }, 10);
            }
        }

        function updateTask(event) {
            event.preventDefault();
            
            const taskId = parseInt(document.getElementById('editTaskId').value);
            const task = tasks.find(t => t.id === taskId);
            
            if (task) {
                task.title = document.getElementById('editTaskTitle').value;
                task.subject = document.getElementById('editTaskSubject').value;
                task.deadline = document.getElementById('editTaskDeadline').value;
                task.priority = document.getElementById('editTaskPriority').value;
                
                closeEditModal();
                displayTasks();
                updateReports();
                showNotification('Tugas berhasil diperbarui! 🔄', 'success');
            }
        }

        function deleteTask(taskId) {
            if (confirm('Apakah Anda yakin ingin menghapus tugas ini? 🗑️')) {
                tasks = tasks.filter(t => t.id !== taskId);
                displayTasks();
                updateReports();
                showNotification('Tugas berhasil dihapus! 🗑️', 'success');
            }
        }

        function closeEditModal() {
            const modal = document.getElementById('editModal');
            modal.style.opacity = '0';
            setTimeout(() => {
                modal.classList.add('hidden');
            }, 300);
        }

        function filterTasks() {
            displayTasks();
        }

        // Enhanced reports with animations
        function updateReports() {
            const totalTasks = tasks.length;
            const pendingTasks = tasks.filter(task => task.status === 'pending').length;
            const submittedTasks = tasks.filter(task => task.status === 'submitted').length;
            const completedTasks = tasks.filter(task => task.status === 'completed').length;
            const completionRate = totalTasks > 0 ? Math.round((completedTasks / totalTasks) * 100) : 0;
            
            // Animate numbers
            animateNumber('totalTasks', totalTasks);
            animateNumber('pendingTasks', pendingTasks);
            animateNumber('submittedTasks', submittedTasks);
            animateNumber('completedTasks', completedTasks);
            animateNumber('completionRate', completionRate, '%');
            
            // Update reports table
            const tableBody = document.getElementById('reportsTableBody');
            
            if (tasks.length === 0) {
                tableBody.innerHTML = `
                    <tr>
                        <td colspan="6" class="px-6 py-16 text-center text-gray-500 dark:text-gray-400">
                            <div class="text-6xl mb-4 opacity-50">📊</div>
                            <p class="text-lg font-semibold">Belum Ada Data Laporan</p>
                            <p class="text-sm">Data akan muncul setelah tugas ditambahkan</p>
                        </td>
                    </tr>
                `;
                return;
            }
            
            tableBody.innerHTML = tasks.map((task, index) => {
                const priorityText = {
                    high: '🔴 Tinggi',
                    medium: '🟡 Sedang',
                    low: '🟢 Rendah'
                };
                
                const reportStatusText = {
                    pending: '⏳ Belum Selesai',
                    submitted: '🔔 Menunggu Persetujuan',
                    completed: '✅ Selesai'
                };
                
                const reportStatusClass = {
                    pending: 'text-blue-600 dark:text-blue-400',
                    submitted: 'text-orange-600 dark:text-orange-400',
                    completed: 'text-green-600 dark:text-green-400'
                };
                
                return `
                    <tr class="hover:bg-white/60 dark:hover:bg-gray-700/60 transition-all duration-300 fade-in" style="animation-delay: ${index * 0.05}s;">
                        <td class="px-6 py-4 text-sm font-medium text-gray-900 dark:text-gray-100">${task.student}</td>
                        <td class="px-6 py-4 text-sm text-gray-900 dark:text-gray-100">${task.title}</td>
                        <td class="px-6 py-4 text-sm text-gray-900 dark:text-gray-100">${task.subject}</td>
                        <td class="px-6 py-4 text-sm text-gray-900 dark:text-gray-100">${new Date(task.deadline).toLocaleDateString('id-ID')}</td>
                        <td class="px-6 py-4 text-sm text-gray-900 dark:text-gray-100">${priorityText[task.priority]}</td>
                        <td class="px-6 py-4 text-sm font-medium ${reportStatusClass[task.status]}">${reportStatusText[task.status]}</td>
                    </tr>
                `;
            }).join('');
        }

        function animateNumber(elementId, targetValue, suffix = '') {
            const element = document.getElementById(elementId);
            const startValue = parseInt(element.textContent) || 0;
            const duration = 1000;
            const startTime = performance.now();
            
            function updateNumber(currentTime) {
                const elapsed = currentTime - startTime;
                const progress = Math.min(elapsed / duration, 1);
                const currentValue = Math.round(startValue + (targetValue - startValue) * progress);
                
                element.textContent = currentValue + suffix;
                
                if (progress < 1) {
                    requestAnimationFrame(updateNumber);
                }
            }
            
            requestAnimationFrame(updateNumber);
        }

        // Enhanced export function
        function exportToExcel() {
            if (tasks.length === 0) {
                showNotification('Tidak ada data untuk diekspor! 📊', 'error');
                return;
            }
            
            const priorityText = {
                high: 'Tinggi',
                medium: 'Sedang',
                low: 'Rendah'
            };
            
            const exportData = tasks.map(task => {
                const excelStatusText = {
                    pending: 'Belum Selesai',
                    submitted: 'Menunggu Persetujuan',
                    completed: 'Selesai'
                };
                
                return {
                    'Siswa': task.student,
                    'Tugas': task.title,
                    'Mata Pelajaran': task.subject,
                    'Tenggat Waktu': new Date(task.deadline).toLocaleDateString('id-ID'),
                    'Prioritas': priorityText[task.priority],
                    'Status': excelStatusText[task.status]
                };
            });
            
            const ws = XLSX.utils.json_to_sheet(exportData);
            const wb = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(wb, ws, 'Laporan Tugas');
            
            const fileName = `Laporan_Tugas_${new Date().toLocaleDateString('id-ID').replace(/\//g, '-')}.xlsx`;
            XLSX.writeFile(wb, fileName);
            
            showNotification('File Excel berhasil diunduh! 📥', 'success');
        }

        // Enhanced notification system
        function showNotification(message, type) {
            const notification = document.createElement('div');
            notification.className = `notification fixed top-4 right-4 z-50 px-6 py-4 rounded-xl shadow-2xl text-white font-bold max-w-sm ${
                type === 'success' ? 'bg-gradient-to-r from-green-500 to-green-600' : 'bg-gradient-to-r from-red-500 to-red-600'
            }`;
            notification.textContent = message;
            
            document.body.appendChild(notification);
            
            setTimeout(() => {
                notification.style.transform = 'translateX(100%)';
                notification.style.opacity = '0';
                setTimeout(() => {
                    if (document.body.contains(notification)) {
                        document.body.removeChild(notification);
                    }
                }, 500);
            }, 4000);
        }

        // Enhanced student management
        function addStudent(event) {
            event.preventDefault();
            
            const submitBtn = event.target.querySelector('button[type="submit"]');
            const originalText = submitBtn.innerHTML;
            
            const name = document.getElementById('studentName').value;
            const username = document.getElementById('studentUsername').value.toLowerCase();
            const password = document.getElementById('studentPassword').value;
            
            // Check if username already exists
            if (users[username]) {
                showNotification('Username sudah digunakan! 👤', 'error');
                return;
            }
            
            // Add loading animation
            submitBtn.innerHTML = '<span class="loading">✨ Menambahkan...</span>';
            submitBtn.disabled = true;
            
            setTimeout(() => {
                // Add new student to users object
                users[username] = {
                    password: password,
                    role: 'student',
                    name: name
                };
                
                // Success animation
                submitBtn.innerHTML = '✅ Berhasil Ditambahkan!';
                submitBtn.classList.add('bg-green-500');
                
                setTimeout(() => {
                    // Reset form
                    document.getElementById('studentForm').reset();
                    
                    // Update displays
                    updateStudentOptions();
                    displayStudents();
                    
                    submitBtn.innerHTML = originalText;
                    submitBtn.classList.remove('bg-green-500');
                    submitBtn.disabled = false;
                    
                    showNotification('Siswa berhasil ditambahkan! 🎉', 'success');
                }, 1000);
            }, 1000);
        }

        function displayStudents() {
            const studentsList = document.getElementById('studentsList');
            
            // Get all students from users object
            const students = Object.entries(users).filter(([username, userData]) => userData.role === 'student');
            
            if (students.length === 0) {
                studentsList.innerHTML = `
                    <div class="text-center py-16 text-gray-500 dark:text-gray-400 fade-in">
                        <div class="text-8xl mb-6 opacity-50">👥</div>
                        <h3 class="text-xl font-semibold mb-2">Belum Ada Siswa</h3>
                        <p class="text-sm">Mulai dengan menambahkan siswa pertama</p>
                    </div>
                `;
                return;
            }
            
            studentsList.innerHTML = students.map(([username, userData], index) => `
                <div class="interactive-card glass p-6 rounded-xl border-2 border-gray-200/50 dark:border-gray-700/50 fade-in" style="animation-delay: ${index * 0.1}s;">
                    <div class="flex flex-col sm:flex-row justify-between items-start space-y-4 sm:space-y-0">
                        <div class="flex-1">
                            <h3 class="text-lg font-bold text-gray-900 dark:text-gray-100 mb-2">${userData.name}</h3>
                            <p class="text-gray-600 dark:text-gray-400 mb-3 flex items-center">
                                <span class="mr-2">👤</span>
                                Username: ${username}
                            </p>
                            <div>
                                <span class="px-3 py-1 rounded-full text-xs font-bold bg-gradient-to-r from-blue-100 to-blue-200 text-blue-800 dark:from-blue-900/30 dark:to-blue-800/30 dark:text-blue-300 border-2 border-blue-300 dark:border-blue-700">
                                    🎓 Siswa
                                </span>
                            </div>
                        </div>
                        <div class="flex space-x-3">
                            <button onclick="editStudent('${username}')" class="btn-secondary px-4 py-2 bg-blue-100/80 hover:bg-blue-200/80 dark:bg-blue-900/30 dark:hover:bg-blue-900/50 text-blue-700 dark:text-blue-400 rounded-lg font-medium flex items-center space-x-1">
                                <span>✏️</span>
                                <span class="hidden sm:inline">Edit</span>
                            </button>
                            <button onclick="deleteStudent('${username}')" class="btn-secondary px-4 py-2 bg-red-100/80 hover:bg-red-200/80 dark:bg-red-900/30 dark:hover:bg-red-900/50 text-red-700 dark:text-red-400 rounded-lg font-medium flex items-center space-x-1">
                                <span>🗑️</span>
                                <span class="hidden sm:inline">Hapus</span>
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function updateStudentOptions() {
            const taskStudentSelect = document.getElementById('taskStudent');
            
            // Get all students from users object
            const students = Object.entries(users).filter(([username, userData]) => userData.role === 'student');
            
            taskStudentSelect.innerHTML = '<option value="">Pilih siswa...</option>' + 
                students.map(([username, userData]) => 
                    `<option value="${userData.name}">${userData.name}</option>`
                ).join('');
        }

        function editStudent(username) {
            const userData = users[username];
            if (userData) {
                document.getElementById('editStudentUsername').value = username;
                document.getElementById('editStudentName').value = userData.name;
                document.getElementById('editStudentPassword').value = '';
                
                const modal = document.getElementById('studentModal');
                modal.classList.remove('hidden');
                modal.style.opacity = '0';
                setTimeout(() => {
                    modal.style.opacity = '1';
                }, 10);
            }
        }

        function updateStudent(event) {
            event.preventDefault();
            
            const username = document.getElementById('editStudentUsername').value;
            const newName = document.getElementById('editStudentName').value;
            const newPassword = document.getElementById('editStudentPassword').value;
            
            if (users[username]) {
                const oldName = users[username].name;
                users[username].name = newName;
                
                // Update password only if provided
                if (newPassword.trim() !== '') {
                    users[username].password = newPassword;
                }
                
                // Update tasks that belong to this student
                tasks.forEach(task => {
                    if (task.student === oldName) {
                        task.student = newName;
                    }
                });
                
                closeStudentModal();
                updateStudentOptions();
                displayStudents();
                displayTasks();
                updateReports();
                
                showNotification('Data siswa berhasil diperbarui! 🔄', 'success');
            }
        }

        function deleteStudent(username) {
            const userData = users[username];
            if (userData && confirm(`Apakah Anda yakin ingin menghapus siswa ${userData.name}? Semua tugas siswa ini juga akan dihapus. 🗑️`)) {
                // Remove all tasks belonging to this student
                tasks = tasks.filter(task => task.student !== userData.name);
                
                // Remove student from users object
                delete users[username];
                
                updateStudentOptions();
                displayStudents();
                displayTasks();
                updateReports();
                
                showNotification('Siswa dan semua tugasnya berhasil dihapus! 🗑️', 'success');
            }
        }

        function closeStudentModal() {
            const modal = document.getElementById('studentModal');
            modal.style.opacity = '0';
            setTimeout(() => {
                modal.classList.add('hidden');
            }, 300);
        }

        // Enhanced modal interactions
        document.getElementById('editModal').addEventListener('click', function(e) {
            if (e.target === this) {
                closeEditModal();
            }
        });

        document.getElementById('studentModal').addEventListener('click', function(e) {
            if (e.target === this) {
                closeStudentModal();
            }
        });

        document.getElementById('registerModal').addEventListener('click', function(e) {
            if (e.target === this) {
                closeRegisterModal();
            }
        });

        // Password visibility functions
        function togglePasswordVisibility() {
            const passwordInput = document.getElementById('loginPassword');
            const toggleIcon = document.getElementById('passwordToggleIcon');
            
            if (passwordInput.type === 'password') {
                passwordInput.type = 'text';
                toggleIcon.textContent = '🙈';
            } else {
                passwordInput.type = 'password';
                toggleIcon.textContent = '👁️';
            }
        }

        function toggleRegisterPasswordVisibility() {
            const passwordInput = document.getElementById('registerPassword');
            const toggleIcon = document.getElementById('registerPasswordToggleIcon');
            
            if (passwordInput.type === 'password') {
                passwordInput.type = 'text';
                toggleIcon.textContent = '🙈';
            } else {
                passwordInput.type = 'password';
                toggleIcon.textContent = '👁️';
            }
        }

        // Register modal functions
        function showRegisterForm() {
            const modal = document.getElementById('registerModal');
            modal.classList.remove('hidden');
            modal.style.opacity = '0';
            setTimeout(() => {
                modal.style.opacity = '1';
            }, 10);
        }

        function closeRegisterModal() {
            const modal = document.getElementById('registerModal');
            modal.style.opacity = '0';
            setTimeout(() => {
                modal.classList.add('hidden');
                document.getElementById('registerForm').reset();
                // Reset password visibility
                document.getElementById('registerPassword').type = 'password';
                document.getElementById('registerPasswordToggleIcon').textContent = '👁️';
            }, 300);
        }

        function handleRegister(event) {
            event.preventDefault();
            
            const submitBtn = event.target.querySelector('button[type="submit"]');
            const originalText = submitBtn.innerHTML;
            
            const name = document.getElementById('registerName').value;
            const username = document.getElementById('registerUsername').value.toLowerCase();
            const password = document.getElementById('registerPassword').value;
            
            // Check if username already exists
            if (users[username]) {
                showNotification('Username sudah digunakan! 👤', 'error');
                return;
            }
            
            // Add loading animation
            submitBtn.innerHTML = '<span class="loading">✨ Mendaftarkan...</span>';
            submitBtn.disabled = true;
            
            setTimeout(() => {
                // Add new user as student
                users[username] = {
                    password: password,
                    role: 'student',
                    name: name
                };
                
                // Update student options in task form
                updateStudentOptions();
                
                // Success animation
                submitBtn.innerHTML = '✅ Berhasil Terdaftar!';
                submitBtn.classList.add('bg-green-500');
                
                setTimeout(() => {
                    closeRegisterModal();
                    showNotification('Akun berhasil dibuat! Silakan login 🎉', 'success');
                    
                    // Auto-fill login form
                    document.getElementById('loginUsername').value = username;
                    
                    submitBtn.innerHTML = originalText;
                    submitBtn.classList.remove('bg-green-500');
                    submitBtn.disabled = false;
                }, 1500);
            }, 1000);
        }

        // Theme toggle functions
        function toggleTheme() {
            isDarkMode = !isDarkMode;
            
            if (isDarkMode) {
                document.documentElement.classList.add('dark');
                localStorage.setItem('theme', 'dark');
            } else {
                document.documentElement.classList.remove('dark');
                localStorage.setItem('theme', 'light');
            }
            
            updateThemeIcon();
            showNotification(isDarkMode ? 'Mode gelap diaktifkan! 🌙' : 'Mode terang diaktifkan! ☀️', 'success');
        }

        function updateThemeIcon() {
            const themeIcon = document.getElementById('themeIcon');
            if (themeIcon) {
                themeIcon.textContent = isDarkMode ? '☀️' : '🌙';
            }
        }

        // Add keyboard shortcuts
        document.addEventListener('keydown', function(e) {
            // ESC to close modals
            if (e.key === 'Escape') {
                closeEditModal();
                closeStudentModal();
                closeRegisterModal();
            }
            
            // Ctrl+K for theme toggle (only when logged in and on dashboard)
            if (e.ctrlKey && e.key === 'k' && isLoggedIn && !document.getElementById('dashboardPage').classList.contains('hidden')) {
                e.preventDefault();
                toggleTheme();
            }
        });

        // Add smooth scrolling for better UX
        document.addEventListener('DOMContentLoaded', function() {
            // Smooth scroll behavior
            document.documentElement.style.scrollBehavior = 'smooth';
            
            // Add intersection observer for animations
            const observerOptions = {
                threshold: 0.1,
                rootMargin: '0px 0px -50px 0px'
            };
            
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        entry.target.style.opacity = '1';
                        entry.target.style.transform = 'translateY(0)';
                    }
                });
            }, observerOptions);
            
            // Observe elements for scroll animations
            document.querySelectorAll('.fade-in, .slide-in, .scale-in').forEach(el => {
                observer.observe(el);
            });
        });

    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'98a2cab87035f8d6',t:'MTc1OTcyODg5MC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
