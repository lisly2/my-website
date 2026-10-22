<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>أطلس الأشياء المنسية | The Atlas of Forgotten Things</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Cairo & Tajawal -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700;900&family=Tajawal:wght@300;400;500;700&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        atlas: {
                            bg: '#0a0512',
                            card: '#160d26',
                            accent: '#9d4edd',
                            gold: '#ffb703',
                            rose: '#f72585',
                            glow: '#e0aaff',
                            deep: '#030108'
                        }
                    },
                    fontFamily: {
                        cairo: ['Cairo', 'sans-serif'],
                        tajawal: ['Tajawal', 'sans-serif']
                    },
                    animation: {
                        'float': 'float 6s ease-in-out infinite',
                        'float-delayed': 'float 7s ease-in-out 2s infinite',
                        'pulse-slow': 'pulse 3s cubic-bezier(0.4, 0, 0.6, 1) infinite',
                        'glow-spin': 'spin 12s linear infinite',
                        'sparkle': 'sparkle 2s ease-in-out infinite'
                    },
                    keyframes: {
                        float: {
                            '0%, 100%': { transform: 'translateY(0px) rotate(0deg)' },
                            '50%': { transform: 'translateY(-15px) rotate(2deg)' }
                        },
                        sparkle: {
                            '0%, 100%': { opacity: 0.3, transform: 'scale(0.8)' },
                            '50%': { opacity: 1, transform: 'scale(1.2)' }
                        }
                    }
                }
            }
        }
    </script>

    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #0a0512;
            color: #f1f1f1;
            overflow-x: hidden;
            user-select: none;
        }

        /* Glassmorphism effects */
        .glass-panel {
            background: rgba(22, 13, 38, 0.65);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(157, 78, 221, 0.25);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.5);
        }

        .glass-card {
            background: rgba(30, 18, 51, 0.5);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 183, 3, 0.15);
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .glass-card:hover {
            border-color: rgba(255, 183, 3, 0.6);
            box-shadow: 0 0 25px rgba(157, 78, 221, 0.4), inset 0 0 15px rgba(255, 183, 3, 0.1);
            transform: translateY(-8px) scale(1.02);
        }

        /* Glowing texts & buttons */
        .glow-text-gold {
            text-shadow: 0 0 12px rgba(255, 183, 3, 0.6), 0 0 25px rgba(255, 183, 3, 0.3);
        }

        .glow-text-purple {
            text-shadow: 0 0 12px rgba(157, 78, 221, 0.8), 0 0 30px rgba(224, 170, 255, 0.4);
        }

        .btn-atlas {
            background: linear-gradient(135deg, #7b2cbf 0%, #3c096c 100%);
            border: 1px solid rgba(255, 183, 3, 0.5);
            box-shadow: 0 0 20px rgba(123, 44, 191, 0.5);
            transition: all 0.3s ease;
        }

        .btn-atlas:hover {
            background: linear-gradient(135deg, #9d4edd 0%, #5a189a 100%);
            box-shadow: 0 0 35px rgba(255, 183, 3, 0.8), 0 0 15px rgba(224, 170, 255, 0.6);
            transform: scale(1.05);
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0a0512;
        }
        ::-webkit-scrollbar-thumb {
            background: #3c096c;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #9d4edd;
        }

        /* Canvas background overlay */
        #starCanvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            pointer-events: none;
            z-index: 0;
        }

        /* Particles container for items */
        .particle {
            position: absolute;
            pointer-events: none;
            border-radius: 50%;
            animation: popParticle 0.8s ease-out forwards;
        }

        @keyframes popParticle {
            0% { opacity: 1; transform: translate(0, 0) scale(1); }
            100% { opacity: 0; transform: translate(var(--dx), var(--dy)) scale(0); }
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between relative selection:bg-atlas-accent selection:text-white">

    <!-- Canvas Background for Mystic Stars -->
    <canvas id="starCanvas"></canvas>

    <!-- Header Navigation -->
    <header class="relative z-20 w-full px-6 py-4 glass-panel border-b border-atlas-accent/20 sticky top-0 flex justify-between items-center">
        <div class="flex items-center gap-3 cursor-pointer" onclick="switchTab('gateway')">
            <div class="w-10 h-10 rounded-full bg-atlas-accent/20 border border-atlas-gold flex items-center justify-center text-atlas-gold shadow-[0_0_15px_rgba(255,183,3,0.4)]">
                <i class="fa-solid me-0.5 fa-compass text-xl animate-spin-slow"></i>
            </div>
            <div>
                <h1 class="text-lg font-bold tracking-wide text-white glow-text-purple">أطلس الأشياء المنسية</h1>
                <p class="text-xs text-atlas-glow/70 font-tajawal">The Atlas of Forgotten Things</p>
            </div>
        </div>

        <nav class="flex items-center gap-2 md:gap-4 text-sm font-tajawal">
            <button id="nav-gateway" onclick="switchTab('gateway')" class="px-3 py-1.5 rounded-lg transition-all hover:bg-atlas-accent/20 text-atlas-glow flex items-center gap-2">
                <i class="fa-solid fa-door-open"></i> <span class="hidden sm:inline">البوابة</span>
            </button>
            <button id="nav-map" onclick="switchTab('map')" class="px-3 py-1.5 rounded-lg transition-all hover:bg-atlas-accent/20 text-atlas-glow flex items-center gap-2">
                <i class="fa-solid fa-map-location-dot"></i> <span class="hidden sm:inline">الخريطة</span>
            </button>
            <button id="nav-room" onclick="switchTab('room')" class="px-3 py-1.5 rounded-lg transition-all hover:bg-atlas-accent/20 text-atlas-glow flex items-center gap-2">
                <i class="fa-solid fa-gem"></i> <span class="hidden sm:inline">غرفة الأشياء</span>
            </button>
            <button id="nav-journal" onclick="switchTab('journal')" class="px-3 py-1.5 rounded-lg transition-all hover:bg-atlas-accent/20 text-atlas-glow flex items-center gap-2 relative">
                <i class="fa-solid fa-book-bookmark"></i> <span class="hidden sm:inline">الدفتر</span>
                <span id="journalBadge" class="hidden absolute -top-1 -right-1 w-2.5 h-2.5 bg-atlas-gold rounded-full animate-ping"></span>
            </button>
        </nav>
    </header>

    <!-- Main Dynamic Content Container -->
    <main class="relative z-10 flex-grow container mx-auto px-4 py-8 flex flex-col justify-center items-center">

        <!-- ================= PAGE 1: GATEWAY (بوابة العالم) ================= -->
        <section id="page-gateway" class="w-full max-w-4xl text-center flex flex-col items-center justify-center min-h-[75vh] py-8 transition-all duration-700">
            <!-- Mystic Glowing Orb Visual -->
            <div class="relative mb-8 group cursor-pointer" onclick="switchTab('map')">
                <div class="absolute -inset-4 rounded-full bg-gradient-to-r from-atlas-accent via-atlas-rose to-atlas-gold opacity-50 blur-xl group-hover:opacity-80 transition duration-1000 group-hover:duration-200 animate-pulse-slow"></div>
                <div class="relative w-44 h-44 md:w-56 md:h-56 rounded-full glass-panel flex flex-col items-center justify-center border-2 border-atlas-gold/60 shadow-[0_0_50px_rgba(157,78,221,0.5)]">
                    <i class="fa-solid fa-hourglass-half text-6xl md:text-7xl text-atlas-gold animate-float"></i>
                    <span class="text-xs text-atlas-glow mt-3 font-tajawal tracking-widest uppercase opacity-80">عالم لا ينسى</span>
                </div>
            </div>

            <h2 class="text-3xl md:text-5xl font-extrabold text-white mb-4 tracking-wide leading-tight glow-text-gold">
                أطلس الأشياء المنسية
            </h2>
            <p class="text-lg md:text-2xl text-atlas-glow/90 font-tajawal italic mb-8 max-w-2xl leading-relaxed">
                «كُل شَيءٍ مَنسِيٍّ يَتْرُكُ أَثَراً... هُنا تُحْفَظُ الحكاياتُ التي أَهْمَلَها الزَّمانُ»
            </p>

            <p class="text-sm md:text-base text-gray-300 font-tajawal max-w-xl mb-10 leading-relaxed bg-atlas-card/40 p-4 rounded-xl border border-atlas-accent/20">
                مرحباً بك في الأطلس التفاعلي. استكشف الجزر المنسية، وانقر على الآثار الغامضة لتكشف أسرار القصص والألغاز المخفية.
            </p>

            <button onclick="switchTab('map')" class="btn-atlas px-8 py-4 rounded-full text-lg font-bold text-white flex items-center gap-3 group">
                <span>ادخلي الأطلس واستكشفي</span>
                <i class="fa-solid fa-arrow-left group-hover:-translate-x-2 transition-transform duration-300 text-atlas-gold"></i>
            </button>
        </section>

        <!-- ================= PAGE 2: INTERACTIVE MAP (الخريطة التفاعلية) ================= -->
        <section id="page-map" class="w-full max-w-5xl hidden transition-all duration-700 my-auto py-4">
            <div class="text-center mb-8">
                <span class="text-xs font-bold text-atlas-gold tracking-widest uppercase bg-atlas-gold/10 px-3 py-1 rounded-full border border-atlas-gold/30">الخريطة السحرية</span>
                <h2 class="text-2xl md:text-4xl font-bold text-white mt-2 glow-text-purple">جزر العالم المنسي</h2>
                <p class="text-sm text-atlas-glow/70 font-tajawal mt-1">اختر إحدى الجزر العائمة للغوص في غرف أسرارها</p>
            </div>

            <!-- Map Islands Grid -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 md:gap-8 mb-8">
                <!-- Island 1 -->
                <div onclick="openIsland('clocks')" class="glass-card p-6 rounded-2xl relative overflow-hidden cursor-pointer group animate-float">
                    <div class="absolute -top-10 -right-10 w-28 h-28 bg-atlas-gold/10 rounded-full blur-2xl group-hover:bg-atlas-gold/30 transition-all"></div>
                    <div class="flex items-center justify-between mb-4">
                        <div class="w-14 h-14 rounded-xl bg-atlas-gold/10 border border-atlas-gold/40 flex items-center justify-center text-atlas-gold text-2xl group-hover:scale-110 transition-transform">
                            <i class="fa-solid fa-clock-rotate-left"></i>
                        </div>
                        <span id="badge-island-clocks" class="text-xs px-2.5 py-1 rounded-full bg-atlas-accent/20 text-atlas-glow border border-atlas-accent/30 font-tajawal">0 / 3 مكتشف</span>
                    </div>
                    <h3 class="text-xl font-bold text-white mb-2 group-hover:text-atlas-gold transition-colors">جزيرة الساعات المنسية</h3>
                    <p class="text-xs text-gray-300 font-tajawal leading-relaxed">حيث يتوقف الزمن وتتحول الثواني إلى مجسمات نحاسية غامضة لا تدور إلا بذكريات الغرباء.</p>
                    <div class="mt-6 flex items-center text-xs text-atlas-gold font-bold gap-2">
                        <span>دخول الجزيرة</span>
                        <i class="fa-solid fa-chevron-left group-hover:-translate-x-1 transition-transform"></i>
                    </div>
                </div>

                <!-- Island 2 -->
                <div onclick="openIsland('letters')" class="glass-card p-6 rounded-2xl relative overflow-hidden cursor-pointer group animate-float-delayed">
                    <div class="absolute -top-10 -right-10 w-28 h-28 bg-atlas-accent/10 rounded-full blur-2xl group-hover:bg-atlas-accent/30 transition-all"></div>
                    <div class="flex items-center justify-between mb-4">
                        <div class="w-14 h-14 rounded-xl bg-atlas-accent/10 border border-atlas-accent/40 flex items-center justify-center text-atlas-accent text-2xl group-hover:scale-110 transition-transform">
                            <i class="fa-solid fa-envelope-open-text"></i>
                        </div>
                        <span id="badge-island-letters" class="text-xs px-2.5 py-1 rounded-full bg-atlas-accent/20 text-atlas-glow border border-atlas-accent/30 font-tajawal">0 / 3 مكتشف</span>
                    </div>
                    <h3 class="text-xl font-bold text-white mb-2 group-hover:text-atlas-accent transition-colors">جزيرة الرسائل العتيقة</h3>
                    <p class="text-xs text-gray-300 font-tajawal leading-relaxed">مستودع الخطابات التي لم تصل أصحابها قط، والمحفوظة بأختام من الشمع الأرجواني البرّاق.</p>
                    <div class="mt-6 flex items-center text-xs text-atlas-accent font-bold gap-2">
                        <span>دخول الجزيرة</span>
                        <i class="fa-solid fa-chevron-left group-hover:-translate-x-1 transition-transform"></i>
                    </div>
                </div>

                <!-- Island 3 -->
                <div onclick="openIsland('mirrors')" class="glass-card p-6 rounded-2xl relative overflow-hidden cursor-pointer group animate-float">
                    <div class="absolute -top-10 -right-10 w-28 h-28 bg-atlas-rose/10 rounded-full blur-2xl group-hover:bg-atlas-rose/30 transition-all"></div>
                    <div class="flex items-center justify-between mb-4">
                        <div class="w-14 h-14 rounded-xl bg-atlas-rose/10 border border-atlas-rose/40 flex items-center justify-center text-atlas-rose text-2xl group-hover:scale-110 transition-transform">
                            <i class="fa-solid fa-wand-magic-sparkles"></i>
                        </div>
                        <span id="badge-island-mirrors" class="text-xs px-2.5 py-1 rounded-full bg-atlas-accent/20 text-atlas-glow border border-atlas-accent/30 font-tajawal">0 / 3 مكتشف</span>
                    </div>
                    <h3 class="text-xl font-bold text-white mb-2 group-hover:text-atlas-rose transition-colors">جزيرة المرايا الغامضة</h3>
                    <p class="text-xs text-gray-300 font-tajawal leading-relaxed">مرايا لا تعكس الوجوه الحالية، بل تعكس أمنيات قديمة وانعكاسات لأماكن لم يعد لها وجود.</p>
                    <div class="mt-6 flex items-center text-xs text-atlas-rose font-bold gap-2">
                        <span>دخول الجزيرة</span>
                        <i class="fa-solid fa-chevron-left group-hover:-translate-x-1 transition-transform"></i>
                    </div>
                </div>
            </div>

            <!-- Secret Island (Locked by default, unlocked upon collecting 5 items) -->
            <div id="secret-island-card" onclick="openIsland('secret')" class="glass-panel p-6 rounded-2xl border-2 border-dashed border-atlas-gold/30 flex flex-col md:flex-row items-center justify-between gap-4 cursor-pointer hover:border-atlas-gold transition-all relative overflow-hidden opacity-75 hover:opacity-100">
                <div class="flex items-center gap-4">
                    <div class="w-12 h-12 rounded-full bg-atlas-gold/20 border border-atlas-gold flex items-center justify-center text-atlas-gold text-xl">
                        <i id="secret-icon" class="fa-solid fa-lock"></i>
                    </div>
                    <div>
                        <div class="flex items-center gap-2">
                            <h4 class="text-lg font-bold text-white">الجزيرة السرية: أرشيف النجوم المفقودة</h4>
                            <span id="secret-badge" class="text-[10px] bg-atlas-gold/20 text-atlas-gold px-2 py-0.5 rounded border border-atlas-gold/40">مغلقة</span>
                        </div>
                        <p id="secret-desc" class="text-xs text-gray-400 font-tajawal">تفتح تلقائياً عند اكتشاف 5 عناصر في الأطلس.</p>
                    </div>
                </div>
                <button id="secret-btn" class="px-5 py-2 rounded-xl bg-atlas-card border border-atlas-gold/40 text-xs text-atlas-gold font-bold pointer-events-none">
                    يتطلب 5 اكتشافات
                </button>
            </div>
        </section>

        <!-- ================= PAGE 3: ROOM OF ITEMS (غرفة الأشياء) ================= -->
        <section id="page-room" class="w-full max-w-5xl hidden transition-all duration-700 py-4">
            <!-- Room Header -->
            <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-8 glass-panel p-5 rounded-2xl">
                <div>
                    <span id="room-subtitle" class="text-xs text-atlas-gold font-bold tracking-widest uppercase">غرفة الاستكشاف</span>
                    <h2 id="room-title" class="text-2xl md:text-3xl font-bold text-white mt-1">جزيرة الساعات المنسية</h2>
                </div>
                <div class="flex items-center gap-3">
                    <button onclick="switchTab('map')" class="px-4 py-2 rounded-xl bg-atlas-card border border-atlas-accent/40 text-xs text-atlas-glow hover:bg-atlas-accent/30 transition-all flex items-center gap-2">
                        <i class="fa-solid fa-map"></i> العودة للخريطة
                    </button>
                </div>
            </div>

            <!-- Items Interactive Cards Grid -->
            <div id="room-items-grid" class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                <!-- Dynamic Items Inserted via JavaScript -->
            </div>

            <!-- Modal / Story Viewer for Selected Item -->
            <div id="item-modal" class="fixed inset-0 z-50 bg-black/80 backdrop-blur-md hidden flex items-center justify-center p-4">
                <div class="glass-panel w-full max-w-2xl rounded-3xl p-6 md:p-8 border border-atlas-gold/50 shadow-[0_0_50px_rgba(255,183,3,0.3)] relative animate-float">
                    <button onclick="closeItemModal()" class="absolute top-4 left-4 w-9 h-9 rounded-full bg-atlas-card border border-atlas-gold/30 text-gray-300 hover:text-white flex items-center justify-center">
                        <i class="fa-solid fa-xmark"></i>
                    </button>

                    <div class="flex items-center gap-3 mb-4">
                        <div id="modal-icon-container" class="w-12 h-12 rounded-xl bg-atlas-gold/20 border border-atlas-gold text-atlas-gold flex items-center justify-center text-2xl">
                            <i id="modal-icon" class="fa-solid fa-stopwatch"></i>
                        </div>
                        <div>
                            <span id="modal-tag" class="text-xs text-atlas-gold font-bold">عنصر منسي</span>
                            <h3 id="modal-title" class="text-2xl font-bold text-white">عنوان الشيء</h3>
                        </div>
                    </div>

                    <!-- Story Text Container -->
                    <div class="bg-atlas-bg/80 rounded-2xl p-5 border border-atlas-accent/20 mb-6">
                        <p id="modal-story" class="text-sm md:text-base text-gray-200 font-tajawal leading-relaxed italic mb-4">
                            قصة الشيء المنسي...
                        </p>
                        <div id="modal-puzzle-box" class="mt-4 pt-4 border-t border-atlas-accent/30">
                            <p id="modal-puzzle-question" class="text-xs md:text-sm font-bold text-atlas-gold mb-3">
                                لغز أو خيار متفرع:
                            </p>
                            <div id="modal-choices" class="flex flex-col gap-2">
                                <!-- Choices dynamically populated -->
                            </div>
                        </div>
                    </div>

                    <!-- Interaction Result Message -->
                    <div id="modal-result" class="hidden p-4 rounded-xl bg-atlas-accent/20 border border-atlas-accent text-xs md:text-sm text-atlas-glow font-tajawal mb-6">
                    </div>

                    <div class="flex justify-between items-center">
                        <span id="modal-status" class="text-xs text-green-400 font-bold flex items-center gap-1.5">
                            <i class="fa-solid fa-circle-check"></i> تمت إضافته لدفتر الاكتشافات
                        </span>
                        <button onclick="closeItemModal()" class="btn-atlas px-6 py-2.5 rounded-xl text-xs font-bold text-white">
                            متابعة الاستكشاف
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- ================= PAGE 4: JOURNAL OF DISCOVERIES (دفتر الاكتشافات) ================= -->
        <section id="page-journal" class="w-full max-w-5xl hidden transition-all duration-700 py-4">
            <div class="text-center mb-8">
                <span class="text-xs font-bold text-atlas-gold tracking-widest uppercase bg-atlas-gold/10 px-3 py-1 rounded-full border border-atlas-gold/30">دفتر الاكتشافات والأرشيف</span>
                <h2 class="text-2xl md:text-4xl font-bold text-white mt-2 glow-text-gold">سجل مكتشفة الأطلس</h2>
                <p class="text-sm text-atlas-glow/70 font-tajawal mt-1">يحتفظ الأطلس بكل حكاية وشارة ولغز قمتِ بحله</p>
            </div>

            <!-- Stats Bar -->
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
                <div class="glass-panel p-4 rounded-2xl text-center">
                    <span class="text-xs text-gray-400 font-tajawal">العناصر المكتشفة</span>
                    <h3 id="stat-collected" class="text-2xl md:text-3xl font-extrabold text-atlas-gold mt-1">0 / 10</h3>
                </div>
                <div class="glass-panel p-4 rounded-2xl text-center">
                    <span class="text-xs text-gray-400 font-tajawal">الألغاز المحلولة</span>
                    <h3 id="stat-puzzles" class="text-2xl md:text-3xl font-extrabold text-atlas-glow mt-1">0</h3>
                </div>
                <div class="glass-panel p-4 rounded-2xl text-center">
                    <span class="text-xs text-gray-400 font-tajawal">الشارات المفتوحة</span>
                    <h3 id="stat-badges" class="text-2xl md:text-3xl font-extrabold text-atlas-rose mt-1">0 / 4</h3>
                </div>
                <div class="glass-panel p-4 rounded-2xl text-center">
                    <span class="text-xs text-gray-400 font-tajawal">نسبة الاكتشاف</span>
                    <h3 id="stat-progress" class="text-2xl md:text-3xl font-extrabold text-green-400 mt-1">0%</h3>
                </div>
            </div>

            <!-- Progress Bar -->
            <div class="w-full bg-atlas-card p-4 rounded-2xl border border-atlas-accent/30 mb-8">
                <div class="flex justify-between items-center mb-2 text-xs font-bold">
                    <span class="text-atlas-glow">اكتمال خريطة الأطلس</span>
                    <span id="progress-text" class="text-atlas-gold">0%</span>
                </div>
                <div class="w-full bg-atlas-bg rounded-full h-3 overflow-hidden border border-atlas-accent/20">
                    <div id="progress-bar" class="bg-gradient-to-l from-atlas-gold via-atlas-rose to-atlas-accent h-full w-0 transition-all duration-1000"></div>
                </div>
            </div>

            <!-- Badges Section -->
            <div class="mb-10">
                <h3 class="text-xl font-bold text-white mb-4 flex items-center gap-2">
                    <i class="fa-solid fa-award text-atlas-gold"></i> شارات الإنجاز السحرية
                </h3>
                <div class="grid grid-cols-2 sm:grid-cols-4 gap-4" id="badges-container">
                    <!-- Dynamic Badges -->
                </div>
            </div>

            <!-- Collected Items Archive -->
            <div>
                <div class="flex justify-between items-center mb-4">
                    <h3 class="text-xl font-bold text-white flex items-center gap-2">
                        <i class="fa-solid fa-box-archive text-atlas-accent"></i> العناصر المجمعة
                    </h3>
                    <button onclick="resetProgress()" class="text-xs text-red-400 hover:text-red-300 underline font-tajawal">
                        إعادة ضبط التقدم
                    </button>
                </div>
                <div id="journal-items-grid" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <!-- Dynamic Archive Items -->
                </div>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="relative z-20 w-full py-4 text-center text-xs text-atlas-glow/50 font-tajawal border-t border-atlas-accent/10 glass-panel">
        <p>أطلس الأشياء المنسية © 2026 • تجربة تفاعلية برمجية بـ Python / Web Tech</p>
    </footer>

    <!-- JavaScript Game Logic -->
    <script>
        /* ================= DATABASE OF ATLAS DATA ================= */
        const ATLAS_DATA = {
            islands: {
                clocks: {
                    title: "جزيرة الساعات المنسية",
                    subtitle: "حيث يتوقف الزمن وتتراكم الثواني",
                    color: "atlas-gold",
                    items: [
                        {
                            id: "clock_1",
                            title: "ساعة النحاس المتوقفة عند 03:15",
                            icon: "fa-stopwatch",
                            summary: "ساعة جيب ذهبية عُثر عليها في قاع البحر، عقاربها متوقفة منذ عام 1892.",
                            story: "تقول الأسطورة إن هذه الساعة لا تقيس الوقت العادي، بل توقفت في اللحظة التي نسي فيها صاحبها أول حلم بحياته. عند النظر إليها بتمعن، تسمعين دقات خفيفة جداً تهمس باسم غريب.",
                            puzzle: "ماذا تفعلين مع الساعة النحاسية؟",
                            choices: [
                                { text: "إعادة تحريك العقارب يدوياً", result: "سمعتِ صوتاً هادئاً كأن الزمن عاد للخلف ثانية واحدة، وانطلقت شعاع ذهبي!", correct: true },
                                { text: "ترك الساعة كما هي والاستماع لدقاتها", result: "همس لكِ صوت المنسيات بكلمة سرية: 'الذكريات لا تموت'.", correct: true }
                            ]
                        },
                        {
                            id: "clock_2",
                            title: "منبه الرمل البنفسجي",
                            icon: "fa-hourglass-start",
                            summary: "ساعة رملية ينساب رملها من الأسفل إلى الأعلى بشكل مخالف للجاذبية.",
                            story: "صنعها كيمائي قديم حاول حبس لحظات السعادة داخل حبيبات الرمل البنفسجية. كلما شعرتِ بالحزن، يرتفع الرمل ليذكركِ بأن الأيام الجميلة تعود دائماً.",
                            puzzle: "كيف تستجيبين لتدفق الرمل؟",
                            choices: [
                                { text: "قلب الساعة الرملية رأساً على عقب", result: "انتشرت ذرات ضوئية بنفسجية ملأت المكان بعبير الزهور المنسية!", correct: true },
                                { text: "لمس زجاج الساعة الدافئ", result: "شعرتِ بدفء عجيب يسري في يدكِ وتم فتح السر الأول للجزيرة.", correct: true }
                            ]
                        },
                        {
                            id: "clock_3",
                            title: "ساعة البرج البلورية",
                            icon: "fa-clock",
                            summary: "شظية صلبة من بلورة برج ساعة قديم انهار منذ قرون.",
                            story: "كانت تتوسط مدينة ضائعة تحت الغيم. البلورة تعكس ألوان الشفق باستمرار وتختزن داخلها أنغام جرس الساعة القديم.",
                            puzzle: "اللغز: كم عدد الثواني في دقيقة منسية؟",
                            choices: [
                                { text: "60 ثانية عادية", result: "إجابة تقليدية.. لكن البلورة أضاءت لتقديرك للزمن!", correct: true },
                                { text: "عدد لا ينتهي من اللحظات", result: "إجابة ساحرة! توهجت البلورة بضوء ناصع وكشفت حقيقتها.", correct: true }
                            ]
                        }
                    ]
                },
                letters: {
                    title: "جزيرة الرسائل العتيقة",
                    subtitle: "أرشيف الخطابات التي لم تصل أصحابها",
                    color: "atlas-accent",
                    items: [
                        {
                            id: "letter_1",
                            title: "رسالة الحبر الأزرق المختومة",
                            icon: "fa-envelope",
                            summary: "خطاب مطوي بخط يد أنيق مختوم بشمع أرجواني برّاق.",
                            story: "كتبت هذه الرسالة مسافرة على متن سفينة فضائية قديمة موجهة لأختها في الأرض، لكن الرسالة ضلت طريقها في أثير المجرّة لسنوات طويلة.",
                            puzzle: "قرار الخطاب:",
                            choices: [
                                { text: "كسر ختم الشمع وقراءة الرسالة", result: "فتحتِ الرسالة وقرأتِ: 'أنا بخير بين النجوم، لا تنسي ابتسامتكِ'.", correct: true },
                                { text: "ترك الرسالة مغلقة احتراماً لخصوصيتها", result: "تقديراً لأمانتكِ، أخرج الغلاف نورا متوهجاً وأحاطكِ بشارة الأمانة!", correct: true }
                            ]
                        },
                        {
                            id: "letter_2",
                            title: "خريطة ورقية محترقة الأطراف",
                            icon: "fa-map",
                            summary: "خريطة مرسومة على ورق بردي قديم بها رموز فلكية.",
                            story: "توضح موقع حدائق معلقة كانت توجد بين السحاب. الأجزاء المحترقة تخفي موقع الكنز الأخير.",
                            puzzle: "ماذا تعتقدين أن الكنز المكتوب في الخريطة؟",
                            choices: [
                                { text: "صندوق مملوء بالجواهر", result: "الجواهر حقيقية، ولكن الكنز الحقيقي كان الرحلة نفسها!", correct: true },
                                { text: "شجرة تثمر أوراقاً من ذهب", result: "بالفعل! نبتت ورقة ذهبية صغرى بداخل دفتر اكتشافاتكِ.", correct: true }
                            ]
                        },
                        {
                            id: "letter_3",
                            title: "بطاقة بريدية من مدينة زجاجية",
                            icon: "fa-box-archive",
                            summary: "بطاقة عليها صورة لمدينة مبنية بالكامل من البلور الصافي.",
                            story: "تاريخ البطاقة يرجع للعام 3022، مكتوب خلفها: 'المدينة لا تزال تشرق كل صباح، أتمنى لو كنتِ هنا معي'.",
                            puzzle: "اختاري الرد المناسب على البطاقة:",
                            choices: [
                                { text: "كتابة: 'أنا أسمعكِ عبر الأطلس'", result: "توهجت البطاقة البريدية واختفت الكلمات لتتحول إلى نغمة موسيقية.", correct: true }
                            ]
                        }
                    ]
                },
                mirrors: {
                    title: "جزيرة المرايا الغامضة",
                    subtitle: "انعكاسات لأحلام وأزمنة غابرة",
                    color: "atlas-rose",
                    items: [
                        {
                            id: "mirror_1",
                            title: "مرآة الإطار الفضي",
                            icon: "fa-wand-magic-sparkles",
                            summary: "مرآة بيضاوية ذات إطار محفور عليه صور أوراق الشجر.",
                            story: "عند النظر إليها لا ترين صورتك الحالية، بل ترين المكان الذي يتمنى قلبكِ زيارته في هذه اللحظة.",
                            puzzle: "ماذا ترين في انعكاس المرآة؟",
                            choices: [
                                { text: "منزلاً دافئاً وسماءً مليئة بالنجوم", result: "ظهر انعكاس منزلي دافئ وأضاءت المرآة باللون الذهبي!", correct: true },
                                { text: "حديقة سرية مليئة بالزهور المضيئة", result: "تفتحت زهرة نادرة داخل الانعكاس وأهدتكِ عطرها المنسي.", correct: true }
                            ]
                        },
                        {
                            id: "mirror_2",
                            title: "شظية الكريستال المتوهجة",
                            icon: "fa-gem",
                            summary: "قطعة زجاجية صغيرة كأنها سقطت من نجمة متهاوية.",
                            story: "تحتفظ بداخلها بآخر شعاع شمس أشرق على قارة مفقودة قبل ملايين السنين.",
                            puzzle: "كيف تحفظين بريق شظية الكريستال؟",
                            choices: [
                                { text: "إحاطتها بيدكِ برفق", result: "شعرتِ بطفو الكريستال وانعكاس ألوان الطيف في أرجاء الغرفة!", correct: true }
                            ]
                        },
                        {
                            id: "mirror_3",
                            title: "نظارة العدسة الملونة",
                            icon: "fa-glasses",
                            summary: "نظارة قديمة بنصف عدسة وردية ونصف ذهبية.",
                            story: "كان يرتديها رسام مجهول لرؤية الألوان الخفية التي لا يراها البشر العاديون في السماء.",
                            puzzle: "عند ارتداء النظارة الملونة، ماذا تكتشفين؟",
                            choices: [
                                { text: "خيالات الخريطة السرية للأطلس", result: "اكتشفتِ الرمز السري الذي يفتح الجزيرة الرابعة!", correct: true }
                            ]
                        }
                    ]
                },
                secret: {
                    title: "الجزيرة السرية: أرشيف النجوم",
                    subtitle: "المكان الذي تجمع فيه كل الأسرار المكتشفة",
                    color: "atlas-gold",
                    items: [
                        {
                            id: "secret_1",
                            title: "مفتاح النجمة المفقودة",
                            icon: "fa-key",
                            summary: "مفتاح متوهج مصنوع من ضوء النجوم الخالص.",
                            story: "هذا المفتاح هو أثمن ما في الأطلس. يفتح باباً لجميع الأحلام والقصص التي لم تُكتب بعد.",
                            puzzle: "ماذا تفتحين بمفتاح النجمة؟",
                            choices: [
                                { text: "فتح كتاب المستقبل المنسي", result: "أبرق الأطلس بكامله وأصبحتِ الحارسة الرسمية للأشياء المنسية!", correct: true }
                            ]
                        }
                    ]
                }
            },
            badges: [
                { id: "badge_first", title: "مكتشفة البداية", desc: "اكتشفي أول عنصر في الأطلس", icon: "fa-compass", req: 1 },
                { id: "badge_explorer", title: "رحالة الجزر", desc: "اكتشفي 5 عناصر مختلفة", icon: "fa-map-marked-alt", req: 5 },
                { id: "badge_master", title: "حارسة الأطلس", desc: "اكتشفي كافة العناصر (10 عناصر)", icon: "fa-crown", req: 10 },
                { id: "badge_puzzles", title: "حلّالة الألغاز", desc: "قومي بحل 3 ألغاز بنجاح", icon: "fa-puzzle-piece", reqPuzzle: 3 }
            ]
        };

        /* ================= STATE MANAGEMENT ================= */
        let userState = {
            discoveredItems: [], // IDs of discovered items
            solvedPuzzles: 0,
            activeIsland: 'clocks',
            unlockedSecret: false
        };

        /* ================= INITIALIZATION ================= */
        window.onload = function() {
            initStarCanvas();
            loadState();
            renderBadges();
            updateUI();

            // Set default view
            switchTab('gateway');
        };

        /* Save & Load LocalStorage */
        function saveState() {
            localStorage.setItem('atlas_user_state_2026', JSON.stringify(userState));
            updateUI();
        }

        function loadState() {
            const saved = localStorage.getItem('atlas_user_state_2026');
            if (saved) {
                try {
                    userState = JSON.parse(saved);
                } catch(e) { console.error("Error loading state", e); }
            }
        }

        function resetProgress() {
            if (confirm("هل أنتِ متأكدة من إعادة ضبط جميع الاكتشافات والبدء من جديد؟")) {
                userState = {
                    discoveredItems: [],
                    solvedPuzzles: 0,
                    activeIsland: 'clocks',
                    unlockedSecret: false
                };
                saveState();
                switchTab('map');
            }
        }

        /* ================= NAVIGATION & TAB SWITCHING ================= */
        function switchTab(tabId) {
            const pages = ['gateway', 'map', 'room', 'journal'];
            pages.forEach(p => {
                const el = document.getElementById(`page-${p}`);
                const nav = document.getElementById(`nav-${p}`);
                if (p === tabId) {
                    el.classList.remove('hidden');
                    if (nav) nav.classList.add('bg-atlas-accent/30', 'text-atlas-gold', 'border', 'border-atlas-gold/40');
                } else {
                    el.classList.add('hidden');
                    if (nav) nav.classList.remove('bg-atlas-accent/30', 'text-atlas-gold', 'border', 'border-atlas-gold/40');
                }
            });

            if (tabId === 'room') {
                renderRoomItems(userState.activeIsland);
            } else if (tabId === 'journal') {
                renderJournal();
            }
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function openIsland(islandKey) {
            if (islandKey === 'secret' && userState.discoveredItems.length < 5) {
                alert("الجزيرة السرية لا تزال مغلقة! اكتشفي 5 عناصر على الأقل لفتح بوابة النجوم.");
                return;
            }
            userState.activeIsland = islandKey;
            switchTab('room');
        }

        /* ================= RENDER ROOM & ITEMS ================= */
        function renderRoomItems(islandKey) {
            const island = ATLAS_DATA.islands[islandKey];
            if (!island) return;

            document.getElementById('room-title').innerText = island.title;
            document.getElementById('room-subtitle').innerText = island.subtitle;

            const grid = document.getElementById('room-items-grid');
            grid.innerHTML = '';

            island.items.forEach(item => {
                const isDiscovered = userState.discoveredItems.includes(item.id);
                const card = document.createElement('div');
                card.className = `glass-card p-6 rounded-2xl relative overflow-hidden cursor-pointer flex flex-col justify-between group ${isDiscovered ? 'border-atlas-gold/60' : ''}`;
                card.onclick = (e) => triggerItemDiscovery(e, item, islandKey);

                card.innerHTML = `
                    <div>
                        <div class="flex justify-between items-center mb-4">
                            <div class="w-12 h-12 rounded-xl ${isDiscovered ? 'bg-atlas-gold/20 text-atlas-gold border-atlas-gold' : 'bg-atlas-card text-gray-400 border-gray-600'} border flex items-center justify-center text-xl group-hover:scale-110 transition-transform">
                                <i class="fa-solid ${item.icon}"></i>
                            </div>
                            ${isDiscovered ? 
                                '<span class="text-[10px] bg-atlas-gold/20 text-atlas-gold px-2.5 py-1 rounded-full border border-atlas-gold/40 font-bold"><i class="fa-solid fa-check me-1"></i>مكتشف</span>' : 
                                '<span class="text-[10px] bg-atlas-accent/20 text-atlas-glow px-2.5 py-1 rounded-full border border-atlas-accent/30"><i class="fa-solid fa-sparkles me-1"></i>انقري للاكتشاف</span>'
                            }
                        </div>
                        <h4 class="text-lg font-bold text-white mb-2 group-hover:text-atlas-gold transition-colors">${item.title}</h4>
                        <p class="text-xs text-gray-300 font-tajawal leading-relaxed mb-4">${item.summary}</p>
                    </div>
                    <div class="pt-3 border-t border-atlas-accent/10 flex items-center justify-between text-xs text-atlas-glow">
                        <span>${isDiscovered ? 'عرض الحكاية واللغز' : 'استكشاف الأثر'}</span>
                        <i class="fa-solid fa-hand-pointer group-hover:translate-y-1 transition-transform"></i>
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        /* Sparkle Particle Trigger on Click */
        function triggerItemDiscovery(e, item, islandKey) {
            // Particle effect
            const rect = e.currentTarget.getBoundingClientRect();
            createParticles(rect.left + rect.width/2, rect.top + rect.height/2);

            openItemModal(item);
        }

        function createParticles(x, y) {
            for (let i = 0; i < 16; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                const size = Math.random() * 8 + 4;
                particle.style.width = `${size}px`;
                particle.style.height = `${size}px`;
                particle.style.background = Math.random() > 0.5 ? '#ffb703' : '#9d4edd';
                particle.style.boxShadow = '0 0 10px currentColor';
                particle.style.left = `${x}px`;
                particle.style.top = `${y}px`;

                const angle = Math.random() * Math.PI * 2;
                const dist = Math.random() * 80 + 20;
                particle.style.setProperty('--dx', `${Math.cos(angle) * dist}px`);
                particle.style.setProperty('--dy', `${Math.sin(angle) * dist}px`);

                document.body.appendChild(particle);
                setTimeout(() => particle.remove(), 800);
            }
        }

        /* Modal Details & Puzzles */
        function openItemModal(item) {
            const modal = document.getElementById('item-modal');
            document.getElementById('modal-title').innerText = item.title;
            document.getElementById('modal-story').innerText = item.story;
            document.getElementById('modal-icon').className = `fa-solid ${item.icon}`;

            const choicesBox = document.getElementById('modal-choices');
            const puzzleQuestion = document.getElementById('modal-puzzle-question');
            const resultBox = document.getElementById('modal-result');

            choicesBox.innerHTML = '';
            resultBox.classList.add('hidden');
            puzzleQuestion.innerText = item.puzzle || "ما هو تفاعلكِ مع هذا الشيء المنسي؟";

            item.choices.forEach(choice => {
                const btn = document.createElement('button');
                btn.className = "w-full text-right p-3 rounded-xl bg-atlas-card border border-atlas-accent/30 hover:border-atlas-gold text-xs text-gray-200 hover:text-white transition-all flex items-center justify-between";
                btn.innerHTML = `<span>${choice.text}</span> <i class="fa-solid fa-wand-sparkles text-atlas-gold"></i>`;
                btn.onclick = () => {
                    resultBox.innerText = choice.result;
                    resultBox.classList.remove('hidden');

                    // Save discovery & puzzle count
                    if (!userState.discoveredItems.includes(item.id)) {
                        userState.discoveredItems.push(item.id);
                    }
                    userState.solvedPuzzles += 1;
                    saveState();
                    renderRoomItems(userState.activeIsland);
                };
                choicesBox.appendChild(btn);
            });

            // Mark as discovered even if just opened
            if (!userState.discoveredItems.includes(item.id)) {
                userState.discoveredItems.push(item.id);
                saveState();
            }

            modal.classList.remove('hidden');
        }

        function closeItemModal() {
            document.getElementById('item-modal').classList.add('hidden');
        }

        /* ================= RENDER JOURNAL & BADGES ================= */
        function renderJournal() {
            const collectedCount = userState.discoveredItems.length;
            const totalItems = 10;
            const percent = Math.min(100, Math.round((collectedCount / totalItems) * 100));

            document.getElementById('stat-collected').innerText = `${collectedCount} / ${totalItems}`;
            document.getElementById('stat-puzzles').innerText = userState.solvedPuzzles;
            document.getElementById('stat-progress').innerText = `${percent}%`;
            document.getElementById('progress-text').innerText = `${percent}%`;
            document.getElementById('progress-bar').style.width = `${percent}%`;

            // Badges
            renderBadges();

            // Render Items Archive
            const archiveGrid = document.getElementById('journal-items-grid');
            archiveGrid.innerHTML = '';

            if (collectedCount === 0) {
                archiveGrid.innerHTML = `
                    <div class="col-span-full text-center py-8 glass-panel rounded-2xl text-gray-400 font-tajawal">
                        <i class="fa-solid fa-box-open text-4xl text-atlas-accent mb-2"></i>
                        <p>لم تكتشفي أي شيء بعد.. ابدئي رحلتكِ من الخريطة!</p>
                    </div>
                `;
                return;
            }

            // Find all discovered item objects
            Object.keys(ATLAS_DATA.islands).forEach(key => {
                ATLAS_DATA.islands[key].items.forEach(item => {
                    if (userState.discoveredItems.includes(item.id)) {
                        const card = document.createElement('div');
                        card.className = "glass-panel p-4 rounded-xl flex items-start gap-4 border border-atlas-gold/30";
                        card.innerHTML = `
                            <div class="w-10 h-10 rounded-lg bg-atlas-gold/20 text-atlas-gold flex items-center justify-center text-lg flex-shrink-0">
                                <i class="fa-solid ${item.icon}"></i>
                            </div>
                            <div>
                                <h5 class="text-sm font-bold text-white mb-1">${item.title}</h5>
                                <p class="text-xs text-gray-300 font-tajawal leading-relaxed">${item.summary}</p>
                            </div>
                        `;
                        archiveGrid.appendChild(card);
                    }
                });
            });
        }

        function renderBadges() {
            const container = document.getElementById('badges-container');
            if (!container) return;
            container.innerHTML = '';

            const collectedCount = userState.discoveredItems.length;
            let unlockedBadgesCount = 0;

            ATLAS_DATA.badges.forEach(badge => {
                let isUnlocked = false;
                if (badge.req && collectedCount >= badge.req) isUnlocked = true;
                if (badge.reqPuzzle && userState.solvedPuzzles >= badge.reqPuzzle) isUnlocked = true;

                if (isUnlocked) unlockedBadgesCount++;

                const bEl = document.createElement('div');
                bEl.className = `glass-card p-4 rounded-xl text-center border ${isUnlocked ? 'border-atlas-gold/60 bg-atlas-gold/10' : 'border-gray-800 opacity-50'}`;
                bEl.innerHTML = `
                    <div class="w-10 h-10 mx-auto rounded-full ${isUnlocked ? 'bg-atlas-gold/20 text-atlas-gold' : 'bg-atlas-card text-gray-600'} flex items-center justify-center text-lg mb-2">
                        <i class="fa-solid ${badge.icon}"></i>
                    </div>
                    <h5 class="text-xs font-bold text-white mb-1">${badge.title}</h5>
                    <p class="text-[10px] text-gray-400 font-tajawal">${badge.desc}</p>
                `;
                container.appendChild(bEl);
            });

            document.getElementById('stat-badges').innerText = `${unlockedBadgesCount} / ${ATLAS_DATA.badges.length}`;
        }

        /* Update Global UI indicators */
        function updateUI() {
            // Check Island completion counts
            Object.keys(ATLAS_DATA.islands).forEach(key => {
                const badge = document.getElementById(`badge-island-${key}`);
                if (badge) {
                    const total = ATLAS_DATA.islands[key].items.length;
                    const found = ATLAS_DATA.islands[key].items.filter(i => userState.discoveredItems.includes(i.id)).length;
                    badge.innerText = `${found} / ${total} مكتشف`;
                }
            });

            // Check Secret Island Status
            const secretCard = document.getElementById('secret-island-card');
            if (secretCard) {
                if (userState.discoveredItems.length >= 5) {
                    document.getElementById('secret-icon').className = "fa-solid fa-key text-atlas-gold";
                    document.getElementById('secret-badge').innerText = "مفتوحة الآن!";
                    document.getElementById('secret-badge').className = "text-[10px] bg-green-500/20 text-green-400 px-2 py-0.5 rounded border border-green-500/40";
                    document.getElementById('secret-desc').innerText = "انقري للدخول إلى أرشيف النجوم السري المنسي.";
                    document.getElementById('secret-btn').innerText = "دخول الجزيرة السرية";
                    document.getElementById('secret-btn').className = "px-5 py-2 rounded-xl bg-atlas-gold text-atlas-bg font-bold text-xs shadow-[0_0_15px_rgba(255,183,3,0.5)]";
                }
            }

            // Journal indicator badge
            const journalBadge = document.getElementById('journalBadge');
            if (journalBadge && userState.discoveredItems.length > 0) {
                journalBadge.classList.remove('hidden');
            }
        }

        /* ================= STAR CANVAS BACKGROUND ================= */
        function initStarCanvas() {
            const canvas = document.getElementById('starCanvas');
            const ctx = canvas.getContext('2d');
            let width, height, stars = [];

            function resize() {
                width = canvas.width = window.innerWidth;
                height = canvas.height = window.innerHeight;
                stars = [];
                for (let i = 0; i < 120; i++) {
                    stars.push({
                        x: Math.random() * width,
                        y: Math.random() * height,
                        radius: Math.random() * 1.8 + 0.5,
                        alpha: Math.random(),
                        speed: Math.random() * 0.01 + 0.005,
                        color: Math.random() > 0.4 ? '#ffffff' : (Math.random() > 0.5 ? '#9d4edd' : '#ffb703')
                    });
                }
            }

            function animate() {
                ctx.clearRect(0, 0, width, height);
                stars.forEach(star => {
                    star.alpha += star.speed;
                    if (star.alpha > 1 || star.alpha < 0) star.speed = -star.speed;

                    ctx.beginPath();
                    ctx.arc(star.x, star.y, star.radius, 0, Math.PI * 2);
                    ctx.fillStyle = star.color;
                    ctx.globalAlpha = Math.abs(star.alpha);
                    ctx.shadowBlur = star.radius * 4;
                    ctx.shadowColor = star.color;
                    ctx.fill();
                });
                requestAnimationFrame(animate);
            }

            window.addEventListener('resize', resize);
            resize();
            animate();
        }
    </script>
</body>
</html>
