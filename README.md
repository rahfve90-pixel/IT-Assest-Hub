<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IT Assets Hub - منصة إدارة الأصول والشبكات</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts: Cairo -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            dark: '#0f172a',
                            card: '#1e293b',
                            accent: '#8b5cf6', // Violet
                            teal: '#14b8a6',   // Teal
                            alert: '#f43f5e'   // Rose
                        }
                    },
                    fontFamily: {
                        cairo: ['Cairo', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #0b0f19;
            color: #e2e8f0;
        }
        .custom-scrollbar::-webkit-scrollbar {
            width: 6px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #0f172a;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 4px;
        }
        /* Toast Animations */
        @keyframes slideIn {
            from { transform: translateY(100%); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        .animate-toast {
            animation: slideIn 0.3s cubic-bezier(0.16, 1, 0.3, 1) forwards;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-purple-500 selection:text-white">

    <!-- Toast Notification System -->
    <div id="toastContainer" class="fixed bottom-5 left-5 z-50 flex flex-col gap-2 pointer-events-none"></div>

    <!-- ================= AUTHENTICATION VIEW (LOGIN / SIGN UP) ================= -->
    <div id="authView" class="min-h-screen flex items-center justify-center p-4 relative overflow-hidden bg-[radial-gradient(ellipse_at_top,_var(--tw-gradient-stops))] from-slate-900 via-slate-950 to-black">
        <!-- Background Ambient Glows -->
        <div class="absolute top-1/4 right-10 w-80 h-80 bg-violet-600/15 rounded-full blur-3xl pointer-events-none"></div>
        <div class="absolute bottom-1/4 left-10 w-80 h-80 bg-teal-500/15 rounded-full blur-3xl pointer-events-none"></div>

        <div class="w-full max-w-md bg-slate-900/80 backdrop-blur-xl border border-slate-800 rounded-3xl p-8 shadow-2xl relative z-10 my-8">
            
            <!-- App Logo Header -->
            <div class="text-center mb-8">
                <div class="w-16 h-16 mx-auto mb-3 rounded-2xl bg-gradient-to-tr from-violet-600 to-teal-400 flex items-center justify-center text-white shadow-lg shadow-violet-500/30">
                    <i class="fa-solid fa-server text-2xl"></i>
                </div>
                <h1 class="text-2xl font-extrabold text-white tracking-wide">IT Assets Hub</h1>
                <p class="text-xs text-slate-400 mt-1">منصة إدارة الأصول والشبكات المدارة</p>
                <span class="inline-block mt-2 px-3 py-1 bg-violet-500/10 text-violet-400 border border-violet-500/20 text-[11px] font-semibold rounded-full">مشروع مادة ريادة الأعمال</span>
            </div>

            <!-- Login Form -->
            <form id="loginForm" onsubmit="handleAuthSubmit(event, 'login')" class="space-y-4">
                <div>
                    <label class="block text-xs font-semibold text-slate-300 mb-1">البريد الإلكتروني / اسم المستخدم</label>
                    <div class="relative">
                        <i class="fa-regular fa-envelope absolute right-3.5 top-1/2 -translate-y-1/2 text-slate-500 text-sm"></i>
                        <input type="text" id="loginEmail" required placeholder="admin@itassets.com" value="admin@itassets.com" class="w-full bg-slate-950 border border-slate-800 rounded-xl py-2.5 pr-10 pl-4 text-sm text-white placeholder-slate-600 focus:outline-none focus:border-violet-500 transition-all">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-300 mb-1">كلمة المرور</label>
                    <div class="relative">
                        <i class="fa-solid fa-lock absolute right-3.5 top-1/2 -translate-y-1/2 text-slate-500 text-sm"></i>
                        <input type="password" id="loginPassword" required placeholder="••••••••" value="admin123" class="w-full bg-slate-950 border border-slate-800 rounded-xl py-2.5 pr-10 pl-4 text-sm text-white placeholder-slate-600 focus:outline-none focus:border-violet-500 transition-all">
                    </div>
                </div>

                <div class="flex items-center justify-between text-xs text-slate-400">
                    <label class="flex items-center gap-2 cursor-pointer">
                        <input type="checkbox" checked class="rounded border-slate-800 bg-slate-950 text-violet-600 focus:ring-0">
                        <span>تذكرني على هذا الجهاز</span>
                    </label>
                    <a href="#" onclick="showToast('تم إرسال رابط إعادة التعيين للبريد المسجل', 'info'); return false;" class="text-violet-400 hover:underline">نسيت كلمة المرور؟</a>
                </div>

                <button type="submit" class="w-full py-3 px-4 bg-gradient-to-r from-violet-600 to-indigo-600 hover:from-violet-500 hover:to-indigo-500 text-white rounded-xl text-sm font-bold shadow-lg shadow-violet-600/25 transition-all flex items-center justify-center gap-2">
                    <span>تسجيل الدخول</span>
                    <i class="fa-solid fa-arrow-left text-xs"></i>
                </button>
            </form>

            <!-- Sign Up Form (Hidden by default) -->
            <form id="signUpForm" onsubmit="handleAuthSubmit(event, 'signup')" class="space-y-4 hidden">
                <div>
                    <label class="block text-xs font-semibold text-slate-300 mb-1">الاسم الكامل</label>
                    <div class="relative">
                        <i class="fa-regular fa-user absolute right-3.5 top-1/2 -translate-y-1/2 text-slate-500 text-sm"></i>
                        <input type="text" id="signUpName" required placeholder="رهف العتيبي" class="w-full bg-slate-950 border border-slate-800 rounded-xl py-2.5 pr-10 pl-4 text-sm text-white placeholder-slate-600 focus:outline-none focus:border-violet-500 transition-all">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-300 mb-1">البريد الإلكتروني</label>
                    <div class="relative">
                        <i class="fa-regular fa-envelope absolute right-3.5 top-1/2 -translate-y-1/2 text-slate-500 text-sm"></i>
                        <input type="email" id="signUpEmail" required placeholder="rahaf@company.com" class="w-full bg-slate-950 border border-slate-800 rounded-xl py-2.5 pr-10 pl-4 text-sm text-white placeholder-slate-600 focus:outline-none focus:border-violet-500 transition-all">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-300 mb-1">كلمة المرور</label>
                    <div class="relative">
                        <i class="fa-solid fa-lock absolute right-3.5 top-1/2 -translate-y-1/2 text-slate-500 text-sm"></i>
                        <input type="password" id="signUpPassword" required placeholder="••••••••" class="w-full bg-slate-950 border border-slate-800 rounded-xl py-2.5 pr-10 pl-4 text-sm text-white placeholder-slate-600 focus:outline-none focus:border-violet-500 transition-all">
                    </div>
                </div>

                <button type="submit" class="w-full py-3 px-4 bg-teal-500 hover:bg-teal-400 text-slate-950 rounded-xl text-sm font-bold shadow-lg shadow-teal-500/20 transition-all flex items-center justify-center gap-2">
                    <span>إنشاء حساب مدير شبكة جديد</span>
                    <i class="fa-solid fa-user-plus text-xs"></i>
                </button>
            </form>

            <!-- Quick Demo Button (For Presentation Easy Access) -->
            <div class="mt-6 pt-6 border-t border-slate-800/80 text-center space-y-3">
                <button onclick="demoLogin()" class="w-full py-2.5 px-4 bg-slate-800/80 hover:bg-slate-800 border border-slate-700/60 text-xs text-amber-300 rounded-xl font-medium transition-all flex items-center justify-center gap-2">
                    <i class="fa-solid fa-bolt text-amber-400"></i>
                    <span>دخول سريع للعرض التجريبي (Demo Mode)</span>
                </button>

                <!-- Auth Mode Switcher -->
                <p id="authToggleText" class="text-xs text-slate-400">
                    ليس لديك حساب؟ 
                    <button onclick="toggleAuthMode('signup')" class="text-violet-400 font-bold hover:underline">إنشاء حساب جديد</button>
                </p>
                <p id="authToggleTextAlt" class="text-xs text-slate-400 hidden">
                    لديك حساب بالفعل؟ 
                    <button onclick="toggleAuthMode('login')" class="text-violet-400 font-bold hover:underline">تسجيل الدخول</button>
                </p>
            </div>

        </div>
    </div>


    <!-- ================= MAIN DASHBOARD VIEW (HIDDEN INITIALLY) ================= -->
    <div id="dashboardView" class="hidden flex-col min-h-screen">
        
        <!-- Top Navigation Header -->
        <header class="bg-slate-900/80 backdrop-blur-md border-b border-slate-800 sticky top-0 z-40">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
                <div class="flex items-center space-x-3 space-x-reverse">
                    <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-violet-600 to-teal-400 flex items-center justify-center text-white shadow-lg shadow-violet-500/20">
                        <i class="fa-solid fa-server text-lg"></i>
                    </div>
                    <div>
                        <h1 class="font-bold text-lg text-white tracking-wide">IT Assets Hub</h1>
                        <p class="text-xs text-slate-400">إدارة الأصول والبنية التحتية للشبكات</p>
                    </div>
                </div>

                <!-- Action Buttons & User Profile -->
                <div class="flex items-center gap-3">
                    <button onclick="toggleBmcModal()" class="px-3.5 py-2 text-xs md:text-sm font-semibold rounded-lg bg-gradient-to-r from-violet-600 to-indigo-600 text-white hover:from-violet-500 hover:to-indigo-500 transition-all shadow-md flex items-center gap-2">
                        <i class="fa-solid fa-lightbulb text-amber-300"></i>
                        <span class="hidden sm:inline">نموذج العمل التجاري</span> (BMC)
                    </button>
                    <button onclick="openAddAssetModal()" class="px-3 py-2 text-xs md:text-sm font-semibold rounded-lg bg-teal-500 text-slate-950 hover:bg-teal-400 transition-all flex items-center gap-2">
                        <i class="fa-solid fa-plus"></i>
                        <span class="hidden sm:inline">إضافة أصل</span>
                    </button>

                    <!-- User Pill & Logout -->
                    <div class="flex items-center gap-2 pr-2 border-r border-slate-800 mr-1">
                        <div class="flex items-center gap-2 bg-slate-950 px-3 py-1.5 rounded-xl border border-slate-800">
                            <div class="w-6 h-6 rounded-full bg-violet-600/30 text-violet-400 flex items-center justify-center text-xs font-bold" id="userAvatar">A</div>
                            <span class="text-xs font-medium text-slate-200 hidden md:inline" id="userNameDisplay">مدير النظام</span>
                        </div>
                        <button onclick="logout()" class="p-2 text-slate-400 hover:text-rose-400 hover:bg-slate-800 rounded-lg transition-all" title="تسجيل الخروج">
                            <i class="fa-solid fa-arrow-right-from-bracket text-sm"></i>
                        </button>
                    </div>
                </div>
            </div>
        </header>

        <!-- Main Container -->
        <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 flex-1 w-full space-y-8">
            
            <!-- Banner Intro for College Project -->
            <div class="bg-gradient-to-r from-slate-900 via-slate-800 to-slate-900 border border-slate-800 rounded-2xl p-6 relative overflow-hidden">
                <div class="absolute -left-10 -bottom-10 w-40 h-40 bg-violet-500/10 rounded-full blur-3xl"></div>
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 relative z-10">
                    <div>
                        <span class="px-3 py-1 bg-violet-500/10 text-violet-400 border border-violet-500/20 text-xs font-semibold rounded-full inline-block mb-2">مشروع مادة ريادة الأعمال</span>
                        <h2 class="text-xl md:text-2xl font-bold text-white mb-1">منصة تتبع وإدارة الأصول والشهادات السحابية</h2>
                        <p class="text-sm text-slate-400 max-w-2xl">حل ريادي مخصص للشركات والأقسام التقنية للحد من تعطل الأجهزة وتفادي انتهاء تراخيص البرمجيات والشهادات الأمنية.</p>
                    </div>
                    <div class="flex items-center gap-2 self-start md:self-auto">
                        <span class="text-xs text-slate-400">حالة النظام الكلية:</span>
                        <span class="flex items-center gap-1.5 px-3 py-1 rounded-full bg-emerald-500/10 border border-emerald-500/30 text-emerald-400 text-xs font-medium">
                            <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
                            مستقر 99.8%
                        </span>
                    </div>
                </div>
            </div>

            <!-- Metrics & Stats Grid -->
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
                <!-- Stat Card 1 -->
                <div class="bg-slate-900 border border-slate-800 rounded-xl p-5 hover:border-slate-700 transition-all">
                    <div class="flex items-center justify-between mb-3">
                        <span class="text-xs text-slate-400 font-medium">إجمالي الأصول والأنظمة</span>
                        <div class="w-8 h-8 rounded-lg bg-slate-800 flex items-center justify-center text-slate-300">
                            <i class="fa-solid fa-boxes-stacked text-sm"></i>
                        </div>
                    </div>
                    <div class="text-3xl font-extrabold text-white" id="stat-total-assets">0</div>
                    <p class="text-xs text-slate-500 mt-2">سيرفرات، راوترات، وتراخيص</p>
                </div>

                <!-- Stat Card 2 -->
                <div class="bg-slate-900 border border-slate-800 rounded-xl p-5 hover:border-slate-700 transition-all">
                    <div class="flex items-center justify-between mb-3">
                        <span class="text-xs text-slate-400 font-medium">الأجهزة والشبكات النشطة</span>
                        <div class="w-8 h-8 rounded-lg bg-emerald-500/10 flex items-center justify-center text-emerald-400">
                            <i class="fa-solid fa-network-wired text-sm"></i>
                        </div>
                    </div>
                    <div class="text-3xl font-extrabold text-emerald-400" id="stat-active">0</div>
                    <p class="text-xs text-slate-500 mt-2">تعمل بكفاءة عالية</p>
                </div>

                <!-- Stat Card 3 -->
                <div class="bg-slate-900 border border-slate-800 rounded-xl p-5 hover:border-slate-700 transition-all">
                    <div class="flex items-center justify-between mb-3">
                        <span class="text-xs text-slate-400 font-medium">تراخيص تنتهي قريباً</span>
                        <div class="w-8 h-8 rounded-lg bg-amber-500/10 flex items-center justify-center text-amber-400">
                            <i class="fa-solid fa-triangle-exclamation text-sm"></i>
                        </div>
                    </div>
                    <div class="text-3xl font-extrabold text-amber-400" id="stat-expiring">0</div>
                    <p class="text-xs text-slate-500 mt-2">تتطلب تجديد خلال 30 يوم</p>
                </div>

                <!-- Stat Card 4 -->
                <div class="bg-slate-900 border border-slate-800 rounded-xl p-5 hover:border-slate-700 transition-all">
                    <div class="flex items-center justify-between mb-3">
                        <span class="text-xs text-slate-400 font-medium">تحت الصيانة</span>
                        <div class="w-8 h-8 rounded-lg bg-rose-500/10 flex items-center justify-center text-rose-400">
                            <i class="fa-solid fa-wrench text-sm"></i>
                        </div>
                    </div>
                    <div class="text-3xl font-extrabold text-rose-400" id="stat-maintenance">0</div>
                    <p class="text-xs text-slate-500 mt-2">فحص فني دوري ومتابعة</p>
                </div>
            </div>

            <!-- Section: Network Health & Asset Table -->
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                
                <!-- Left Side: Interactive Asset List (2 Cols) -->
                <div class="lg:col-span-2 bg-slate-900 border border-slate-800 rounded-2xl p-6 flex flex-col gap-6">
                    
                    <!-- Table Filters and Search -->
                    <div class="flex flex-col sm:flex-row items-stretch sm:items-center justify-between gap-4">
                        <!-- Search Input -->
                        <div class="relative flex-1">
                            <i class="fa-solid fa-magnifying-glass absolute right-3 top-1/2 -translate-y-1/2 text-slate-500 text-sm"></i>
                            <input type="text" id="searchInput" oninput="renderAssets()" placeholder="البحث باسم الأصل أو الـ IP..." class="w-full bg-slate-950 border border-slate-800 rounded-xl py-2.5 pr-9 pl-4 text-sm text-white placeholder-slate-500 focus:outline-none focus:border-violet-500 transition-all">
                        </div>

                        <!-- Filter Tabs -->
                        <div class="flex items-center gap-1 bg-slate-950 p-1 rounded-xl border border-slate-800 text-xs overflow-x-auto">
                            <button onclick="setFilter('all')" id="filter-all" class="filter-btn px-3 py-1.5 rounded-lg text-slate-300 font-medium hover:text-white transition-all">الكل</button>
                            <button onclick="setFilter('active')" id="filter-active" class="filter-btn px-3 py-1.5 rounded-lg text-slate-400 font-medium hover:text-white transition-all">نشط</button>
                            <button onclick="setFilter('expiring')" id="filter-expiring" class="filter-btn px-3 py-1.5 rounded-lg text-slate-400 font-medium hover:text-white transition-all">تراخيص تنتهي</button>
                            <button onclick="setFilter('maintenance')" id="filter-maintenance" class="filter-btn px-3 py-1.5 rounded-lg text-slate-400 font-medium hover:text-white transition-all">صيانة</button>
                        </div>
                    </div>

                    <!-- Assets Table -->
                    <div class="overflow-x-auto custom-scrollbar">
                        <table class="w-full text-right text-sm">
                            <thead>
                                <tr class="text-xs text-slate-400 border-b border-slate-800 pb-3">
                                    <th class="pb-3 font-semibold">الأصل / الجهاز</th>
                                    <th class="pb-3 font-semibold">الفئة</th>
                                    <th class="pb-3 font-semibold">عنوان IP / الترخيص</th>
                                    <th class="pb-3 font-semibold">الحالة</th>
                                    <th class="pb-3 font-semibold">التاريخ</th>
                                    <th class="pb-3 font-semibold text-left">إجراءات</th>
                                </tr>
                            </thead>
                            <tbody id="assetsTableBody" class="divide-y divide-slate-800/60">
                                <!-- JS Inject Table Rows -->
                            </tbody>
                        </table>
                        <div id="emptyState" class="hidden py-12 text-center text-slate-500 text-sm">
                            <i class="fa-solid fa-folder-open text-3xl mb-2 text-slate-600 block"></i>
                            لا توجد أصول تطابق البحث أو الفلتر المحدد.
                        </div>
                    </div>
                </div>

                <!-- Right Side: System & Network Topology Widget (1 Col) -->
                <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 flex flex-col justify-between space-y-6">
                    <div>
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-bold text-white text-base flex items-center gap-2">
                                <i class="fa-solid fa-heart-pulse text-violet-400"></i>
                                حالة خوادم الشبكة الرئيسية
                            </h3>
                            <span class="text-xs bg-slate-800 text-slate-400 px-2.5 py-1 rounded-full">مباشر</span>
                        </div>
                        <p class="text-xs text-slate-400 mb-6">تتبع حي لاستقرار الخدمات الأساسية لقسم إدارة الشبكات</p>

                        <!-- Network Nodes List -->
                        <div class="space-y-4">
                            <!-- Node 1 -->
                            <div class="p-3 bg-slate-950 rounded-xl border border-slate-800/80 flex items-center justify-between">
                                <div class="flex items-center gap-3">
                                    <div class="w-3 h-3 rounded-full bg-emerald-500 animate-pulse"></div>
                                    <div>
                                        <h4 class="text-xs font-bold text-white">Domain Controller (AD)</h4>
                                        <p class="text-[11px] text-slate-500">192.168.1.10</p>
                                    </div>
                                </div>
                                <span class="text-xs text-emerald-400 font-mono">99.9%</span>
                            </div>

                            <!-- Node 2 -->
                            <div class="p-3 bg-slate-950 rounded-xl border border-slate-800/80 flex items-center justify-between">
                                <div class="flex items-center gap-3">
                                    <div class="w-3 h-3 rounded-full bg-emerald-500"></div>
                                    <div>
                                        <h4 class="text-xs font-bold text-white">Core Firewall & Router</h4>
                                        <p class="text-[11px] text-slate-500">10.0.0.1</p>
                                    </div>
                                </div>
                                <span class="text-xs text-emerald-400 font-mono">100%</span>
                            </div>

                            <!-- Node 3 -->
                            <div class="p-3 bg-slate-950 rounded-xl border border-slate-800/80 flex items-center justify-between">
                                <div class="flex items-center gap-3">
                                    <div class="w-3 h-3 rounded-full bg-amber-500"></div>
                                    <div>
                                        <h4 class="text-xs font-bold text-white">NAS Backup Storage</h4>
                                        <p class="text-[11px] text-slate-500">192.168.1.50</p>
                                    </div>
                                </div>
                                <span class="text-xs text-amber-400 font-mono">94.2%</span>
                            </div>

                            <!-- Node 4 -->
                            <div class="p-3 bg-slate-950 rounded-xl border border-slate-800/80 flex items-center justify-between">
                                <div class="flex items-center gap-3">
                                    <div class="w-3 h-3 rounded-full bg-emerald-500"></div>
                                    <div>
                                        <h4 class="text-xs font-bold text-white">DHCP & DNS Server</h4>
                                        <p class="text-[11px] text-slate-500">192.168.1.2</p>
                                    </div>
                                </div>
                                <span class="text-xs text-emerald-400 font-mono">99.9%</span>
                            </div>
                        </div>
                    </div>

                    <!-- Entrepreneurship Quick Tip -->
                    <div class="p-4 rounded-xl bg-violet-950/30 border border-violet-800/40 text-xs space-y-2">
                        <span class="text-violet-300 font-bold flex items-center gap-1.5">
                            <i class="fa-solid fa-graduation-cap"></i> ملحوظة لعرض المشروع
                        </span>
                        <p class="text-slate-400 leading-relaxed">
                            تُبرز هذه اللوحة حل مشكلة ضياع الوقت وتكاليف التوقف الفجائي للأنظمة، وهي النقطة الجوهرية للتحقق من جدوى الفكرة الاستثمارية.
                        </p>
                    </div>
                </div>

            </div>

        </main>

        <!-- Footer -->
        <footer class="border-t border-slate-800 py-6 text-center text-xs text-slate-500">
            <p>مشروع مادة ريادة الأعمال - تخصص إدارة أنظمة الشبكات © 2026</p>
        </footer>
    </div>


    <!-- ADD ASSET MODAL -->
    <div id="addAssetModal" class="fixed inset-0 bg-black/70 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-slate-900 border border-slate-800 rounded-2xl w-full max-w-md p-6 shadow-2xl relative">
            <button onclick="closeAddAssetModal()" class="absolute left-4 top-4 text-slate-400 hover:text-white">
                <i class="fa-solid fa-xmark text-lg"></i>
            </button>
            <h3 class="text-lg font-bold text-white mb-4">إضافة أصل / جهاز جديد</h3>
            
            <form id="addAssetForm" onsubmit="handleFormSubmit(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-medium text-slate-300 mb-1">اسم الأصل أو الجهاز</label>
                    <input type="text" id="assetName" required placeholder="مثال: Main Cisco Switch" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-sm text-white focus:outline-none focus:border-violet-500">
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-medium text-slate-300 mb-1">الفئة</label>
                        <select id="assetCategory" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-sm text-white focus:outline-none focus:border-violet-500">
                            <option value="سيرفر">سيرفر (Server)</option>
                            <option value="شبكات">أجهزة شبكات (Router/Switch)</option>
                            <option value="ترخيص">ترخيص برمجيات (License)</option>
                            <option value="جهاز ذكي">حاسب / حاسوب (Hardware)</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-medium text-slate-300 mb-1">الحالة</label>
                        <select id="assetStatus" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-sm text-white focus:outline-none focus:border-violet-500">
                            <option value="active">نشط (Active)</option>
                            <option value="expiring">ينتهي قريباً</option>
                            <option value="maintenance">صيانة</option>
                        </select>
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-medium text-slate-300 mb-1">عنوان IP أو مفتاح الترخيص</label>
                    <input type="text" id="assetIp" required placeholder="192.168.1.100 أو XXXX-XXXX" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-sm text-white focus:outline-none focus:border-violet-500">
                </div>

                <div>
                    <label class="block text-xs font-medium text-slate-300 mb-1">تاريخ التجنيح / الانتهاء</label>
                    <input type="date" id="assetDate" required class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-sm text-white focus:outline-none focus:border-violet-500">
                </div>

                <div class="pt-3 flex items-center justify-end gap-3">
                    <button type="button" onclick="closeAddAssetModal()" class="px-4 py-2 rounded-xl text-xs text-slate-400 hover:text-white border border-slate-800">إلغاء</button>
                    <button type="submit" class="px-4 py-2 rounded-xl text-xs font-bold bg-violet-600 hover:bg-violet-500 text-white transition-all">حفظ الأصل</button>
                </div>
            </form>
        </div>
    </div>

    <!-- BUSINESS MODEL CANVAS (BMC) MODAL -->
    <div id="bmcModal" class="fixed inset-0 bg-black/80 backdrop-blur-md z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-slate-900 border border-slate-800 rounded-2xl w-full max-w-4xl max-h-[90vh] overflow-y-auto custom-scrollbar p-6 shadow-2xl relative">
            <button onclick="toggleBmcModal()" class="absolute left-5 top-5 text-slate-400 hover:text-white">
                <i class="fa-solid fa-xmark text-xl"></i>
            </button>

            <div class="mb-6">
                <span class="px-3 py-1 bg-teal-500/10 text-teal-400 border border-teal-500/20 text-xs font-semibold rounded-full inline-block mb-2">عرض التقديم لمادة ريادة الأعمال</span>
                <h3 class="text-2xl font-extrabold text-white">نموذج العمل التجاري (Business Model Canvas)</h3>
                <p class="text-sm text-slate-400">تحليل ريادي متكامل لمشروع "IT Assets Hub" الموجه لأقسام تقنية المعلومات والشبكات</p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                
                <!-- Box 1: Problem -->
                <div class="bg-slate-950 border border-slate-800 rounded-xl p-4">
                    <div class="text-violet-400 text-sm font-bold mb-2 flex items-center gap-2">
                        <i class="fa-solid fa-circle-exclamation"></i> 1. المشكلة (Problem)
                    </div>
                    <ul class="text-xs text-slate-300 space-y-2 list-disc list-inside">
                        <li>تعطل الخوادم المفاجئ بسبب غياب التتبع المنظم.</li>
                        <li>غرامات ومشاكل انقطاع بسبب انتهاء التراخيص والشهادات الأمنية دون علم الإدارة.</li>
                        <li>اعتماد المنشآت على جداول Excel عشوائية لإدارة الأجهزة الثمينة.</li>
                    </ul>
                </div>

                <!-- Box 2: Solution -->
                <div class="bg-slate-950 border border-slate-800 rounded-xl p-4">
                    <div class="text-teal-400 text-sm font-bold mb-2 flex items-center gap-2">
                        <i class="fa-solid fa-square-check"></i> 2. الحل المقترح (Solution)
                    </div>
                    <ul class="text-xs text-slate-300 space-y-2 list-disc list-inside">
                        <li>منصة سحابية موحدة ومبسطة لتتبع حالة الأصول التقنية والشبكية.</li>
                        <li>نظام تنبيهات مبكرة قبل انتهاء عقود التراخيص أو الصيانة.</li>
                        <li>لوحة تحكم تفاعلية توضح جاهزية الخوادم (Uptime).</li>
                    </ul>
                </div>

                <!-- Box 3: Customer Segments -->
                <div class="bg-slate-950 border border-slate-800 rounded-xl p-4">
                    <div class="text-indigo-400 text-sm font-bold mb-2 flex items-center gap-2">
                        <i class="fa-solid fa-users"></i> 3. شرائح العملاء (Customers)
                    </div>
                    <ul class="text-xs text-slate-300 space-y-2 list-disc list-inside">
                        <li>الشركات الصغيرة والمتوسطة (SMEs).</li>
                        <li>المدارس والمراكز التدريبية والأكاديمية.</li>
                        <li>مدراء ومسؤولو شبكات تقنية المعلومات (Network Admins).</li>
                    </ul>
                </div>

                <!-- Box 4: Value Proposition -->
                <div class="bg-slate-950 border border-slate-800 rounded-xl p-4">
                    <div class="text-amber-400 text-sm font-bold mb-2 flex items-center gap-2">
                        <i class="fa-solid fa-gem"></i> 4. القيمة المضافة (Value Proposition)
                    </div>
                    <ul class="text-xs text-slate-300 space-y-2 list-disc list-inside">
                        <li>حماية المنشأة من توقف الأعمال المفاجئ (Zero Downtime goal).</li>
                        <li>توفير الميزانية وتفادي خسائر تجديد التراخيص المتأخرة.</li>
                        <li>سهولة الاستخدام دون الحاجة لتأهيل تدريبي معقد.</li>
                    </ul>
                </div>

                <!-- Box 5: Revenue Streams -->
                <div class="bg-slate-950 border border-slate-800 rounded-xl p-4">
                    <div class="text-emerald-400 text-sm font-bold mb-2 flex items-center gap-2">
                        <i class="fa-solid fa-coins"></i> 5. مصادر الدخل (Revenue)
                    </div>
                    <ul class="text-xs text-slate-300 space-y-2 list-disc list-inside">
                        <li>اشتراكات شهريّة/سنوية بحسب عدد الأجهزة (SaaS Model).</li>
                        <li>رسوم إعداد وإعادة هيكلة وتوثيق شبكات المنشأة.</li>
                        <li>تقديم تقارير التدقيق الدوري كخدمة إضافية.</li>
                    </ul>
                </div>

                <!-- Box 6: Channels & Cost -->
                <div class="bg-slate-950 border border-slate-800 rounded-xl p-4">
                    <div class="text-rose-400 text-sm font-bold mb-2 flex items-center gap-2">
                        <i class="fa-solid fa-chart-pie"></i> 6. التكاليف والقنوات (Cost & Channels)
                    </div>
                    <ul class="text-xs text-slate-300 space-y-2 list-disc list-inside">
                        <li><strong>القنوات:</strong> التسويق الرقمي المباشر عبر LinkedIn ومجتمعات التقنية المحلية.</li>
                        <li><strong>التكاليف:</strong> استضافة السيرفرات السحابية، تطوير المنصة والتسويق.</li>
                    </ul>
                </div>

            </div>

            <div class="mt-6 text-center">
                <button onclick="toggleBmcModal()" class="px-6 py-2.5 bg-slate-800 hover:bg-slate-700 text-white rounded-xl text-xs font-semibold">إغلاق النافذة</button>
            </div>
        </div>
    </div>

    <!-- JavaScript Application Logic -->
    <script>
        // Initial Assets Mock Data
        let assets = [
            { id: 1, name: "Dell PowerEdge R740 Server", category: "سيرفر", ip: "192.168.1.15", status: "active", date: "2026-11-15" },
            { id: 2, name: "Cisco Catalyst 9300 Switch", category: "شبكات", ip: "10.0.4.1", status: "active", date: "2027-01-20" },
            { id: 3, name: "Fortinet FortiGate Firewall", category: "شبكات", ip: "10.0.0.254", status: "expiring", date: "2026-04-10" },
            { id: 4, name: "VMware vSphere License", category: "ترخيص", ip: "VS6-STD-XXXX", status: "expiring", date: "2026-03-30" },
            { id: 5, name: "HP ProLiant Backup Node", category: "سيرفر", ip: "192.168.1.88", status: "maintenance", date: "2026-03-28" },
        ];

        let currentFilter = 'all';

        // Toast Notification Function
        function showToast(message, type = 'success') {
            const container = document.getElementById('toastContainer');
            const toast = document.createElement('div');
            
            let bgClass = 'bg-slate-900 border-slate-700 text-white';
            let icon = 'fa-circle-check text-emerald-400';
            
            if(type === 'info') icon = 'fa-circle-info text-sky-400';
            if(type === 'error') icon = 'fa-circle-xmark text-rose-400';

            toast.className = `px-4 py-3 rounded-xl border ${bgClass} shadow-xl text-xs flex items-center gap-3 animate-toast pointer-events-auto`;
            toast.innerHTML = `<i class="fa-solid ${icon} text-base"></i><span>${message}</span>`;
            
            container.appendChild(toast);

            setTimeout(() => {
                toast.style.opacity = '0';
                toast.style.transition = 'opacity 0.3s ease';
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        // Toggle Auth View Mode (Login / Sign Up)
        function toggleAuthMode(mode) {
            const loginForm = document.getElementById('loginForm');
            const signUpForm = document.getElementById('signUpForm');
            const t1 = document.getElementById('authToggleText');
            const t2 = document.getElementById('authToggleTextAlt');

            if(mode === 'signup') {
                loginForm.classList.add('hidden');
                signUpForm.classList.remove('hidden');
                t1.classList.add('hidden');
                t2.classList.remove('hidden');
            } else {
                signUpForm.classList.add('hidden');
                loginForm.classList.remove('hidden');
                t2.classList.add('hidden');
                t1.classList.remove('hidden');
            }
        }

        // Authentication Submission
        function handleAuthSubmit(e, type) {
            e.preventDefault();
            let userName = 'مدير النظام';

            if(type === 'signup') {
                userName = document.getElementById('signUpName').value || 'مدير جديد';
                showToast(`تم إنشاء حسابك بنجاح! مرحباً بك ${userName}`, 'success');
            } else {
                const email = document.getElementById('loginEmail').value;
                userName = email.split('@')[0] || 'مدير النظام';
                showToast(`أهلاً بك مجدداً، ${userName}`, 'success');
            }

            setUserProfile(userName);
            showDashboard();
        }

        // Quick Demo Login (for Class Presentation)
        function demoLogin() {
            showToast('تم تسجيل الدخول في وضع العرض التجريبي السريع', 'info');
            setUserProfile('رهف (طالبة ريادة الأعمال)');
            showDashboard();
        }

        function setUserProfile(name) {
            document.getElementById('userNameDisplay').textContent = name;
            document.getElementById('userAvatar').textContent = name.charAt(0).toUpperCase();
        }

        function showDashboard() {
            document.getElementById('authView').classList.add('hidden');
            document.getElementById('dashboardView').classList.remove('hidden');
            document.getElementById('dashboardView').classList.add('flex');
            renderAssets();
        }

        function logout() {
            document.getElementById('dashboardView').classList.add('hidden');
            document.getElementById('dashboardView').classList.remove('flex');
            document.getElementById('authView').classList.remove('hidden');
            showToast('تم تسجيل الخروج بنجاح', 'info');
        }

        // Render Page Data & Table
        function renderAssets() {
            const tableBody = document.getElementById('assetsTableBody');
            const searchVal = document.getElementById('searchInput').value.toLowerCase();
            const emptyState = document.getElementById('emptyState');
            
            tableBody.innerHTML = '';

            const filtered = assets.filter(item => {
                const matchesFilter = currentFilter === 'all' || item.status === currentFilter;
                const matchesSearch = item.name.toLowerCase().includes(searchVal) || item.ip.toLowerCase().includes(searchVal);
                return matchesFilter && matchesSearch;
            });

            if (filtered.length === 0) {
                emptyState.classList.remove('hidden');
            } else {
                emptyState.classList.add('hidden');
            }

            filtered.forEach(asset => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-800/40 transition-colors";
                
                let statusBadge = '';
                if(asset.status === 'active') {
                    statusBadge = `<span class="px-2.5 py-1 rounded-full text-[11px] font-semibold bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">نشط</span>`;
                } else if(asset.status === 'expiring') {
                    statusBadge = `<span class="px-2.5 py-1 rounded-full text-[11px] font-semibold bg-amber-500/10 text-amber-400 border border-amber-500/20">ينتهي قريباً</span>`;
                } else {
                    statusBadge = `<span class="px-2.5 py-1 rounded-full text-[11px] font-semibold bg-rose-500/10 text-rose-400 border border-rose-500/20">تحت الصيانة</span>`;
                }

                tr.innerHTML = `
                    <td class="py-3.5 font-semibold text-white">${asset.name}</td>
                    <td class="py-3.5 text-slate-400 text-xs">${asset.category}</td>
                    <td class="py-3.5 font-mono text-xs text-violet-300">${asset.ip}</td>
                    <td class="py-3.5">${statusBadge}</td>
                    <td class="py-3.5 text-slate-400 text-xs">${asset.date}</td>
                    <td class="py-3.5 text-left">
                        <button onclick="deleteAsset(${asset.id})" class="text-slate-500 hover:text-rose-400 transition-colors p-1" title="حذف الأصل">
                            <i class="fa-solid fa-trash-can text-sm"></i>
                        </button>
                    </td>
                `;
                tableBody.appendChild(tr);
            });

            updateStats();
        }

        // Update Stat Counters
        function updateStats() {
            document.getElementById('stat-total-assets').textContent = assets.length;
            document.getElementById('stat-active').textContent = assets.filter(a => a.status === 'active').length;
            document.getElementById('stat-expiring').textContent = assets.filter(a => a.status === 'expiring').length;
            document.getElementById('stat-maintenance').textContent = assets.filter(a => a.status === 'maintenance').length;
        }

        // Filter Handling
        function setFilter(filter) {
            currentFilter = filter;
            document.querySelectorAll('.filter-btn').forEach(btn => {
                btn.classList.remove('bg-violet-600', 'text-white');
                btn.classList.add('text-slate-400');
            });
            const activeBtn = document.getElementById(`filter-${filter}`);
            if(activeBtn) {
                activeBtn.classList.add('bg-violet-600', 'text-white');
                activeBtn.classList.remove('text-slate-400');
            }
            renderAssets();
        }

        // Add Asset Modal Logic
        function openAddAssetModal() {
            document.getElementById('addAssetModal').classList.remove('hidden');
        }

        function closeAddAssetModal() {
            document.getElementById('addAssetModal').classList.add('hidden');
        }

        function handleFormSubmit(e) {
            e.preventDefault();
            const newAsset = {
                id: Date.now(),
                name: document.getElementById('assetName').value,
                category: document.getElementById('assetCategory').value,
                status: document.getElementById('assetStatus').value,
                ip: document.getElementById('assetIp').value,
                date: document.getElementById('assetDate').value
            };

            assets.unshift(newAsset);
            renderAssets();
            closeAddAssetModal();
            document.getElementById('addAssetForm').reset();
            showToast('تم إضافة الأصل الجديد بنجاح!', 'success');
        }

        // Delete Asset Logic
        function deleteAsset(id) {
            assets = assets.filter(a => a.id !== id);
            renderAssets();
            showToast('تم إزالة الأصل من القائمة', 'info');
        }

        // BMC Modal Toggle
        function toggleBmcModal() {
            const modal = document.getElementById('bmcModal');
            modal.classList.toggle('hidden');
        }
    </script>
</body>
</html>ssest-Hub
