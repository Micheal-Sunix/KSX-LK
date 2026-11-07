
<!DOCTYPE html>
<html lang="en" class="h-full bg-slate-950 text-slate-100 dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KSX - Kabwe Security Exchange | Next-Gen Trading Terminal</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        mono: ['JetBrains Mono', 'monospace'],
                    },
                    colors: {
                        brand: {
                            50: '#eefdf5',
                            400: '#34d399',
                            500: '#10b981',
                            600: '#059669',
                            700: '#047857',
                            900: '#064e3b',
                        },
                        ksx: {
                            bg: '#020617',
                            card: '#0f172a',
                            border: '#1e293b',
                            accent: '#10b981',
                            red: '#f43f5e',
                            amber: '#f59e0b',
                            cyan: '#06b6d4',
                            indigo: '#6366f1',
                            purple: '#8b5cf6'
                        }
                    },
                    boxShadow: {
                        'glow-emerald': '0 0 25px -2px rgba(16, 185, 129, 0.35)',
                        'glow-indigo': '0 0 25px -2px rgba(99, 102, 241, 0.35)',
                        'glow-cyan': '0 0 25px -2px rgba(6, 182, 212, 0.35)',
                        'glow-rose': '0 0 25px -2px rgba(244, 63, 94, 0.35)',
                        'glow-amber': '0 0 25px -2px rgba(245, 158, 11, 0.35)',
                    }
                }
            }
        }
    </script>
    <!-- Lucide Icons CDN -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts Inter & JetBrains Mono -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #020617; }
        ::-webkit-scrollbar { width: 5px; height: 5px; }
        ::-webkit-scrollbar-track { background: #020617; }
        ::-webkit-scrollbar-thumb { background: #1e293b; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #334155; }
        .glass-panel {
            background: rgba(15, 23, 42, 0.75);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(30, 41, 59, 0.8);
        }
        .glass-nav {
            background: rgba(2, 6, 23, 0.85);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
        }
        @keyframes flashGreen {
            0% { background-color: rgba(16, 185, 129, 0.35); }
            100% { background-color: transparent; }
        }
        @keyframes flashRed {
            0% { background-color: rgba(244, 63, 94, 0.35); }
            100% { background-color: transparent; }
        }
        .flash-up { animation: flashGreen 0.7s ease-out; }
        .flash-down { animation: flashRed 0.7s ease-out; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="h-full flex flex-col min-h-screen bg-slate-950 text-slate-100 antialiased selection:bg-emerald-500 selection:text-slate-950">

    <!-- TOP ANNOUNCEMENT & REAL-TIME TICKER STRIP -->
    <div class="bg-gradient-to-r from-emerald-950 via-slate-950 to-slate-950 text-emerald-300 text-xs py-1.5 px-4 border-b border-emerald-900/40 flex flex-wrap justify-between items-center gap-2">
        <div class="flex items-center gap-3">
            <span class="inline-flex items-center gap-1.5 px-2.5 py-0.5 rounded-full bg-emerald-500/10 text-emerald-400 font-mono text-[10px] border border-emerald-500/20 font-bold">
                <span class="w-2 h-2 rounded-full bg-emerald-400 animate-ping"></span> SEC ZAMBIA LICENSED
            </span>
            <span class="font-semibold text-slate-300 hidden sm:inline text-[11px]">Kàbwe Security Exchange Order Matching Engine</span>
        </div>
        <!-- Dynamic Running Ticker Summary -->
        <div class="hidden lg:flex items-center gap-4 text-[11px] font-mono overflow-x-auto no-scrollbar" id="topMiniTicker">
            <!-- Dynamic ticker items generated via JS -->
        </div>
        <div id="liveTime" class="font-mono text-emerald-400/90 text-[11px] font-bold">00:00:00 CAT</div>
    </div>

    <!-- MAIN HEADER NAVIGATION -->
    <header class="glass-nav border-b border-slate-800/80 sticky top-0 z-40">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between gap-4">
            <!-- Brand Logo & Badge -->
            <div class="flex items-center gap-3 cursor-pointer" onclick="switchTab('markets')">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-emerald-600 via-teal-500 to-amber-400 flex items-center justify-center shadow-glow-emerald font-black text-slate-950 text-xl tracking-tighter">
                    KSX
                </div>
                <div>
                    <h1 class="text-base font-extrabold tracking-tight text-white flex items-center gap-1.5">
                        Kàbwe Security Exchange
                        <span class="bg-amber-500/10 text-amber-400 border border-amber-500/30 text-[9px] px-1.5 py-0.5 rounded font-bold uppercase tracking-wider">SEC APPROVED</span>
                    </h1>
                    <p class="text-[11px] text-slate-400 font-medium">Central Zambia & National Equities Hub</p>
                </div>
            </div>

            <!-- Navigation Tabs -->
            <nav class="hidden lg:flex items-center gap-1 bg-slate-900/90 p-1.5 rounded-xl border border-slate-800 text-xs font-semibold">
                <button onclick="switchTab('markets')" id="nav-markets" class="px-3.5 py-1.5 rounded-lg text-slate-300 hover:text-white hover:bg-slate-800 transition flex items-center gap-1.5">
                    <i data-lucide="trending-up" class="w-4 h-4 text-emerald-400"></i> Markets
                </button>
                <button onclick="switchTab('trade')" id="nav-trade" class="px-3.5 py-1.5 rounded-lg text-slate-300 hover:text-white hover:bg-slate-800 transition flex items-center gap-1.5">
                    <i data-lucide="candlestick-chart" class="w-4 h-4 text-teal-400"></i> Terminal
                </button>
                <button onclick="switchTab('portfolio')" id="nav-portfolio" class="px-3.5 py-1.5 rounded-lg text-slate-300 hover:text-white hover:bg-slate-800 transition flex items-center gap-1.5">
                    <i data-lucide="wallet" class="w-4 h-4 text-amber-400"></i> Portfolio
                </button>
                <button onclick="switchTab('news')" id="nav-news" class="px-3.5 py-1.5 rounded-lg text-slate-300 hover:text-white hover:bg-slate-800 transition flex items-center gap-1.5">
                    <i data-lucide="newspaper" class="w-4 h-4 text-cyan-400"></i> Financial News
                </button>
                <button onclick="switchTab('community')" id="nav-community" class="px-3.5 py-1.5 rounded-lg text-slate-300 hover:text-white hover:bg-slate-800 transition flex items-center gap-1.5">
                    <i data-lucide="message-square" class="w-4 h-4 text-indigo-400"></i> Trader Hub
                </button>
                <button id="admin-nav-btn" onclick="switchTab('admin')" class="hidden px-3 py-1.5 rounded-lg text-rose-400 hover:bg-rose-500/10 transition flex items-center gap-1.5 border border-rose-500/30">
                    <i data-lucide="shield-alert" class="w-4 h-4"></i> Admin
                </button>
            </nav>

            <!-- User Balance & Auth Header Section -->
            <div class="flex items-center gap-3">
                <div id="userHeaderBar" class="hidden flex items-center gap-3 bg-slate-900/90 border border-slate-700/60 rounded-xl px-3.5 py-1.5">
                    <div class="text-right">
                        <div class="text-[10px] uppercase tracking-wider text-slate-400 font-medium">ZMW Cash</div>
                        <div id="headerBalance" class="text-xs sm:text-sm font-bold text-emerald-400 font-mono">ZMW 0.00</div>
                    </div>
                    <div class="h-6 w-[1px] bg-slate-800"></div>
                    <div class="flex items-center gap-2">
                        <div id="userAvatar" class="w-8 h-8 rounded-lg bg-emerald-500/20 text-emerald-300 font-bold flex items-center justify-center text-xs border border-emerald-500/30">
                            TR
                        </div>
                        <div class="hidden sm:block text-left">
                            <div id="userName" class="text-xs font-bold text-white leading-tight">Trader</div>
                            <div id="userRoleBadge" class="text-[9px] text-emerald-400 uppercase font-semibold">Verified</div>
                        </div>
                        <button onclick="handleLogout()" class="ml-1 text-slate-400 hover:text-rose-400 p-1 transition" title="Logout">
                            <i data-lucide="log-out" class="w-4 h-4"></i>
                        </button>
                    </div>
                </div>

                <div id="authHeaderBtns" class="flex items-center gap-2">
                    <button onclick="openAuthModal('login')" class="px-3.5 py-1.5 text-xs font-semibold text-slate-300 hover:text-white bg-slate-900 hover:bg-slate-800 border border-slate-700 rounded-xl transition">
                        Log In
                    </button>
                    <button onclick="openAuthModal('register')" class="px-3.5 py-1.5 text-xs font-bold text-slate-950 bg-emerald-400 hover:bg-emerald-300 rounded-xl shadow-glow-emerald transition">
                        Register
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- MOBILE NAVIGATION DOCK -->
    <div class="lg:hidden fixed bottom-0 left-0 right-0 z-40 bg-slate-950/95 border-t border-slate-800 flex justify-around p-2 text-[10px] font-semibold backdrop-blur-md">
        <button onclick="switchTab('markets')" class="flex flex-col items-center gap-1 text-slate-400 hover:text-emerald-400 py-1">
            <i data-lucide="trending-up" class="w-5 h-5"></i>
            <span>Markets</span>
        </button>
        <button onclick="switchTab('trade')" class="flex flex-col items-center gap-1 text-slate-400 hover:text-emerald-400 py-1">
            <i data-lucide="candlestick-chart" class="w-5 h-5"></i>
            <span>Terminal</span>
        </button>
        <button onclick="switchTab('portfolio')" class="flex flex-col items-center gap-1 text-slate-400 hover:text-emerald-400 py-1">
            <i data-lucide="wallet" class="w-5 h-5"></i>
            <span>Portfolio</span>
        </button>
        <button onclick="switchTab('news')" class="flex flex-col items-center gap-1 text-slate-400 hover:text-cyan-400 py-1">
            <i data-lucide="newspaper" class="w-5 h-5"></i>
            <span>News</span>
        </button>
        <button onclick="switchTab('community')" class="flex flex-col items-center gap-1 text-slate-400 hover:text-indigo-400 py-1">
            <i data-lucide="message-square" class="w-5 h-5"></i>
            <span>Articles</span>
        </button>
        <button id="mobile-admin-btn" onclick="switchTab('admin')" class="hidden flex flex-col items-center gap-1 text-rose-400 py-1">
            <i data-lucide="shield-alert" class="w-5 h-5"></i>
            <span>Admin</span>
        </button>
    </div>

    <!-- TOAST NOTIFICATION CONTAINER -->
    <div id="toastContainer" class="fixed top-20 right-4 z-50 flex flex-col gap-2 max-w-sm w-full pointer-events-none"></div>

    <!-- MAIN APP CONTAINER -->
    <main class="flex-1 max-w-7xl w-full mx-auto p-4 sm:p-6 pb-24 lg:pb-6">

        <!-- TAB 1: MARKETS DIRECTORY -->
        <section id="tab-markets" class="space-y-6">
            <!-- Hero Market Banner -->
            <div class="relative overflow-hidden rounded-3xl glass-panel border border-emerald-500/20 p-6 sm:p-8">
                <div class="relative z-10 max-w-3xl">
                    <span class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-bold bg-emerald-500/10 text-emerald-400 border border-emerald-500/30 mb-3">
                        <i data-lucide="shield-check" class="w-3.5 h-3.5"></i> SEC Regulated Security Exchange
                    </span>
                    <h2 class="text-2xl sm:text-4xl font-black text-white tracking-tight leading-tight">
                        Trade Central Zambia & National Equities
                    </h2>
                    <p class="mt-2 text-xs sm:text-sm text-slate-300 leading-relaxed">
                        Instant execution for FVG Milling, LDC Local Construction, Chamo Milling, Kabwe Mall Holdings, Power Tools Logistics, ZCCM-IH, ZANACO Bank, and Airtel Zambia.
                    </p>
                    <div class="mt-5 flex flex-wrap items-center gap-3">
                        <button onclick="switchTab('trade')" class="px-5 py-2.5 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-extrabold text-xs sm:text-sm rounded-xl shadow-glow-emerald transition flex items-center gap-2">
                            Launch Terminal <i data-lucide="arrow-right" class="w-4 h-4"></i>
                        </button>
                        <button onclick="openDepositModal()" class="px-5 py-2.5 bg-slate-900 hover:bg-slate-800 text-slate-200 border border-slate-700 text-xs sm:text-sm font-bold rounded-xl transition flex items-center gap-2">
                            <i data-lucide="smartphone" class="w-4 h-4 text-emerald-400"></i> Mobile Money Deposit
                        </button>
                    </div>
                </div>
                <div class="absolute -right-12 -bottom-12 opacity-10 pointer-events-none">
                    <i data-lucide="building-2" class="w-96 h-96 text-white"></i>
                </div>
            </div>

            <!-- Market Directory Filters -->
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
                <div>
                    <h3 class="text-lg font-bold text-white flex items-center gap-2">
                        Directory of Listed Equities
                        <span id="marketAssetCount" class="text-xs bg-slate-900 px-2.5 py-0.5 rounded-md text-emerald-400 font-mono border border-slate-800">8 Securities</span>
                    </h3>
                    <p class="text-xs text-slate-400">Real-time spot prices in Zambian Kwacha (ZMW)</p>
                </div>
                <div class="flex items-center gap-2 w-full sm:w-auto">
                    <div class="relative flex-1 sm:w-72">
                        <i data-lucide="search" class="w-4 h-4 absolute left-3 top-2.5 text-slate-400"></i>
                        <input type="text" id="marketSearchInput" onkeyup="filterMarkets()" placeholder="Search symbol or security..." class="w-full bg-slate-900 border border-slate-800 text-xs text-white pl-9 pr-3 py-2 rounded-xl focus:outline-none focus:border-emerald-500">
                    </div>
                </div>
            </div>

            <!-- Asset Cards Grid -->
            <div id="marketGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5">
                <!-- JS dynamic injection -->
            </div>
        </section>

        <!-- TAB 2: TRADING TERMINAL & HIGH-PERFORMANCE CHART ENGINE -->
        <section id="tab-trade" class="hidden space-y-4">
            <!-- Terminal Header Bar -->
            <div class="glass-panel rounded-2xl p-4 flex flex-wrap items-center justify-between gap-4">
                <div class="flex items-center gap-4">
                    <select id="assetSelector" onchange="selectAsset(this.value)" class="bg-slate-950 border border-slate-700 text-white text-sm font-bold rounded-xl px-4 py-2 focus:ring-2 focus:ring-emerald-500 outline-none cursor-pointer">
                        <!-- JS populated -->
                    </select>
                    <div>
                        <div id="terminalPrice" class="text-xl sm:text-3xl font-black font-mono text-emerald-400">ZMW 0.00</div>
                        <div id="terminalChange" class="text-xs font-semibold text-emerald-400 flex items-center gap-1">+0.00% Today</div>
                    </div>
                </div>

                <div class="grid grid-cols-2 sm:grid-cols-4 gap-6 text-xs border-l border-slate-800 pl-4">
                    <div>
                        <span class="text-slate-400 block text-[10px] uppercase font-medium">24h High</span>
                        <span id="statHigh" class="font-mono text-slate-200 font-bold text-xs sm:text-sm">ZMW 0.00</span>
                    </div>
                    <div>
                        <span class="text-slate-400 block text-[10px] uppercase font-medium">24h Low</span>
                        <span id="statLow" class="font-mono text-slate-200 font-bold text-xs sm:text-sm">ZMW 0.00</span>
                    </div>
                    <div>
                        <span class="text-slate-400 block text-[10px] uppercase font-medium">24h Volume</span>
                        <span id="statVol" class="font-mono text-slate-200 font-bold text-xs sm:text-sm">0 SHARES</span>
                    </div>
                    <div>
                        <span class="text-slate-400 block text-[10px] uppercase font-medium">Market Cap</span>
                        <span id="statCap" class="font-mono text-slate-200 font-bold text-xs sm:text-sm">ZMW 0M</span>
                    </div>
                </div>
            </div>

            <!-- Terminal Grid: Custom Canvas Chart + Orderbook + Execution -->
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-4">
                
                <!-- Custom Chart Canvas (8 Cols) -->
                <div class="lg:col-span-8 glass-panel rounded-2xl p-4 flex flex-col h-[550px]">
                    <div class="flex flex-wrap items-center justify-between mb-3 pb-2 border-b border-slate-800 gap-2">
                        <div class="flex items-center gap-2">
                            <i data-lucide="candlestick-chart" class="w-4 h-4 text-emerald-400"></i>
                            <span id="chartTitle" class="text-sm font-bold text-white">Security Price Chart</span>
                        </div>
                        
                        <div class="flex flex-wrap items-center gap-3">
                            <!-- CHART MODE TOGGLE -->
                            <div class="flex items-center gap-1 text-xs bg-slate-950 p-1 rounded-xl border border-slate-800">
                                <button onclick="setChartType('line')" id="chartTypeLine" class="px-3 py-1 rounded-lg bg-emerald-500 text-slate-950 font-bold text-[11px] flex items-center gap-1.5 transition">
                                    <i data-lucide="trending-up" class="w-3.5 h-3.5"></i> Line
                                </button>
                                <button onclick="setChartType('candlestick')" id="chartTypeCandle" class="px-3 py-1 rounded-lg text-slate-400 hover:text-white text-[11px] flex items-center gap-1.5 transition">
                                    <i data-lucide="candlestick-chart" class="w-3.5 h-3.5"></i> Candles
                                </button>
                            </div>

                            <!-- TECHNICAL INDICATOR TOGGLES -->
                            <div class="flex items-center gap-1 text-xs bg-slate-950 p-1 rounded-xl border border-slate-800">
                                <button onclick="toggleSMA(9)" id="sma9Btn" class="px-2.5 py-1 rounded-lg text-slate-400 hover:text-white text-[10px] font-mono transition">SMA-9</button>
                                <button onclick="toggleSMA(20)" id="sma20Btn" class="px-2.5 py-1 rounded-lg text-slate-400 hover:text-white text-[10px] font-mono transition">SMA-20</button>
                            </div>

                            <!-- TIMEFRAME SELECTOR -->
                            <div class="flex items-center gap-1 text-xs bg-slate-950 p-1 rounded-xl border border-slate-800">
                                <button onclick="changeTimeframe('1M')" id="tf-1M" class="px-2.5 py-1 rounded-lg bg-emerald-500 text-slate-950 font-bold text-[11px]">1M</button>
                                <button onclick="changeTimeframe('5M')" id="tf-5M" class="px-2.5 py-1 rounded-lg text-slate-400 hover:text-white text-[11px]">5M</button>
                                <button onclick="changeTimeframe('1H')" id="tf-1H" class="px-2.5 py-1 rounded-lg text-slate-400 hover:text-white text-[11px]">1H</button>
                                <button onclick="changeTimeframe('1D')" id="tf-1D" class="px-2.5 py-1 rounded-lg text-slate-400 hover:text-white text-[11px]">1D</button>
                            </div>
                        </div>
                    </div>
                    
                    <!-- HTML5 Custom Dynamic Canvas -->
                    <div class="flex-1 relative w-full h-full min-h-0 bg-slate-950/90 rounded-xl overflow-hidden border border-slate-800/80 p-2">
                        <canvas id="ksxChartCanvas" class="w-full h-full block"></canvas>
                    </div>
                </div>

                <!-- Orderbook & Order Ticket (4 Cols) -->
                <div class="lg:col-span-4 flex flex-col gap-4">
                    <!-- Order Book Card -->
                    <div class="glass-panel rounded-2xl p-4 flex flex-col h-[250px]">
                        <div class="text-xs font-bold text-slate-300 mb-2 flex justify-between items-center border-b border-slate-800 pb-2">
                            <span class="flex items-center gap-1.5"><i data-lucide="list" class="w-3.5 h-3.5 text-emerald-400"></i> ORDER BOOK DEPTH</span>
                            <span class="text-[10px] font-mono text-slate-500">SPREAD: 0.15%</span>
                        </div>
                        <div class="grid grid-cols-3 text-[10px] uppercase font-semibold text-slate-500 mb-1 px-1">
                            <span>Price (ZMW)</span>
                            <span class="text-right">Size</span>
                            <span class="text-right">Total</span>
                        </div>
                        <div id="orderbookAsks" class="space-y-0.5 text-xs font-mono overflow-y-auto flex-1 text-rose-400"></div>
                        <div id="orderbookCurrent" class="my-1 py-1 text-center bg-slate-950 border-y border-slate-800 text-xs font-bold font-mono text-emerald-400">
                            ZMW 0.00
                        </div>
                        <div id="orderbookBids" class="space-y-0.5 text-xs font-mono overflow-y-auto flex-1 text-emerald-400"></div>
                    </div>

                    <!-- Order Execution Form -->
                    <div class="glass-panel rounded-2xl p-4 flex flex-col justify-between flex-1">
                        <div>
                            <!-- Buy/Sell Tabs -->
                            <div class="grid grid-cols-2 gap-2 p-1 bg-slate-950 rounded-xl border border-slate-800 mb-3">
                                <button onclick="setOrderSide('BUY')" id="btnSideBuy" class="py-2 rounded-lg text-xs font-bold transition bg-emerald-500 text-slate-950 shadow-glow-emerald">
                                    BUY
                                </button>
                                <button onclick="setOrderSide('SELL')" id="btnSideSell" class="py-2 rounded-lg text-xs font-bold transition text-slate-400 hover:text-white">
                                    SELL
                                </button>
                            </div>

                            <!-- Order Type Selector -->
                            <div class="flex items-center justify-between text-xs text-slate-400 mb-2">
                                <span>Order Type:</span>
                                <div class="flex gap-2">
                                    <button onclick="setOrderType('limit')" id="typeLimitBtn" class="text-emerald-400 font-bold border-b border-emerald-400">Limit</button>
                                    <button onclick="setOrderType('market')" id="typeMarketBtn" class="text-slate-400 hover:text-white">Market</button>
                                </div>
                            </div>

                            <div class="space-y-3">
                                <div id="priceInputWrapper">
                                    <label class="block text-[11px] text-slate-400 font-medium mb-1">Limit Price (ZMW)</label>
                                    <input type="number" id="orderPrice" step="0.01" class="w-full bg-slate-950 border border-slate-800 text-white font-mono text-sm rounded-xl px-3 py-2 focus:outline-none focus:border-emerald-500">
                                </div>

                                <div>
                                    <label class="block text-[11px] text-slate-400 font-medium mb-1">Quantity (Shares)</label>
                                    <div class="relative">
                                        <input type="number" id="orderQty" min="1" value="10" oninput="calculateOrderTotal()" class="w-full bg-slate-950 border border-slate-800 text-white font-mono text-sm rounded-xl px-3 py-2 focus:outline-none focus:border-emerald-500">
                                        <span class="absolute right-3 top-2.5 text-xs text-slate-500 font-mono">SHARES</span>
                                    </div>
                                </div>

                                <!-- Quick Percentage Buttons -->
                                <div class="grid grid-cols-4 gap-1">
                                    <button onclick="setOrderPct(0.25)" class="py-1 bg-slate-950 hover:bg-slate-800 text-[10px] font-mono text-slate-400 rounded-lg border border-slate-800">25%</button>
                                    <button onclick="setOrderPct(0.50)" class="py-1 bg-slate-950 hover:bg-slate-800 text-[10px] font-mono text-slate-400 rounded-lg border border-slate-800">50%</button>
                                    <button onclick="setOrderPct(0.75)" class="py-1 bg-slate-950 hover:bg-slate-800 text-[10px] font-mono text-slate-400 rounded-lg border border-slate-800">75%</button>
                                    <button onclick="setOrderPct(1.00)" class="py-1 bg-slate-950 hover:bg-slate-800 text-[10px] font-mono text-slate-400 rounded-lg border border-slate-800">100%</button>
                                </div>

                                <div class="bg-slate-950 p-3 rounded-xl border border-slate-800 text-xs space-y-1.5 font-mono">
                                    <div class="flex justify-between text-slate-400">
                                        <span>Est. Total:</span>
                                        <span id="orderEstTotal" class="text-white font-bold">ZMW 0.00</span>
                                    </div>
                                    <div class="flex justify-between text-slate-400 text-[10px]">
                                        <span>Fee (0.1% SEC):</span>
                                        <span id="orderFeeVal" class="text-slate-400">ZMW 0.00</span>
                                    </div>
                                    <div class="flex justify-between text-slate-400 text-[10px]">
                                        <span>Available:</span>
                                        <span id="orderAvailCash" class="text-emerald-400 font-semibold">ZMW 0.00</span>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <button onclick="executeTrade()" id="btnSubmitOrder" class="w-full mt-3 py-3 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-extrabold rounded-xl shadow-glow-emerald text-xs uppercase tracking-wider transition">
                            Execute Buy Order
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 3: PORTFOLIO & WALLET -->
        <section id="tab-portfolio" class="hidden space-y-6">
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-5">
                <div class="glass-panel p-6 rounded-3xl flex items-center justify-between border border-emerald-500/20">
                    <div>
                        <div class="text-xs text-slate-400 font-semibold uppercase tracking-wider">Total Portfolio Value</div>
                        <div id="portTotalVal" class="text-2xl sm:text-3xl font-black text-white font-mono mt-1">ZMW 0.00</div>
                        <div class="text-[11px] text-emerald-400 font-semibold mt-1 flex items-center gap-1">
                            <i data-lucide="trending-up" class="w-3.5 h-3.5"></i> Holdings + ZMW Cash
                        </div>
                    </div>
                    <div class="w-12 h-12 rounded-2xl bg-emerald-500/10 border border-emerald-500/20 flex items-center justify-center text-emerald-400 shadow-glow-emerald">
                        <i data-lucide="pie-chart" class="w-6 h-6"></i>
                    </div>
                </div>

                <div class="glass-panel p-6 rounded-3xl flex items-center justify-between border border-amber-500/20">
                    <div>
                        <div class="text-xs text-slate-400 font-semibold uppercase tracking-wider">Available ZMW Balance</div>
                        <div id="portCashVal" class="text-2xl sm:text-3xl font-black text-emerald-400 font-mono mt-1">ZMW 0.00</div>
                        <div class="text-[11px] text-slate-400 mt-1">Ready for order execution</div>
                    </div>
                    <div class="w-12 h-12 rounded-2xl bg-amber-500/10 border border-amber-500/20 flex items-center justify-center text-amber-400">
                        <i data-lucide="banknote" class="w-6 h-6"></i>
                    </div>
                </div>

                <div class="glass-panel p-6 rounded-3xl flex flex-col justify-center gap-2.5">
                    <button onclick="openDepositModal()" class="w-full py-2.5 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-extrabold text-xs rounded-xl shadow-glow-emerald transition flex items-center justify-center gap-2">
                        <i data-lucide="arrow-down-left" class="w-4 h-4"></i> Mobile Money Deposit
                    </button>
                    <button onclick="openWithdrawModal()" class="w-full py-2.5 bg-slate-900 hover:bg-slate-800 text-slate-200 border border-slate-700 font-bold text-xs rounded-xl transition flex items-center justify-center gap-2">
                        <i data-lucide="arrow-up-right" class="w-4 h-4"></i> Withdraw Cash
                    </button>
                </div>
            </div>

            <!-- Asset Holdings Table -->
            <div class="glass-panel rounded-3xl p-6">
                <h3 class="text-base font-bold text-white mb-4 flex items-center gap-2">
                    <i data-lucide="layers" class="w-5 h-5 text-emerald-400"></i> Stock Portfolio Holdings
                </h3>
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-xs">
                        <thead>
                            <tr class="border-b border-slate-800 text-slate-400 font-semibold uppercase tracking-wider">
                                <th class="py-3 px-3">Symbol</th>
                                <th class="py-3 px-3">Security Name</th>
                                <th class="py-3 px-3 text-right">Shares Owned</th>
                                <th class="py-3 px-3 text-right">Spot Price</th>
                                <th class="py-3 px-3 text-right">Holding Value</th>
                                <th class="py-3 px-3 text-center">Action</th>
                            </tr>
                        </thead>
                        <tbody id="holdingsTableBody" class="divide-y divide-slate-800/60 font-mono">
                            <!-- Populated dynamically -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- TAB 4: CATEGORIZED FINANCIAL NEWS HUB -->
        <section id="tab-news" class="hidden space-y-6">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 glass-panel p-6 rounded-3xl border border-cyan-500/20">
                <div>
                    <h2 class="text-2xl font-black text-white flex items-center gap-2">
                        <i data-lucide="newspaper" class="w-6 h-6 text-cyan-400"></i> Financial News & Commodity Stream
                    </h2>
                    <p class="text-xs text-slate-400 mt-1">Live coverage of Zambian commercial news and international macro developments.</p>
                </div>
                <div class="flex flex-wrap items-center gap-2">
                    <button onclick="openPublishNewsModal()" class="px-4 py-2 rounded-xl text-xs font-bold bg-cyan-500 text-slate-950 shadow-glow-cyan flex items-center gap-1.5 transition">
                        <i data-lucide="plus-circle" class="w-4 h-4"></i> Post News Release
                    </button>
                    <button onclick="filterNews('ALL')" id="newsFilterALL" class="px-3.5 py-2 rounded-xl text-xs font-bold bg-emerald-500 text-slate-950 shadow-glow-emerald">All</button>
                    <button onclick="filterNews('LOCAL')" id="newsFilterLOCAL" class="px-3.5 py-2 rounded-xl text-xs font-bold bg-slate-900 text-slate-400 border border-slate-800">Local (Zambia)</button>
                    <button onclick="filterNews('INTERNATIONAL')" id="newsFilterINTL" class="px-3.5 py-2 rounded-xl text-xs font-bold bg-slate-900 text-slate-400 border border-slate-800">Global</button>
                    <button onclick="filterNews('MY_PORTFOLIO')" id="newsFilterPORT" class="px-3.5 py-2 rounded-xl text-xs font-bold bg-slate-900 text-slate-400 border border-slate-800">My Holdings</button>
                </div>
            </div>

            <div id="newsGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5">
                <!-- Dynamic News Cards -->
            </div>
        </section>

        <!-- TAB 5: COMMUNITY ARTICLES & RESEARCH HUB -->
        <section id="tab-community" class="hidden space-y-6">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 glass-panel p-6 rounded-3xl border border-indigo-500/20">
                <div>
                    <h2 class="text-2xl font-black text-white flex items-center gap-2">
                        <i data-lucide="message-square" class="w-6 h-6 text-indigo-400"></i> Trader Articles & Research Hub
                    </h2>
                    <p class="text-xs text-slate-300 mt-1">Publish stock thesis with mandatory image attachments and discuss market trends.</p>
                </div>
                <button onclick="openPublishArticleModal()" class="px-5 py-2.5 bg-indigo-500 hover:bg-indigo-400 text-slate-950 font-bold text-xs sm:text-sm rounded-xl shadow-glow-indigo transition flex items-center gap-2">
                    <i data-lucide="pen-tool" class="w-4 h-4"></i> Post Article with Image
                </button>
            </div>

            <div id="articlesList" class="space-y-5">
                <!-- Dynamic Articles -->
            </div>
        </section>

        <!-- TAB 6: ADMIN CONTROL PANEL -->
        <section id="tab-admin" class="hidden space-y-6">
            <div class="glass-panel border border-rose-800/40 rounded-3xl p-6 flex items-center justify-between">
                <div>
                    <h3 class="text-lg font-extrabold text-rose-300 flex items-center gap-2">
                        <i data-lucide="shield-check" class="w-5 h-5 text-rose-400"></i> KSX Market Management Console
                    </h3>
                    <p class="text-xs text-rose-300/70">Control security spot prices, inject cash balances, and monitor system metrics.</p>
                </div>
                <span class="px-3 py-1 bg-rose-500/20 text-rose-400 text-xs font-bold rounded-lg border border-rose-500/30">ADMINISTRATOR</span>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <!-- Price Override Card -->
                <div class="glass-panel p-6 rounded-3xl space-y-4">
                    <h4 class="text-sm font-bold text-white border-b border-slate-800 pb-2">Override Market Spot Prices</h4>
                    <div class="space-y-3">
                        <div>
                            <label class="block text-xs text-slate-400 mb-1">Select Security</label>
                            <select id="adminAssetSelect" class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-3">
                                <!-- JS Populated -->
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs text-slate-400 mb-1">New Spot Price (ZMW)</label>
                            <input type="number" step="0.01" id="adminNewPrice" placeholder="e.g. 18.50" class="w-full bg-slate-950 border border-slate-800 text-white font-mono text-xs rounded-xl p-3">
                        </div>
                        <button onclick="adminUpdatePrice()" class="w-full py-2.5 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold text-xs rounded-xl shadow-glow-emerald transition">
                            Apply Price Override
                        </button>
                    </div>
                </div>

                <!-- Cash Injection Card -->
                <div class="glass-panel p-6 rounded-3xl space-y-4">
                    <h4 class="text-sm font-bold text-white border-b border-slate-800 pb-2">Direct Account Balance Credit</h4>
                    <div class="space-y-3">
                        <div>
                            <label class="block text-xs text-slate-400 mb-1">Target Trader Account</label>
                            <select id="adminUserSelect" class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-3">
                                <!-- JS Populated -->
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs text-slate-400 mb-1">Amount to Credit (ZMW)</label>
                            <input type="number" id="adminInjectAmount" placeholder="e.g. 10000" class="w-full bg-slate-950 border border-slate-800 text-white font-mono text-xs rounded-xl p-3">
                        </div>
                        <button onclick="adminInjectCash()" class="w-full py-2.5 bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold text-xs rounded-xl transition">
                            Credit Account Balance
                        </button>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- AUTHENTICATION MODAL -->
    <div id="authModal" class="hidden fixed inset-0 z-50 bg-slate-950/85 backdrop-blur-md flex items-center justify-center p-4">
        <div class="glass-panel border border-slate-800 w-full max-w-md rounded-3xl shadow-2xl overflow-hidden relative">
            <button onclick="closeAuthModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white p-1">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>
            <div class="flex border-b border-slate-800 bg-slate-950/60">
                <button onclick="setAuthTab('login')" id="authTabLogin" class="flex-1 py-3 text-sm font-bold text-center border-b-2 border-emerald-500 text-emerald-400">
                    Log In
                </button>
                <button onclick="setAuthTab('register')" id="authTabRegister" class="flex-1 py-3 text-sm font-bold text-center border-b-2 border-transparent text-slate-400 hover:text-slate-200">
                    Register Account
                </button>
            </div>
            <div class="p-6">
                <!-- LOGIN FORM -->
                <form id="formLogin" onsubmit="handleLoginSubmit(event)" class="space-y-4">
                    <div>
                        <label class="block text-xs text-slate-300 font-medium mb-1">Email Address</label>
                        <input type="email" id="loginEmail" required placeholder="trader@ksx.zm" class="w-full bg-slate-950 border border-slate-800 text-white text-sm rounded-xl p-3 focus:outline-none focus:border-emerald-500">
                    </div>
                    <div>
                        <label class="block text-xs text-slate-300 font-medium mb-1">Password</label>
                        <input type="password" id="loginPassword" required placeholder="••••••••" class="w-full bg-slate-950 border border-slate-800 text-white text-sm rounded-xl p-3 focus:outline-none focus:border-emerald-500">
                    </div>
                    <button type="submit" class="w-full py-3 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold rounded-xl shadow-glow-emerald text-sm transition">
                        Log In to Platform
                    </button>
                    <div class="pt-3 border-t border-slate-800 text-center">
                        <span class="text-[11px] text-slate-500 block mb-2 font-mono">QUICK DEMO ACCESS</span>
                        <div class="grid grid-cols-2 gap-2">
                            <button type="button" onclick="fillDemoCredentials('trader@ksx.zm', 'user123')" class="px-2 py-1.5 bg-slate-900 hover:bg-slate-800 text-xs text-slate-300 rounded-xl border border-slate-800">
                                Demo Trader
                            </button>
                            <button type="button" onclick="fillDemoCredentials('admin@ksx.zm', 'admin123')" class="px-2 py-1.5 bg-slate-900 hover:bg-slate-800 text-xs text-rose-300 rounded-xl border border-slate-800">
                                Demo Admin
                            </button>
                        </div>
                    </div>
                </form>

                <!-- REGISTER FORM -->
                <form id="formRegister" onsubmit="handleRegisterSubmit(event)" class="hidden space-y-3">
                    <div>
                        <label class="block text-xs text-slate-300 font-medium mb-1">Full Name</label>
                        <input type="text" id="regName" required placeholder="Chileshe Mulenga" class="w-full bg-slate-950 border border-slate-800 text-white text-sm rounded-xl p-2.5 focus:outline-none focus:border-emerald-500">
                    </div>
                    <div>
                        <label class="block text-xs text-slate-300 font-medium mb-1">Email Address</label>
                        <input type="email" id="regEmail" required placeholder="chileshe@example.zm" class="w-full bg-slate-950 border border-slate-800 text-white text-sm rounded-xl p-2.5 focus:outline-none focus:border-emerald-500">
                    </div>
                    <div>
                        <label class="block text-xs text-slate-300 font-medium mb-1">NRC / Passport Number</label>
                        <input type="text" id="regNrc" required placeholder="123456/10/1" class="w-full bg-slate-950 border border-slate-800 text-white text-sm rounded-xl p-2.5 focus:outline-none focus:border-emerald-500">
                    </div>
                    <div>
                        <label class="block text-xs text-slate-300 font-medium mb-1">Password</label>
                        <input type="password" id="regPassword" required minlength="6" placeholder="At least 6 characters" class="w-full bg-slate-950 border border-slate-800 text-white text-sm rounded-xl p-2.5 focus:outline-none focus:border-emerald-500">
                    </div>
                    <div class="p-2.5 bg-emerald-500/10 border border-emerald-500/20 rounded-xl text-emerald-300 text-[11px] flex items-center gap-2">
                        <i data-lucide="gift" class="w-4 h-4 shrink-0 text-emerald-400"></i>
                        <span>Includes <strong>ZMW 5,000.00</strong> starting balance!</span>
                    </div>
                    <button type="submit" class="w-full py-3 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold rounded-xl shadow-glow-emerald text-sm transition">
                        Register Account
                    </button>
                </form>
            </div>
        </div>
    </div>

    <!-- ARTICLE PUBLISH MODAL (ENFORCED IMAGE ATTACHMENT) -->
    <div id="publishArticleModal" class="hidden fixed inset-0 z-50 bg-slate-950/85 backdrop-blur-md flex items-center justify-center p-4">
        <div class="glass-panel border border-slate-800 w-full max-w-xl rounded-3xl shadow-2xl p-6 relative max-h-[90vh] overflow-y-auto">
            <button onclick="closePublishArticleModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>
            <h3 class="text-lg font-bold text-white flex items-center gap-2 mb-1">
                <i data-lucide="pen-tool" class="w-5 h-5 text-indigo-400"></i> Post Market Research Article
            </h3>
            <p class="text-xs text-slate-400 mb-4">Must include both body analysis and a featured image attachment.</p>
            <form onsubmit="handlePublishArticle(event)" class="space-y-4">
                <div>
                    <label class="block text-xs text-slate-300 mb-1">Article Title *</label>
                    <input type="text" id="articleTitle" required placeholder="e.g. FVG Milling Expansion Drive & Q4 Earnings Thesis" class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-3 focus:outline-none focus:border-indigo-500">
                </div>
                
                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs text-slate-300 mb-1">Security Tag *</label>
                        <select id="articleAssetTag" class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-3">
                            <!-- Populated via JS -->
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-300 mb-1">Category *</label>
                        <select id="articleCategory" class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-3">
                            <option value="Fundamental Analysis">Fundamental Analysis</option>
                            <option value="Agribusiness & Milling">Agribusiness & Milling</option>
                            <option value="Infrastructure">Infrastructure</option>
                            <option value="Macroeconomics">Macroeconomics</option>
                            <option value="Commodities">Commodities</option>
                        </select>
                    </div>
                </div>

                <!-- Drag and Drop Image Box -->
                <div class="p-3 bg-slate-950/80 rounded-xl border border-indigo-500/30 space-y-2">
                    <label class="block text-xs font-bold text-indigo-300 flex items-center gap-1.5">
                        <i data-lucide="image" class="w-4 h-4"></i> Featured Image (Strictly Required) *
                    </label>
                    <div id="articleDropZone" class="border-2 border-dashed border-indigo-500/40 rounded-xl p-4 text-center cursor-pointer hover:border-indigo-400 transition" onclick="document.getElementById('articleImageFile').click()">
                        <i data-lucide="upload-cloud" class="w-8 h-8 mx-auto text-indigo-400 mb-1"></i>
                        <span class="text-xs text-slate-300 block font-semibold">Click or Drag Image File Here</span>
                        <span class="text-[10px] text-slate-500 block">PNG, JPG, WEBP formats supported</span>
                        <input type="file" id="articleImageFile" accept="image/*" onchange="previewSelectedImage(event, 'article')" class="hidden">
                    </div>
                    <div class="text-center text-[10px] text-slate-500 font-bold uppercase tracking-wider">OR ENTER DIRECT IMAGE URL</div>
                    <input type="url" id="articleImageUrl" placeholder="https://images.unsplash.com/photo-..." class="w-full bg-slate-900 border border-slate-800 text-white text-xs rounded-xl p-2.5 focus:outline-none focus:border-indigo-500 font-mono">
                    
                    <div id="articleImagePreviewContainer" class="hidden mt-2 rounded-xl overflow-hidden max-h-40 border border-slate-800 relative">
                        <img id="articleImagePreview" src="" alt="Preview" class="w-full h-full object-cover">
                    </div>
                </div>

                <div>
                    <label class="block text-xs text-slate-300 mb-1">Article Body Text *</label>
                    <textarea id="articleContent" rows="4" required placeholder="Provide your detailed financial reasoning, order backlog estimates, or milling output projections..." class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-3 focus:outline-none focus:border-indigo-500"></textarea>
                </div>
                
                <button type="submit" class="w-full py-3 bg-indigo-500 hover:bg-indigo-400 text-slate-950 font-bold rounded-xl text-xs uppercase tracking-wider transition shadow-glow-indigo">
                    Publish Article to KSX Hub
                </button>
            </form>
        </div>
    </div>

    <!-- NEWS PUBLISH MODAL (ENFORCED IMAGE ATTACHMENT) -->
    <div id="publishNewsModal" class="hidden fixed inset-0 z-50 bg-slate-950/85 backdrop-blur-md flex items-center justify-center p-4">
        <div class="glass-panel border border-slate-800 w-full max-w-xl rounded-3xl shadow-2xl p-6 relative max-h-[90vh] overflow-y-auto">
            <button onclick="closePublishNewsModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>
            <h3 class="text-lg font-bold text-white flex items-center gap-2 mb-1">
                <i data-lucide="newspaper" class="w-5 h-5 text-cyan-400"></i> Post Breaking News Release
            </h3>
            <p class="text-xs text-slate-400 mb-4">Post breaking news impacting local or global securities (Image Required).</p>
            <form onsubmit="handlePublishNews(event)" class="space-y-4">
                <div>
                    <label class="block text-xs text-slate-300 mb-1">News Headline *</label>
                    <input type="text" id="newsTitle" required placeholder="e.g. Chamo Milling Secures Export License to Regional Hubs" class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-3 focus:outline-none focus:border-cyan-500">
                </div>

                <div class="grid grid-cols-3 gap-3">
                    <div>
                        <label class="block text-xs text-slate-300 mb-1">Tag Asset *</label>
                        <select id="newsAssetTag" class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-3">
                            <!-- Populated via JS -->
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-300 mb-1">Region *</label>
                        <select id="newsLocation" class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-3">
                            <option value="LOCAL">Local (Zambia)</option>
                            <option value="INTERNATIONAL">International</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-300 mb-1">Sentiment *</label>
                        <select id="newsSentiment" class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-3">
                            <option value="BULLISH">BULLISH</option>
                            <option value="BEARISH">BEARISH</option>
                            <option value="NEUTRAL">NEUTRAL</option>
                        </select>
                    </div>
                </div>

                <div class="p-3 bg-slate-950/80 rounded-xl border border-cyan-500/30 space-y-2">
                    <label class="block text-xs font-bold text-cyan-300 flex items-center gap-1.5">
                        <i data-lucide="image" class="w-4 h-4"></i> News Thumbnail (Strictly Required) *
                    </label>
                    <div id="newsDropZone" class="border-2 border-dashed border-cyan-500/40 rounded-xl p-4 text-center cursor-pointer hover:border-cyan-400 transition" onclick="document.getElementById('newsImageFile').click()">
                        <i data-lucide="upload-cloud" class="w-8 h-8 mx-auto text-cyan-400 mb-1"></i>
                        <span class="text-xs text-slate-300 block font-semibold">Click or Drag Thumbnail File Here</span>
                        <input type="file" id="newsImageFile" accept="image/*" onchange="previewSelectedImage(event, 'news')" class="hidden">
                    </div>
                    <div class="text-center text-[10px] text-slate-500 font-bold uppercase tracking-wider">OR ENTER DIRECT IMAGE URL</div>
                    <input type="url" id="newsImageUrl" placeholder="https://images.unsplash.com/photo-..." class="w-full bg-slate-900 border border-slate-800 text-white text-xs rounded-xl p-2.5 focus:outline-none focus:border-cyan-500 font-mono">
                    
                    <div id="newsImagePreviewContainer" class="hidden mt-2 rounded-xl overflow-hidden max-h-40 border border-slate-800 relative">
                        <img id="newsImagePreview" src="" alt="Preview" class="w-full h-full object-cover">
                    </div>
                </div>

                <div>
                    <label class="block text-xs text-slate-300 mb-1">News Summary *</label>
                    <textarea id="newsSummary" rows="3" required placeholder="Provide a summary of the announcement..." class="w-full bg-slate-950 border border-slate-800 text-white text-xs rounded-xl p-3 focus:outline-none focus:border-cyan-500"></textarea>
                </div>

                <button type="submit" class="w-full py-3 bg-cyan-500 hover:bg-cyan-400 text-slate-950 font-bold rounded-xl text-xs uppercase tracking-wider transition shadow-glow-cyan">
                    Publish Breaking News
                </button>
            </form>
        </div>
    </div>

    <!-- MOBILE MONEY DEPOSIT MODAL -->
    <div id="depositModal" class="hidden fixed inset-0 z-50 bg-slate-950/85 backdrop-blur-md flex items-center justify-center p-4">
        <div class="glass-panel border border-slate-800 w-full max-w-md rounded-3xl shadow-2xl p-6 relative">
            <button onclick="closeDepositModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>
            <h3 class="text-lg font-bold text-white flex items-center gap-2 mb-1">
                <i data-lucide="smartphone" class="w-5 h-5 text-emerald-400"></i> Mobile Money Deposit
            </h3>
            <p class="text-xs text-slate-400 mb-4">Instant credit to your ZMW trading wallet.</p>
            <form onsubmit="handleDepositSubmit(event)" class="space-y-4">
                <div>
                    <label class="block text-xs text-slate-300 mb-1">Provider Network</label>
                    <div class="grid grid-cols-3 gap-2">
                        <label class="border border-slate-700 bg-slate-950 p-2.5 rounded-xl text-center cursor-pointer hover:border-emerald-500">
                            <input type="radio" name="momoProvider" value="Airtel Money" checked class="hidden peer">
                            <span class="block text-xs font-bold text-rose-400">Airtel</span>
                        </label>
                        <label class="border border-slate-700 bg-slate-950 p-2.5 rounded-xl text-center cursor-pointer hover:border-emerald-500">
                            <input type="radio" name="momoProvider" value="MTN MoMo" class="hidden peer">
                            <span class="block text-xs font-bold text-amber-400">MTN MoMo</span>
                        </label>
                        <label class="border border-slate-700 bg-slate-950 p-2.5 rounded-xl text-center cursor-pointer hover:border-emerald-500">
                            <input type="radio" name="momoProvider" value="ZANACO Express" class="hidden peer">
                            <span class="block text-xs font-bold text-emerald-400">ZANACO</span>
                        </label>
                    </div>
                </div>
                <div>
                    <label class="block text-xs text-slate-300 mb-1">Phone Number (+260)</label>
                    <input type="text" required placeholder="097XXXXXXX or 096XXXXXXX" class="w-full bg-slate-950 border border-slate-800 text-white font-mono text-sm rounded-xl p-3">
                </div>
                <div>
                    <label class="block text-xs text-slate-300 mb-1">Deposit Amount (ZMW)</label>
                    <input type="number" id="depositAmount" min="10" required placeholder="500" class="w-full bg-slate-950 border border-slate-800 text-white font-mono text-sm rounded-xl p-3">
                </div>
                <button type="submit" class="w-full py-3 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold rounded-xl shadow-glow-emerald text-sm transition">
                    Prompt Mobile PIN Request
                </button>
            </form>
        </div>
    </div>

    <!-- WITHDRAWAL MODAL -->
    <div id="withdrawModal" class="hidden fixed inset-0 z-50 bg-slate-950/85 backdrop-blur-md flex items-center justify-center p-4">
        <div class="glass-panel border border-slate-800 w-full max-w-md rounded-3xl shadow-2xl p-6 relative">
            <button onclick="closeWithdrawModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>
            <h3 class="text-lg font-bold text-white flex items-center gap-2 mb-1">
                <i data-lucide="arrow-up-right" class="w-5 h-5 text-amber-400"></i> Withdraw Cash Payout
            </h3>
            <p class="text-xs text-slate-400 mb-4">Direct mobile wallet transfer.</p>
            <form onsubmit="handleWithdrawSubmit(event)" class="space-y-4">
                <div>
                    <label class="block text-xs text-slate-300 mb-1">Destination Phone Number</label>
                    <input type="text" required placeholder="097XXXXXXX" class="w-full bg-slate-950 border border-slate-800 text-white font-mono text-sm rounded-xl p-3">
                </div>
                <div>
                    <label class="block text-xs text-slate-300 mb-1">Withdrawal Amount (ZMW)</label>
                    <input type="number" id="withdrawAmount" min="10" required placeholder="200" class="w-full bg-slate-950 border border-slate-800 text-white font-mono text-sm rounded-xl p-3">
                </div>
                <button type="submit" class="w-full py-3 bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold rounded-xl shadow-lg text-sm transition">
                    Process Payout
                </button>
            </form>
        </div>
    </div>

    <!-- APPLICATION JAVASCRIPT LOGIC -->
    <script>
        // ==========================================
        // 1. DATA STRUCTURES & DEFAULT SEEDED DATA
        // ==========================================
        const DEFAULT_ASSETS = [
            {
                id: 'KMH',
                name: 'Kabwe Mall Holdings',
                category: 'Real Estate / Retail',
                price: 18.50,
                change: 2.4,
                high: 19.10,
                low: 18.00,
                volume: 42300,
                marketCap: '148M',
                description: 'Premier retail & commercial property development situated in central Kabwe town center.'
            },
            {
                id: 'FVG',
                name: 'FVG Milling Company',
                category: 'Agribusiness & Grain Processing',
                price: 16.40,
                change: 3.2,
                high: 16.90,
                low: 15.80,
                volume: 84500,
                marketCap: '175M',
                description: 'Leading Central Province mealie meal manufacturer and commercial grain storage facility operator.'
            },
            {
                id: 'LDC',
                name: 'LDC Local Construction Co.',
                category: 'Infrastructure & Engineering',
                price: 21.00,
                change: 1.5,
                high: 21.80,
                low: 20.20,
                volume: 53200,
                marketCap: '230M',
                description: 'Major Zambian civil engineering contractor specializing in road networks, bridges, and commercial plazas.'
            },
            {
                id: 'CHM',
                name: 'Chamo Milling Products',
                category: 'Food Processing',
                price: 9.75,
                change: -0.8,
                high: 10.10,
                low: 9.60,
                volume: 91000,
                marketCap: '88M',
                description: 'Producer of fortified maize meal, animal stockfeeds, and refined agricultural oils in Kabwe district.'
            },
            {
                id: 'PTL',
                name: 'Power Tools Logistics',
                category: 'Transport & Freight',
                price: 12.80,
                change: -1.1,
                high: 13.20,
                low: 12.50,
                volume: 89000,
                marketCap: '95M',
                description: 'Inter-city passenger coach transport and heavy cargo haulage network across Zambia.'
            },
            {
                id: 'ZCCM',
                name: 'ZCCM Investments Holdings',
                category: 'Mining & Resources',
                price: 48.00,
                change: 1.8,
                high: 49.50,
                low: 47.20,
                volume: 154000,
                marketCap: '1.2B',
                description: 'State-backed mining holding company with strategic copper and gold royalties.'
            },
            {
                id: 'ZNC',
                name: 'ZANACO Bank Plc',
                category: 'Financial Services',
                price: 3.85,
                change: -0.5,
                high: 3.95,
                low: 3.80,
                volume: 310000,
                marketCap: '550M',
                description: "Zambia's flagship commercial bank serving nationwide retail and corporate clients."
            },
            {
                id: 'AIR',
                name: 'Airtel Networks Zambia',
                category: 'Telecommunications',
                price: 32.00,
                change: 3.1,
                high: 32.50,
                low: 31.00,
                volume: 98000,
                marketCap: '820M',
                description: 'Leading mobile network operator offering nationwide 4G/5G data and Airtel Money services.'
            }
        ];

        const DEFAULT_NEWS = [
            {
                id: 'news-local-1',
                type: 'LOCAL',
                title: 'FVG Milling Unveils New Automated Grain Silos in Kabwe Hub',
                category: 'Local Agribusiness',
                timestamp: '15 mins ago',
                assetId: 'FVG',
                sentiment: 'BULLISH',
                imageUrl: 'https://images.unsplash.com/photo-1574323347407-f5e1ad6d020b?auto=format&fit=crop&w=600&q=80',
                summary: 'FVG Milling expands operational capacity by 40,000 metric tons, bolstering supply chain security across Central Province.',
                source: 'Kabwe Commercial Gazette'
            },
            {
                id: 'news-intl-1',
                type: 'INTERNATIONAL',
                title: 'US Federal Reserve Cuts Benchmark Rates; Emerging Markets Gain',
                category: 'Global Macroeconomics',
                timestamp: '35 mins ago',
                assetId: 'ZNC',
                sentiment: 'BULLISH',
                imageUrl: 'https://images.unsplash.com/photo-1611974789855-9c2a0a7236a3?auto=format&fit=crop&w=600&q=80',
                summary: 'Lower US interest rates drive portfolio allocations toward high-yielding African equity markets, boosting ZANACO liquidity.',
                source: 'Bloomberg Financial Wire'
            },
            {
                id: 'news-local-2',
                type: 'LOCAL',
                title: 'LDC Local Construction Wins ZMW 150M Provincial Infrastructure Contract',
                category: 'Local Infrastructure',
                timestamp: '1 hour ago',
                assetId: 'LDC',
                sentiment: 'BULLISH',
                imageUrl: 'https://images.unsplash.com/photo-1541888946425-d0fbb186a5b7?auto=format&fit=crop&w=600&q=80',
                summary: 'LDC Construction awarded dual-carriageway paving and commercial bridge contracts linking Kabwe industrial parks.',
                source: 'Zambian Ministry of Infrastructure'
            },
            {
                id: 'news-local-3',
                type: 'LOCAL',
                title: 'Chamo Milling Expands Fortified Stockfeed Distribution to Copperbelt',
                category: 'Local Agribusiness',
                timestamp: '2 hours ago',
                assetId: 'CHM',
                sentiment: 'BULLISH',
                imageUrl: 'https://images.unsplash.com/photo-1595838788459-6485877d4a45?auto=format&fit=crop&w=600&q=80',
                summary: 'Chamo Milling secures key supply agreements with major commercial poultry and livestock cooperatives in Ndola and Kitwe.',
                source: 'Zambia Farmers National Union'
            },
            {
                id: 'news-intl-2',
                type: 'INTERNATIONAL',
                title: 'London Metal Exchange (LME) Copper Surges to 14-Month High',
                category: 'International Mining',
                timestamp: '4 hours ago',
                assetId: 'ZCCM',
                sentiment: 'BULLISH',
                imageUrl: 'https://images.unsplash.com/photo-1518709268805-4e9042af9f23?auto=format&fit=crop&w=600&q=80',
                summary: 'Global EV battery demand and green grid infrastructure buildouts spur copper rally, driving up ZCCM royalty forecasts.',
                source: 'Reuters Global Commodities'
            }
        ];

        const DEFAULT_ARTICLES = [
            {
                id: 'art-1',
                title: 'Investment Thesis: Why FVG Milling and Chamo Milling Are Cornering Central Province Supply',
                author: 'Mwala Tembo (Senior Analyst)',
                assetTag: 'FVG',
                category: 'Fundamental Analysis',
                date: 'Nov 07, 2026',
                upvotes: 56,
                downvotes: 2,
                imageUrl: 'https://images.unsplash.com/photo-1574323347407-f5e1ad6d020b?auto=format&fit=crop&w=800&q=80',
                content: 'Central Zambia continues to establish itself as the agricultural engine of the country. FVG Milling’s recent silo expansion paired with Chamo Milling’s stockfeed distribution upgrades create strong revenue moats. Dividend yields for both milling leaders are projected to outperform local bank deposits...',
                comments: [
                    { user: 'Bwalya K.', text: 'Agreed! Meal production margins remain protected even during climate fluctuations.' },
                    { user: 'Chileshe M.', text: 'Are they planning secondary listing offerings on LuSE as well?' }
                ]
            },
            {
                id: 'art-2',
                title: 'LDC Construction Order Backlog Reaches Record Highs in Central Zambia',
                author: 'Kondwani Phiri',
                assetTag: 'LDC',
                category: 'Infrastructure',
                date: 'Nov 06, 2026',
                upvotes: 38,
                downvotes: 1,
                imageUrl: 'https://images.unsplash.com/photo-1541888946425-d0fbb186a5b7?auto=format&fit=crop&w=800&q=80',
                content: 'LDC Local Construction Company has demonstrated consistent execution across municipal infrastructure contracts. With Kabwe’s transit role growing rapidly, local road contracts provide steady multi-year cash flows...',
                comments: [
                    { user: 'KabweTrader', text: 'LDC machinery asset utilization looks solid this quarter!' }
                ]
            }
        ];

        const SEEDED_USERS = [
            {
                name: 'Demo Trader',
                email: 'trader@ksx.zm',
                password: 'user123',
                role: 'USER',
                nrc: '111111/11/1',
                balance: 10000.00,
                holdings: { 'FVG': 300, 'LDC': 150, 'ZCCM': 50 }
            },
            {
                name: 'KSX Market Admin',
                email: 'admin@ksx.zm',
                password: 'admin123',
                role: 'ADMIN',
                nrc: '999999/99/1',
                balance: 250000.00,
                holdings: { 'ZCCM': 2000, 'CHM': 1000 }
            }
        ];

        // State Vars
        let assets = [];
        let newsItems = [];
        let articles = [];
        let users = [];
        let currentUser = null;
        let selectedAssetId = 'FVG';
        let currentOrderSide = 'BUY'; // BUY or SELL
        let currentOrderType = 'limit'; // limit or market
        let activeChartType = 'line'; // 'line' or 'candlestick'
        let activeNewsFilter = 'ALL';
        let showSMA9 = false;
        let showSMA20 = false;

        let chartPriceHistory = {};
        let canvasAnimationId = null;

        // ==========================================
        // 2. INITIALIZATION & LOCALSTORAGE persistence
        // ==========================================
        window.onload = function() {
            lucide.createIcons();
            initStorage();
            initPriceHistory();
            renderTopMiniTicker();
            renderMarketGrid();
            populateAssetDropdowns();
            selectAsset(selectedAssetId);
            renderNewsFeed();
            renderArticles();
            updateUIState();
            startLiveClock();
            startMarketSimulation();

            window.addEventListener('resize', () => {
                drawCustomCanvasChart();
            });
        };

        function initStorage() {
            const storedAssets = localStorage.getItem('ksx_assets_v3');
            assets = storedAssets ? JSON.parse(storedAssets) : DEFAULT_ASSETS;

            const storedNews = localStorage.getItem('ksx_news_v3');
            newsItems = storedNews ? JSON.parse(storedNews) : DEFAULT_NEWS;

            const storedArticles = localStorage.getItem('ksx_articles_v3');
            articles = storedArticles ? JSON.parse(storedArticles) : DEFAULT_ARTICLES;

            const storedUsers = localStorage.getItem('ksx_users_v3');
            users = storedUsers ? JSON.parse(storedUsers) : SEEDED_USERS;

            const sessionEmail = localStorage.getItem('ksx_session_v3');
            if (sessionEmail) {
                currentUser = users.find(u => u.email === sessionEmail) || null;
            }
        }

        function saveUsers() {
            localStorage.setItem('ksx_users_v3', JSON.stringify(users));
            if (currentUser) {
                const idx = users.findIndex(u => u.email === currentUser.email);
                if (idx !== -1) users[idx] = currentUser;
            }
            updateUIState();
        }

        function saveAssets() {
            localStorage.setItem('ksx_assets_v3', JSON.stringify(assets));
        }

        function saveNews() {
            localStorage.setItem('ksx_news_v3', JSON.stringify(newsItems));
        }

        function saveArticles() {
            localStorage.setItem('ksx_articles_v3', JSON.stringify(articles));
        }

        // Initialize detailed OHLC tick history for all assets
        function initPriceHistory() {
            const now = Date.now();
            assets.forEach(a => {
                const points = 40;
                const labels = [];
                const prices = [];
                const ohlc = [];
                const volumes = [];
                let p = a.price;

                for (let i = points; i >= 0; i--) {
                    const timeStr = new Date(now - i * 60000).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
                    const open = parseFloat((p + (Math.random() - 0.49) * 0.35).toFixed(2));
                    const close = parseFloat((open + (Math.random() - 0.48) * 0.45).toFixed(2));
                    const high = parseFloat((Math.max(open, close) + Math.random() * 0.35).toFixed(2));
                    const low = parseFloat((Math.min(open, close) - Math.random() * 0.35).toFixed(2));
                    const vol = Math.floor(Math.random() * 1500) + 300;
                    
                    labels.push(timeStr);
                    prices.push(close);
                    ohlc.push({ open, high, low, close });
                    volumes.push(vol);
                    p = close;
                }

                chartPriceHistory[a.id] = { labels, prices, ohlc, volumes };
            });
        }

        // ==========================================
        // 3. UI STATE & ROUTING
        // ==========================================
        function updateUIState() {
            const userHeaderBar = document.getElementById('userHeaderBar');
            const authHeaderBtns = document.getElementById('authHeaderBtns');
            const adminNavBtn = document.getElementById('admin-nav-btn');
            const mobileAdminBtn = document.getElementById('mobile-admin-btn');

            if (currentUser) {
                userHeaderBar.classList.remove('hidden');
                authHeaderBtns.classList.add('hidden');
                document.getElementById('userName').innerText = currentUser.name;
                document.getElementById('userAvatar').innerText = currentUser.name.slice(0, 2).toUpperCase();
                document.getElementById('headerBalance').innerText = `ZMW ${currentUser.balance.toLocaleString('en-US', {minimumFractionDigits: 2})}`;

                if (currentUser.role === 'ADMIN') {
                    document.getElementById('userRoleBadge').innerText = 'ADMIN';
                    document.getElementById('userRoleBadge').className = 'text-[9px] text-rose-400 uppercase font-bold';
                    adminNavBtn.classList.remove('hidden');
                    mobileAdminBtn.classList.remove('hidden');
                } else {
                    document.getElementById('userRoleBadge').innerText = 'VERIFIED';
                    document.getElementById('userRoleBadge').className = 'text-[9px] text-emerald-400 uppercase font-semibold';
                    adminNavBtn.classList.add('hidden');
                    mobileAdminBtn.classList.add('hidden');
                }
            } else {
                userHeaderBar.classList.add('hidden');
                authHeaderBtns.classList.remove('hidden');
                adminNavBtn.classList.add('hidden');
                mobileAdminBtn.classList.add('hidden');
            }

            populateAdminDropdowns();
            renderPortfolio();
            calculateOrderTotal();
        }

        function switchTab(tabId) {
            ['markets', 'trade', 'portfolio', 'news', 'community', 'admin'].forEach(id => {
                const el = document.getElementById(`tab-${id}`);
                const nav = document.getElementById(`nav-${id}`);
                if (el) el.classList.add('hidden');
                if (nav) nav.className = 'px-3.5 py-1.5 rounded-lg text-slate-300 hover:text-white hover:bg-slate-800 transition flex items-center gap-1.5';
            });

            const activeEl = document.getElementById(`tab-${tabId}`);
            const activeNav = document.getElementById(`nav-${tabId}`);
            if (activeEl) activeEl.classList.remove('hidden');
            if (activeNav) activeNav.className = 'px-3.5 py-1.5 rounded-lg bg-emerald-500/10 text-emerald-400 border border-emerald-500/20 font-bold transition flex items-center gap-1.5';

            if (tabId === 'admin' && (!currentUser || currentUser.role !== 'ADMIN')) {
                showToast('Administrator security permission required', 'error');
                switchTab('markets');
                return;
            }

            if (tabId === 'trade') {
                setTimeout(() => {
                    drawCustomCanvasChart();
                }, 50);
            }
        }

        function renderTopMiniTicker() {
            const container = document.getElementById('topMiniTicker');
            if (!container) return;
            container.innerHTML = assets.slice(0, 6).map(a => `
                <div class="flex items-center gap-1 cursor-pointer hover:text-white transition" onclick="tradeSpecificAsset('${a.id}')">
                    <span class="text-slate-400 font-bold">${a.id}:</span>
                    <span class="font-bold text-white">ZMW ${a.price.toFixed(2)}</span>
                    <span class="${a.change >= 0 ? 'text-emerald-400' : 'text-rose-400'}">${a.change >= 0 ? '+' : ''}${a.change}%</span>
                </div>
            `).join('<span class="text-slate-800">|</span>');
        }

        // ==========================================
        // 4. ADVANCED CUSTOM CANVAS CHART ENGINE
        // ==========================================
        function drawCustomCanvasChart() {
            const canvas = document.getElementById('ksxChartCanvas');
            if (!canvas) return;
            const ctx = canvas.getContext('2d');

            const rect = canvas.parentElement.getBoundingClientRect();
            canvas.width = rect.width;
            canvas.height = rect.height;

            const history = chartPriceHistory[selectedAssetId];
            if (!history || history.ohlc.length === 0) return;

            // Clear Canvas
            ctx.fillStyle = '#020617';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            const ohlc = history.ohlc;
            const prices = history.prices;
            const volumes = history.volumes;

            // Compute Scale Boundaries
            let minP = Math.min(...ohlc.map(c => c.low));
            let maxP = Math.max(...ohlc.map(c => c.high));
            const pPadding = (maxP - minP) * 0.1 || 1;
            minP -= pPadding;
            maxP += pPadding;

            const maxVol = Math.max(...volumes, 1);
            const chartHeight = canvas.height - 40;
            const volHeight = 70;

            const barWidth = Math.max(3, (canvas.width - 60) / ohlc.length - 4);

            // Draw Horizontal Gridlines & Price Scale
            ctx.strokeStyle = '#1e293b';
            ctx.lineWidth = 1;
            for (let i = 1; i <= 4; i++) {
                const y = (chartHeight / 5) * i;
                ctx.beginPath();
                ctx.moveTo(0, y);
                ctx.lineTo(canvas.width - 50, y);
                ctx.stroke();

                const pVal = maxP - (i / 5) * (maxP - minP);
                ctx.fillStyle = '#64748b';
                ctx.font = '10px JetBrains Mono';
                ctx.fillText('ZMW ' + pVal.toFixed(2), canvas.width - 48, y + 3);
            }

            // Draw Bottom Volume Bars
            ohlc.forEach((c, idx) => {
                const x = 15 + idx * (barWidth + 4);
                const vol = volumes[idx];
                const vH = (vol / maxVol) * volHeight;
                const isBull = c.close >= c.open;

                ctx.fillStyle = isBull ? 'rgba(16, 185, 129, 0.25)' : 'rgba(244, 63, 94, 0.25)';
                ctx.fillRect(x, canvas.height - vH - 20, barWidth, vH);
            });

            // Draw Price Lines or Candlesticks
            if (activeChartType === 'line') {
                // Line View with Fill Gradient
                const gradient = ctx.createLinearGradient(0, 0, 0, chartHeight);
                gradient.addColorStop(0, 'rgba(16, 185, 129, 0.35)');
                gradient.addColorStop(1, 'rgba(16, 185, 129, 0.0)');

                ctx.beginPath();
                prices.forEach((p, idx) => {
                    const x = 15 + idx * (barWidth + 4) + barWidth / 2;
                    const y = chartHeight - ((p - minP) / (maxP - minP)) * chartHeight;
                    if (idx === 0) ctx.moveTo(x, y);
                    else ctx.lineTo(x, y);
                });

                ctx.strokeStyle = '#10b981';
                ctx.lineWidth = 2.5;
                ctx.stroke();

                // Fill area
                const lastX = 15 + (prices.length - 1) * (barWidth + 4) + barWidth / 2;
                ctx.lineTo(lastX, chartHeight);
                ctx.lineTo(15 + barWidth / 2, chartHeight);
                ctx.closePath();
                ctx.fillStyle = gradient;
                ctx.fill();

            } else {
                // Candlestick rendering with wicks
                ohlc.forEach((c, idx) => {
                    const x = 15 + idx * (barWidth + 4);
                    const isBull = c.close >= c.open;
                    const color = isBull ? '#10b981' : '#f43f5e';

                    const openY = chartHeight - ((c.open - minP) / (maxP - minP)) * chartHeight;
                    const closeY = chartHeight - ((c.close - minP) / (maxP - minP)) * chartHeight;
                    const highY = chartHeight - ((c.high - minP) / (maxP - minP)) * chartHeight;
                    const lowY = chartHeight - ((c.low - minP) / (maxP - minP)) * chartHeight;

                    // Wick
                    ctx.strokeStyle = color;
                    ctx.lineWidth = 1.5;
                    ctx.beginPath();
                    ctx.moveTo(x + barWidth / 2, highY);
                    ctx.lineTo(x + barWidth / 2, lowY);
                    ctx.stroke();

                    // Body
                    ctx.fillStyle = color;
                    const bodyY = Math.min(openY, closeY);
                    const bodyH = Math.max(2, Math.abs(closeY - openY));
                    ctx.fillRect(x, bodyY, barWidth, bodyH);
                });
            }

            // Technical SMA Overlays
            if (showSMA9) drawSMA(ctx, prices, 9, '#f59e0b', barWidth, minP, maxP, chartHeight);
            if (showSMA20) drawSMA(ctx, prices, 20, '#6366f1', barWidth, minP, maxP, chartHeight);
        }

        function drawSMA(ctx, prices, period, color, barWidth, minP, maxP, chartHeight) {
            ctx.beginPath();
            ctx.strokeStyle = color;
            ctx.lineWidth = 1.5;

            for (let i = period - 1; i < prices.length; i++) {
                const slice = prices.slice(i - period + 1, i + 1);
                const avg = slice.reduce((a, b) => a + b, 0) / period;
                const x = 15 + i * (barWidth + 4) + barWidth / 2;
                const y = chartHeight - ((avg - minP) / (maxP - minP)) * chartHeight;

                if (i === period - 1) ctx.moveTo(x, y);
                else ctx.lineTo(x, y);
            }
            ctx.stroke();
        }

        function setChartType(type) {
            activeChartType = type;
            const bLine = document.getElementById('chartTypeLine');
            const bCandle = document.getElementById('chartTypeCandle');

            if (type === 'line') {
                bLine.className = 'px-3 py-1 rounded-lg bg-emerald-500 text-slate-950 font-bold text-[11px] flex items-center gap-1.5 transition';
                bCandle.className = 'px-3 py-1 rounded-lg text-slate-400 hover:text-white text-[11px] flex items-center gap-1.5 transition';
            } else {
                bCandle.className = 'px-3 py-1 rounded-lg bg-emerald-500 text-slate-950 font-bold text-[11px] flex items-center gap-1.5 transition';
                bLine.className = 'px-3 py-1 rounded-lg text-slate-400 hover:text-white text-[11px] flex items-center gap-1.5 transition';
            }
            drawCustomCanvasChart();
            showToast(`Switched chart mode to ${type.toUpperCase()}`, 'info');
        }

        function toggleSMA(period) {
            if (period === 9) {
                showSMA9 = !showSMA9;
                const btn = document.getElementById('sma9Btn');
                btn.className = showSMA9 ? 'px-2.5 py-1 rounded-lg bg-amber-500 text-slate-950 font-bold text-[10px] font-mono' : 'px-2.5 py-1 rounded-lg text-slate-400 hover:text-white text-[10px] font-mono';
            } else {
                showSMA20 = !showSMA20;
                const btn = document.getElementById('sma20Btn');
                btn.className = showSMA20 ? 'px-2.5 py-1 rounded-lg bg-indigo-500 text-slate-950 font-bold text-[10px] font-mono' : 'px-2.5 py-1 rounded-lg text-slate-400 hover:text-white text-[10px] font-mono';
            }
            drawCustomCanvasChart();
        }

        function changeTimeframe(tf) {
            ['1M', '5M', '1H', '1D'].forEach(t => {
                const b = document.getElementById(`tf-${t}`);
                if (b) b.className = t === tf ? 'px-2.5 py-1 rounded-lg bg-emerald-500 text-slate-950 font-bold text-[11px]' : 'px-2.5 py-1 rounded-lg text-slate-400 hover:text-white text-[11px]';
            });
            drawCustomCanvasChart();
            showToast(`Timeframe resolution set to ${tf}`, 'info');
        }

        // Live Market Tick Engine
        function startMarketSimulation() {
            setInterval(() => {
                const asset = assets.find(a => a.id === selectedAssetId);
                if (!asset) return;

                const delta = (Math.random() - 0.485) * (asset.price * 0.009);
                const oldPrice = asset.price;
                asset.price = Math.max(0.10, parseFloat((asset.price + delta).toFixed(2)));

                if (asset.price > asset.high) asset.high = asset.price;
                if (asset.price < asset.low) asset.low = asset.price;

                const pEl = document.getElementById('terminalPrice');
                if (pEl) {
                    pEl.innerText = `ZMW ${asset.price.toFixed(2)}`;
                    pEl.className = `text-xl sm:text-3xl font-black font-mono ${asset.price >= oldPrice ? 'text-emerald-400 flash-up' : 'text-rose-400 flash-down'}`;
                }

                document.getElementById('statHigh').innerText = `ZMW ${asset.high.toFixed(2)}`;
                document.getElementById('statLow').innerText = `ZMW ${asset.low.toFixed(2)}`;

                if (chartPriceHistory[selectedAssetId]) {
                    const h = chartPriceHistory[selectedAssetId];
                    const nowStr = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' });
                    
                    h.labels.shift();
                    h.labels.push(nowStr);
                    h.prices.shift();
                    h.prices.push(asset.price);

                    const open = oldPrice;
                    const close = asset.price;
                    const high = Math.max(open, close) + 0.1;
                    const low = Math.min(open, close) - 0.1;
                    const vol = Math.floor(Math.random() * 1200) + 200;

                    h.ohlc.shift();
                    h.ohlc.push({ open, high, low, close });
                    h.volumes.shift();
                    h.volumes.push(vol);

                    drawCustomCanvasChart();
                }

                renderOrderBook(asset.price);
                renderTopMiniTicker();
            }, 2000);
        }

        // ==========================================
        // 5. MARKETS & ORDER TICKET EXECUTION
        // ==========================================
        function renderMarketGrid(filterText = '') {
            const grid = document.getElementById('marketGrid');
            if (!grid) return;
            grid.innerHTML = '';

            const filtered = assets.filter(a => 
                a.name.toLowerCase().includes(filterText.toLowerCase()) || 
                a.id.toLowerCase().includes(filterText.toLowerCase()) ||
                a.category.toLowerCase().includes(filterText.toLowerCase())
            );

            document.getElementById('marketAssetCount').innerText = `${filtered.length} Securities`;

            filtered.forEach(a => {
                const isPos = a.change >= 0;
                const card = document.createElement('div');
                card.className = 'glass-panel rounded-3xl p-6 flex flex-col justify-between hover:border-emerald-500/40 transition group';
                card.innerHTML = `
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <span class="px-2.5 py-1 rounded-lg text-xs font-bold bg-slate-900 text-emerald-400 font-mono border border-slate-800">${a.id}</span>
                            <span class="text-xs font-bold font-mono ${isPos ? 'text-emerald-400' : 'text-rose-400'} flex items-center gap-0.5">
                                ${isPos ? '▲ +' : '▼ '}${a.change}%
                            </span>
                        </div>
                        <h4 class="text-base font-extrabold text-white group-hover:text-emerald-400 transition tracking-tight">${a.name}</h4>
                        <p class="text-xs text-slate-400 mt-1 line-clamp-2">${a.description}</p>
                    </div>

                    <div class="mt-5 pt-4 border-t border-slate-800/80 flex items-center justify-between">
                        <div>
                            <span class="text-[10px] text-slate-500 uppercase font-semibold block">Spot Quote</span>
                            <span class="text-lg font-black font-mono text-white">ZMW ${a.price.toFixed(2)}</span>
                        </div>
                        <button onclick="tradeSpecificAsset('${a.id}')" class="px-4 py-2 bg-slate-900 hover:bg-emerald-500 hover:text-slate-950 text-slate-200 text-xs font-extrabold rounded-xl border border-slate-800 transition flex items-center gap-1.5 shadow-glow-emerald">
                            Trade Terminal <i data-lucide="arrow-right" class="w-3.5 h-3.5"></i>
                        </button>
                    </div>
                `;
                grid.appendChild(card);
            });
            lucide.createIcons();
        }

        function filterMarkets() {
            const query = document.getElementById('marketSearchInput').value;
            renderMarketGrid(query);
        }

        function tradeSpecificAsset(assetId) {
            selectAsset(assetId);
            switchTab('trade');
        }

        function populateAssetDropdowns() {
            ['assetSelector', 'adminAssetSelect', 'articleAssetTag', 'newsAssetTag'].forEach(id => {
                const el = document.getElementById(id);
                if (!el) return;
                el.innerHTML = '';
                assets.forEach(a => {
                    const opt = document.createElement('option');
                    opt.value = a.id;
                    opt.innerText = `${a.id} - ${a.name}`;
                    el.appendChild(opt);
                });
            });
        }

        function selectAsset(assetId) {
            selectedAssetId = assetId;
            const asset = assets.find(a => a.id === assetId);
            if (!asset) return;

            document.getElementById('assetSelector').value = assetId;
            document.getElementById('terminalPrice').innerText = `ZMW ${asset.price.toFixed(2)}`;
            document.getElementById('statHigh').innerText = `ZMW ${asset.high.toFixed(2)}`;
            document.getElementById('statLow').innerText = `ZMW ${asset.low.toFixed(2)}`;
            document.getElementById('statVol').innerText = `${asset.volume.toLocaleString()} SHARES`;
            document.getElementById('statCap').innerText = `ZMW ${asset.marketCap}`;
            document.getElementById('chartTitle').innerText = `${asset.name} (${asset.id}) Trading Canvas`;

            document.getElementById('orderPrice').value = asset.price.toFixed(2);
            renderOrderBook(asset.price);
            drawCustomCanvasChart();
            calculateOrderTotal();
        }

        function renderOrderBook(basePrice) {
            const asksContainer = document.getElementById('orderbookAsks');
            const bidsContainer = document.getElementById('orderbookBids');
            document.getElementById('orderbookCurrent').innerText = `ZMW ${basePrice.toFixed(2)}`;

            asksContainer.innerHTML = '';
            bidsContainer.innerHTML = '';

            for (let i = 4; i >= 1; i--) {
                const askPrice = (basePrice + i * 0.12).toFixed(2);
                const size = Math.floor(Math.random() * 250) + 20;
                const total = (askPrice * size).toFixed(0);
                asksContainer.innerHTML += `
                    <div class="grid grid-cols-3 px-1 py-0.5 hover:bg-slate-900 cursor-pointer">
                        <span class="font-semibold">${askPrice}</span>
                        <span class="text-right text-slate-300">${size}</span>
                        <span class="text-right text-slate-500">${total}</span>
                    </div>
                `;
            }

            for (let i = 1; i <= 5; i++) {
                const bidPrice = (basePrice - i * 0.12).toFixed(2);
                const size = Math.floor(Math.random() * 300) + 15;
                const total = (bidPrice * size).toFixed(0);
                bidsContainer.innerHTML += `
                    <div class="grid grid-cols-3 px-1 py-0.5 hover:bg-slate-900 cursor-pointer">
                        <span class="font-semibold">${bidPrice}</span>
                        <span class="text-right text-slate-300">${size}</span>
                        <span class="text-right text-slate-500">${total}</span>
                    </div>
                `;
            }
        }

        function setOrderSide(side) {
            currentOrderSide = side;
            const bBuy = document.getElementById('btnSideBuy');
            const bSell = document.getElementById('btnSideSell');
            const submitBtn = document.getElementById('btnSubmitOrder');

            if (side === 'BUY') {
                bBuy.className = 'py-2 rounded-lg text-xs font-bold transition bg-emerald-500 text-slate-950 shadow-glow-emerald';
                bSell.className = 'py-2 rounded-lg text-xs font-bold transition text-slate-400 hover:text-white';
                submitBtn.className = 'w-full mt-3 py-3 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-extrabold rounded-xl shadow-glow-emerald text-xs uppercase tracking-wider transition';
                submitBtn.innerText = 'Execute Buy Order';
            } else {
                bSell.className = 'py-2 rounded-lg text-xs font-bold transition bg-rose-500 text-white shadow-glow-rose';
                bBuy.className = 'py-2 rounded-lg text-xs font-bold transition text-slate-400 hover:text-white';
                submitBtn.className = 'w-full mt-3 py-3 bg-rose-500 hover:bg-rose-400 text-white font-extrabold rounded-xl shadow-glow-rose text-xs uppercase tracking-wider transition';
                submitBtn.innerText = 'Execute Sell Order';
            }
            calculateOrderTotal();
        }

        function setOrderType(type) {
            currentOrderType = type;
            const bLimit = document.getElementById('typeLimitBtn');
            const bMarket = document.getElementById('typeMarketBtn');
            const priceInput = document.getElementById('orderPrice');

            if (type === 'limit') {
                bLimit.className = 'text-emerald-400 font-bold border-b border-emerald-400';
                bMarket.className = 'text-slate-400 hover:text-white';
                priceInput.readOnly = false;
            } else {
                bMarket.className = 'text-emerald-400 font-bold border-b border-emerald-400';
                bLimit.className = 'text-slate-400 hover:text-white';
                priceInput.readOnly = true;
                priceInput.value = assets.find(a => a.id === selectedAssetId).price.toFixed(2);
            }
            calculateOrderTotal();
        }

        function setOrderPct(pct) {
            if (!currentUser) return;
            const price = parseFloat(document.getElementById('orderPrice').value) || 1;
            if (currentOrderSide === 'BUY') {
                const spendable = currentUser.balance * pct;
                document.getElementById('orderQty').value = Math.floor(spendable / price);
            } else {
                const holdings = currentUser.holdings[selectedAssetId] || 0;
                document.getElementById('orderQty').value = Math.floor(holdings * pct);
            }
            calculateOrderTotal();
        }

        function calculateOrderTotal() {
            const price = parseFloat(document.getElementById('orderPrice').value) || 0;
            const qty = parseInt(document.getElementById('orderQty').value) || 0;
            const subtotal = price * qty;
            const fee = subtotal * 0.001;
            const total = subtotal + fee;

            document.getElementById('orderEstTotal').innerText = `ZMW ${total.toFixed(2)}`;
            document.getElementById('orderFeeVal').innerText = `ZMW ${fee.toFixed(2)}`;

            if (currentUser) {
                if (currentOrderSide === 'BUY') {
                    document.getElementById('orderAvailCash').innerText = `ZMW ${currentUser.balance.toFixed(2)}`;
                } else {
                    const owned = currentUser.holdings[selectedAssetId] || 0;
                    document.getElementById('orderAvailCash').innerText = `${owned} Shares Owned`;
                }
            } else {
                document.getElementById('orderAvailCash').innerText = 'Log in required';
            }
        }

        function executeTrade() {
            if (!currentUser) {
                openAuthModal('login');
                showToast('Please log in to place orders', 'info');
                return;
            }

            const price = parseFloat(document.getElementById('orderPrice').value);
            const qty = parseInt(document.getElementById('orderQty').value);

            if (!price || price <= 0 || !qty || qty <= 0) {
                showToast('Invalid price or quantity', 'error');
                return;
            }

            const totalCost = (price * qty) * 1.001;

            if (currentOrderSide === 'BUY') {
                if (currentUser.balance < totalCost) {
                    showToast('Insufficient cash balance for this order', 'error');
                    return;
                }
                currentUser.balance -= totalCost;
                currentUser.holdings[selectedAssetId] = (currentUser.holdings[selectedAssetId] || 0) + qty;
                showToast(`EXECUTED BUY: ${qty} units of ${selectedAssetId} @ ZMW ${price.toFixed(2)}`, 'success');
            } else {
                const owned = currentUser.holdings[selectedAssetId] || 0;
                if (owned < qty) {
                    showToast(`Insufficient share holdings! You only own ${owned} units.`, 'error');
                    return;
                }
                const proceeds = (price * qty) * 0.999;
                currentUser.balance += proceeds;
                currentUser.holdings[selectedAssetId] -= qty;
                if (currentUser.holdings[selectedAssetId] <= 0) delete currentUser.holdings[selectedAssetId];
                showToast(`EXECUTED SELL: ${qty} units of ${selectedAssetId} @ ZMW ${price.toFixed(2)}`, 'success');
            }

            saveUsers();
        }

        // ==========================================
        // 6. PORTFOLIO VIEW LOGIC
        // ==========================================
        function renderPortfolio() {
            const body = document.getElementById('holdingsTableBody');
            if (!body) return;
            body.innerHTML = '';

            if (!currentUser) {
                document.getElementById('portTotalVal').innerText = 'ZMW 0.00';
                document.getElementById('portCashVal').innerText = 'ZMW 0.00';
                body.innerHTML = `<tr><td colspan="6" class="py-6 text-center text-slate-500 font-sans">Log in to view portfolio holdings</td></tr>`;
                return;
            }

            let holdingsVal = 0;
            const keys = Object.keys(currentUser.holdings || {});

            if (keys.length === 0) {
                body.innerHTML = `<tr><td colspan="6" class="py-6 text-center text-slate-500 font-sans">No security holdings in portfolio. Visit Markets to trade!</td></tr>`;
            } else {
                keys.forEach(sym => {
                    const qty = currentUser.holdings[sym];
                    const asset = assets.find(a => a.id === sym);
                    if (!asset || qty <= 0) return;
                    const val = qty * asset.price;
                    holdingsVal += val;

                    const tr = document.createElement('tr');
                    tr.className = 'hover:bg-slate-900/50';
                    tr.innerHTML = `
                        <td class="py-3 px-3 font-bold text-emerald-400">${asset.id}</td>
                        <td class="py-3 px-3 font-sans font-medium text-slate-200">${asset.name}</td>
                        <td class="py-3 px-3 text-right font-bold text-white">${qty}</td>
                        <td class="py-3 px-3 text-right text-slate-300">ZMW ${asset.price.toFixed(2)}</td>
                        <td class="py-3 px-3 text-right font-bold text-emerald-400">ZMW ${val.toFixed(2)}</td>
                        <td class="py-3 px-3 text-center">
                            <button onclick="tradeSpecificAsset('${asset.id}')" class="px-2.5 py-1 bg-slate-800 hover:bg-slate-700 text-slate-200 rounded-lg text-[11px] font-sans">
                                Trade
                            </button>
                        </td>
                    `;
                    body.appendChild(tr);
                });
            }

            const grandTotal = currentUser.balance + holdingsVal;
            document.getElementById('portTotalVal').innerText = `ZMW ${grandTotal.toLocaleString('en-US', {minimumFractionDigits: 2})}`;
            document.getElementById('portCashVal').innerText = `ZMW ${currentUser.balance.toLocaleString('en-US', {minimumFractionDigits: 2})}`;
        }

        // ==========================================
        // 7. COMMUNITY ARTICLES & NEWS PUBLISHING
        // ==========================================
        function openPublishArticleModal() {
            if (!currentUser) { openAuthModal('login'); return; }
            document.getElementById('publishArticleModal').classList.remove('hidden');
        }

        function closePublishArticleModal() {
            document.getElementById('publishArticleModal').classList.add('hidden');
        }

        function openPublishNewsModal() {
            if (!currentUser) { openAuthModal('login'); return; }
            document.getElementById('publishNewsModal').classList.remove('hidden');
        }

        function closePublishNewsModal() {
            document.getElementById('publishNewsModal').classList.add('hidden');
        }

        function previewSelectedImage(event, targetType) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    const previewImg = document.getElementById(`${targetType}ImagePreview`);
                    previewImg.src = e.target.result;
                    document.getElementById(`${targetType}ImagePreviewContainer`).classList.remove('hidden');
                };
                reader.readAsDataURL(file);
            }
        }

        function handlePublishArticle(e) {
            e.preventDefault();
            const title = document.getElementById('articleTitle').value.trim();
            const assetTag = document.getElementById('articleAssetTag').value;
            const category = document.getElementById('articleCategory').value;
            const content = document.getElementById('articleContent').value.trim();
            const urlInput = document.getElementById('articleImageUrl').value.trim();
            const previewSrc = document.getElementById('articleImagePreview').src;

            let finalImage = urlInput;
            if (!finalImage && previewSrc && !document.getElementById('articleImagePreviewContainer').classList.contains('hidden')) {
                finalImage = previewSrc;
            }

            // STRICT VALIDATION CHECK FOR IMAGE & CONTENT
            if (!finalImage || !content) {
                showToast('VALIDATION ERROR: Both body analysis and an attached image are strictly required to post.', 'error');
                return;
            }

            const newArt = {
                id: 'art-' + Date.now(),
                title,
                author: currentUser.name,
                assetTag,
                category,
                date: new Date().toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' }),
                upvotes: 1,
                downvotes: 0,
                imageUrl: finalImage,
                content,
                comments: []
            };

            articles.unshift(newArt);
            saveArticles();
            closePublishArticleModal();
            renderArticles();
            showToast('Article published with image successfully!', 'success');
        }

        function handlePublishNews(e) {
            e.preventDefault();
            const title = document.getElementById('newsTitle').value.trim();
            const assetId = document.getElementById('newsAssetTag').value;
            const type = document.getElementById('newsLocation').value;
            const sentiment = document.getElementById('newsSentiment').value;
            const summary = document.getElementById('newsSummary').value.trim();
            const urlInput = document.getElementById('newsImageUrl').value.trim();
            const previewSrc = document.getElementById('newsImagePreview').src;

            let finalImage = urlInput;
            if (!finalImage && previewSrc && !document.getElementById('newsImagePreviewContainer').classList.contains('hidden')) {
                finalImage = previewSrc;
            }

            // STRICT VALIDATION CHECK FOR IMAGE & SUMMARY
            if (!finalImage || !summary) {
                showToast('VALIDATION ERROR: News summary and a thumbnail image are strictly required.', 'error');
                return;
            }

            const newNews = {
                id: 'news-' + Date.now(),
                type,
                title,
                category: type === 'LOCAL' ? 'Local Development' : 'Global Financial',
                timestamp: 'Just now',
                assetId,
                sentiment,
                imageUrl: finalImage,
                summary,
                source: `Posted by ${currentUser.name}`
            };

            newsItems.unshift(newNews);
            saveNews();
            closePublishNewsModal();
            renderNewsFeed();
            showToast('Breaking news published to stream!', 'success');
        }

        function renderArticles() {
            const container = document.getElementById('articlesList');
            if (!container) return;
            container.innerHTML = '';

            articles.forEach(art => {
                const card = document.createElement('div');
                card.className = 'glass-panel rounded-3xl p-6 space-y-4 border border-slate-800';

                const commentsHTML = art.comments.map(c => `
                    <div class="bg-slate-950 p-3 rounded-xl border border-slate-800/80 text-xs font-sans">
                        <span class="font-bold text-emerald-400 mr-2">${c.user}:</span>
                        <span class="text-slate-300">${c.text}</span>
                    </div>
                `).join('');

                card.innerHTML = `
                    <div class="flex items-center justify-between flex-wrap gap-2">
                        <div class="flex items-center gap-2">
                            <span class="px-2.5 py-0.5 rounded-lg text-[10px] font-bold bg-indigo-500/10 text-indigo-400 border border-indigo-500/20 font-mono">${art.assetTag}</span>
                            <span class="px-2.5 py-0.5 rounded-lg text-[10px] font-semibold bg-slate-900 text-slate-400 border border-slate-800">${art.category}</span>
                        </div>
                        <span class="text-xs text-slate-500 font-mono">${art.date}</span>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-12 gap-5 items-start">
                        ${art.imageUrl ? `
                        <div class="md:col-span-4 rounded-2xl overflow-hidden border border-slate-800 max-h-48">
                            <img src="${art.imageUrl}" alt="Article Feature" class="w-full h-full object-cover hover:scale-105 transition duration-500" onerror="this.src='https://images.unsplash.com/photo-1611974789855-9c2a0a7236a3?auto=format&fit=crop&w=600&q=80'">
                        </div>
                        ` : ''}
                        <div class="${art.imageUrl ? 'md:col-span-8' : 'md:col-span-12'} space-y-2">
                            <h3 class="text-xl font-bold text-white tracking-tight">${art.title}</h3>
                            <p class="text-xs sm:text-sm text-slate-300 leading-relaxed">${art.content}</p>
                        </div>
                    </div>

                    <div class="flex items-center justify-between text-xs pt-3 border-t border-slate-800">
                        <span class="text-slate-400 italic">Authored by <strong class="text-slate-200 font-sans">${art.author}</strong></span>
                        <div class="flex items-center gap-2">
                            <button onclick="upvoteArticle('${art.id}', true)" class="flex items-center gap-1.5 bg-slate-900 hover:bg-indigo-950/40 text-slate-200 px-3 py-1 rounded-xl border border-slate-800 transition font-mono text-xs">
                                <i data-lucide="thumbs-up" class="w-3.5 h-3.5 text-indigo-400"></i> ${art.upvotes || 0}
                            </button>
                            <button onclick="upvoteArticle('${art.id}', false)" class="flex items-center gap-1.5 bg-slate-900 hover:bg-rose-950/40 text-slate-200 px-3 py-1 rounded-xl border border-slate-800 transition font-mono text-xs">
                                <i data-lucide="thumbs-down" class="w-3.5 h-3.5 text-rose-400"></i> ${art.downvotes || 0}
                            </button>
                        </div>
                    </div>

                    <div class="space-y-2 pt-2">
                        <span class="text-[10px] font-bold text-slate-400 block uppercase tracking-wider">Responses (${art.comments.length})</span>
                        <div class="space-y-1.5">${commentsHTML}</div>
                        <div class="flex gap-2 pt-2">
                            <input type="text" id="comment-input-${art.id}" placeholder="Write response..." class="flex-1 bg-slate-950 border border-slate-800 text-white text-xs rounded-xl px-3.5 py-2 focus:outline-none focus:border-indigo-500">
                            <button onclick="addComment('${art.id}')" class="px-4 py-2 bg-indigo-500 text-slate-950 font-bold text-xs rounded-xl hover:bg-indigo-400 transition shadow-glow-indigo">Post</button>
                        </div>
                    </div>
                `;
                container.appendChild(card);
            });
            lucide.createIcons();
        }

        function upvoteArticle(id, isUp) {
            const art = articles.find(a => a.id === id);
            if (art) {
                if (isUp) art.upvotes = (art.upvotes || 0) + 1;
                else art.downvotes = (art.downvotes || 0) + 1;
                saveArticles();
                renderArticles();
            }
        }

        function addComment(id) {
            if (!currentUser) { openAuthModal('login'); return; }
            const input = document.getElementById(`comment-input-${id}`);
            const text = input.value.trim();
            if (!text) return;

            const art = articles.find(a => a.id === id);
            if (art) {
                art.comments.push({ user: currentUser.name, text });
                saveArticles();
                renderArticles();
            }
        }

        function renderNewsFeed() {
            const grid = document.getElementById('newsGrid');
            if (!grid) return;
            grid.innerHTML = '';

            let filtered = newsItems;
            if (activeNewsFilter === 'LOCAL') filtered = newsItems.filter(n => n.type === 'LOCAL');
            if (activeNewsFilter === 'INTERNATIONAL') filtered = newsItems.filter(n => n.type === 'INTERNATIONAL');
            if (activeNewsFilter === 'MY_PORTFOLIO' && currentUser) {
                const owned = Object.keys(currentUser.holdings || {});
                filtered = newsItems.filter(n => owned.includes(n.assetId));
            }

            filtered.forEach(n => {
                const isBull = n.sentiment === 'BULLISH';
                const isLocal = n.type === 'LOCAL';
                const card = document.createElement('div');
                card.className = 'glass-panel rounded-3xl p-5 flex flex-col justify-between space-y-3 border border-slate-800 overflow-hidden';
                card.innerHTML = `
                    <div>
                        ${n.imageUrl ? `
                        <div class="h-40 -mx-5 -mt-5 mb-3 overflow-hidden border-b border-slate-800">
                            <img src="${n.imageUrl}" alt="News Thumbnail" class="w-full h-full object-cover hover:scale-105 transition duration-500" onerror="this.src='https://images.unsplash.com/photo-1611974789855-9c2a0a7236a3?auto=format&fit=crop&w=600&q=80'">
                        </div>
                        ` : ''}
                        <div class="flex items-center justify-between mb-2">
                            <div class="flex items-center gap-1.5">
                                <span class="px-2.5 py-0.5 rounded-md text-[10px] font-bold bg-slate-900 text-cyan-400 font-mono border border-slate-800">${n.assetId}</span>
                                <span class="px-2 py-0.5 rounded-md text-[9px] font-bold ${isLocal ? 'bg-amber-500/10 text-amber-400 border border-amber-500/20' : 'bg-indigo-500/10 text-indigo-400 border border-indigo-500/20'} font-mono">
                                    ${n.type}
                                </span>
                            </div>
                            <span class="px-2.5 py-0.5 rounded-md text-[10px] font-extrabold ${isBull ? 'bg-emerald-500/10 text-emerald-400 border border-emerald-500/20' : 'bg-rose-500/10 text-rose-400 border border-rose-500/20'}">
                                ${n.sentiment}
                            </span>
                        </div>
                        <h4 class="text-base font-bold text-white leading-snug tracking-tight">${n.title}</h4>
                        <p class="text-xs text-slate-400 mt-2 leading-relaxed">${n.summary}</p>
                    </div>
                    <div class="flex items-center justify-between text-[10px] text-slate-500 font-mono pt-3 border-t border-slate-800">
                        <span>${n.source}</span>
                        <span>${n.timestamp}</span>
                    </div>
                `;
                grid.appendChild(card);
            });
            lucide.createIcons();
        }

        function filterNews(type) {
            activeNewsFilter = type;
            ['ALL', 'LOCAL', 'INTL', 'PORT'].forEach(t => {
                const btn = document.getElementById(`newsFilter${t}`);
                if (btn) {
                    btn.className = (t === type || (t === 'INTL' && type === 'INTERNATIONAL') || (t === 'PORT' && type === 'MY_PORTFOLIO')) ? 
                        'px-3.5 py-2 rounded-xl text-xs font-bold bg-emerald-500 text-slate-950 shadow-glow-emerald' : 
                        'px-3.5 py-2 rounded-xl text-xs font-bold bg-slate-900 text-slate-400 border border-slate-800';
                }
            });
            renderNewsFeed();
        }

        // ==========================================
        // 8. DEPOSIT / WITHDRAWAL & ADMIN ENGINE
        // ==========================================
        function openDepositModal() {
            if (!currentUser) { openAuthModal('login'); return; }
            document.getElementById('depositModal').classList.remove('hidden');
        }
        function closeDepositModal() { document.getElementById('depositModal').classList.add('hidden'); }

        function handleDepositSubmit(e) {
            e.preventDefault();
            const amt = parseFloat(document.getElementById('depositAmount').value);
            if (isNaN(amt) || amt <= 0) return;
            currentUser.balance += amt;
            saveUsers();
            closeDepositModal();
            showToast(`DEPOSIT SUCCESS: ZMW ${amt.toFixed(2)} credited via Mobile Money`, 'success');
        }

        function openWithdrawModal() {
            if (!currentUser) { openAuthModal('login'); return; }
            document.getElementById('withdrawModal').classList.remove('hidden');
        }
        function closeWithdrawModal() { document.getElementById('withdrawModal').classList.add('hidden'); }

        function handleWithdrawSubmit(e) {
            e.preventDefault();
            const amt = parseFloat(document.getElementById('withdrawAmount').value);
            if (isNaN(amt) || amt <= 0 || currentUser.balance < amt) {
                showToast('Insufficient cash balance', 'error');
                return;
            }
            currentUser.balance -= amt;
            saveUsers();
            closeWithdrawModal();
            showToast(`WITHDRAWAL SENT: ZMW ${amt.toFixed(2)} dispatched to Mobile Money`, 'success');
        }

        function populateAdminDropdowns() {
            const aSel = document.getElementById('adminAssetSelect');
            const uSel = document.getElementById('adminUserSelect');
            if (aSel) {
                aSel.innerHTML = '';
                assets.forEach(a => {
                    const opt = document.createElement('option');
                    opt.value = a.id;
                    opt.innerText = `${a.id} - ${a.name} (ZMW ${a.price.toFixed(2)})`;
                    aSel.appendChild(opt);
                });
            }
            if (uSel) {
                uSel.innerHTML = '';
                users.forEach(u => {
                    const opt = document.createElement('option');
                    opt.value = u.email;
                    opt.innerText = `${u.name} (${u.email}) - ZMW ${u.balance.toFixed(2)}`;
                    uSel.appendChild(opt);
                });
            }
        }

        function adminUpdatePrice() {
            const assetId = document.getElementById('adminAssetSelect').value;
            const newPrice = parseFloat(document.getElementById('adminNewPrice').value);
            if (isNaN(newPrice) || newPrice <= 0) return;

            const asset = assets.find(a => a.id === assetId);
            if (asset) {
                asset.price = newPrice;
                saveAssets();
                renderMarketGrid();
                selectAsset(selectedAssetId);
                showToast(`ADMIN: ${asset.id} spot price override to ZMW ${newPrice.toFixed(2)}`, 'info');
            }
        }

        function adminInjectCash() {
            const email = document.getElementById('adminUserSelect').value;
            const amount = parseFloat(document.getElementById('adminInjectAmount').value);
            if (isNaN(amount) || amount <= 0) return;

            const target = users.find(u => u.email === email);
            if (target) {
                target.balance += amount;
                saveUsers();
                showToast(`ADMIN: ZMW ${amount.toFixed(2)} credited to ${target.email}`, 'info');
            }
        }

        // ==========================================
        // 9. AUTHENTICATION & TOAST HELPERS
        // ==========================================
        function openAuthModal(tab = 'login') {
            document.getElementById('authModal').classList.remove('hidden');
            setAuthTab(tab);
        }
        function closeAuthModal() { document.getElementById('authModal').classList.add('hidden'); }

        function setAuthTab(tab) {
            const formLogin = document.getElementById('formLogin');
            const formRegister = document.getElementById('formRegister');
            const tLogin = document.getElementById('authTabLogin');
            const tReg = document.getElementById('authTabRegister');

            if (tab === 'login') {
                formLogin.classList.remove('hidden');
                formRegister.classList.add('hidden');
                tLogin.className = 'flex-1 py-3 text-sm font-bold text-center border-b-2 border-emerald-500 text-emerald-400';
                tReg.className = 'flex-1 py-3 text-sm font-bold text-center border-b-2 border-transparent text-slate-400 hover:text-slate-200';
            } else {
                formLogin.classList.add('hidden');
                formRegister.classList.remove('hidden');
                tReg.className = 'flex-1 py-3 text-sm font-bold text-center border-b-2 border-emerald-500 text-emerald-400';
                tLogin.className = 'flex-1 py-3 text-sm font-bold text-center border-b-2 border-transparent text-slate-400 hover:text-slate-200';
            }
        }

        function fillDemoCredentials(email, pass) {
            document.getElementById('loginEmail').value = email;
            document.getElementById('loginPassword').value = pass;
        }

        function handleLoginSubmit(e) {
            e.preventDefault();
            const email = document.getElementById('loginEmail').value.trim();
            const pass = document.getElementById('loginPassword').value.trim();
            const found = users.find(u => u.email.toLowerCase() === email.toLowerCase() && u.password === pass);

            if (found) {
                currentUser = found;
                localStorage.setItem('ksx_session_v3', currentUser.email);
                closeAuthModal();
                showToast(`Welcome back, ${currentUser.name}!`, 'success');
                updateUIState();
            } else {
                showToast('Invalid email or password', 'error');
            }
        }

        function handleRegisterSubmit(e) {
            e.preventDefault();
            const name = document.getElementById('regName').value.trim();
            const email = document.getElementById('regEmail').value.trim();
            const nrc = document.getElementById('regNrc').value.trim();
            const password = document.getElementById('regPassword').value.trim();

            if (users.some(u => u.email.toLowerCase() === email.toLowerCase())) {
                showToast('Email address already registered', 'error');
                return;
            }

            const newUser = {
                name, email, password, nrc,
                role: 'USER',
                balance: 5000.00,
                holdings: {}
            };

            users.push(newUser);
            currentUser = newUser;
            localStorage.setItem('ksx_session_v3', currentUser.email);
            saveUsers();
            closeAuthModal();
            showToast(`Account created! Logged in as ${currentUser.name}`, 'success');
        }

        function handleLogout() {
            currentUser = null;
            localStorage.removeItem('ksx_session_v3');
            showToast('Logged out of platform', 'info');
            updateUIState();
        }

        function showToast(msg, type = 'info') {
            const container = document.getElementById('toastContainer');
            const toast = document.createElement('div');
            let bg = 'bg-slate-900 border-slate-700 text-slate-100';
            if (type === 'success') bg = 'bg-emerald-950 border-emerald-600 text-emerald-200';
            if (type === 'error') bg = 'bg-rose-950 border-rose-600 text-rose-200';

            toast.className = `p-3.5 rounded-2xl border shadow-2xl text-xs font-semibold flex items-center gap-2 pointer-events-auto transform transition duration-300 translate-y-2 ${bg}`;
            toast.innerHTML = `
                <i data-lucide="${type === 'success' ? 'check-circle' : type === 'error' ? 'alert-circle' : 'info'}" class="w-4 h-4 shrink-0"></i>
                <span>${msg}</span>
            `;

            container.appendChild(toast);
            lucide.createIcons();
            setTimeout(() => {
                toast.classList.add('opacity-0', '-translate-y-2');
                setTimeout(() => toast.remove(), 300);
            }, 3500);
        }

        function startLiveClock() {
            setInterval(() => {
                const clock = document.getElementById('liveTime');
                if (clock) clock.innerText = new Date().toLocaleTimeString('en-US', { hour12: false }) + ' CAT';
            }, 1000);
        }
    </script>
</body>
</html>

