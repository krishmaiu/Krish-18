# Krish-18
index.html
```html
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ZANE // v18.0 — OS Launchpad</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: JetBrains Mono & Space Grotesk -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:ital,wght@0,300;0,400;0,600;0,700;0,800;1,400&family=Space+Grotesk:wght@400;600;700&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        mono: ['"JetBrains Mono"', 'monospace'],
                        sans: ['"Space Grotesk"', 'sans-serif'],
                    },
                    colors: {
                        void: '#050507',
                        obsidian: '#0B0C10',
                        panel: '#101218',
                        emeraldNeon: '#10B981',
                        cyanNeon: '#06B6D4',
                    },
                    boxShadow: {
                        'neon-emerald': '0 0 15px rgba(16, 185, 129, 0.25), inset 0 0 10px rgba(16, 185, 129, 0.05)',
                        'neon-cyan': '0 0 15px rgba(6, 182, 212, 0.25), inset 0 0 10px rgba(6, 182, 212, 0.05)',
                    }
                }
            }
        }
    </script>

    <style>
        /* Core Cyber Aesthetic & Custom Scrollbars */
        :root {
            --bg-void: #050507;
            --accent-primary: #10b981;
            --accent-secondary: #06b6d4;
        }

        body {
            background-color: var(--bg-void);
            color: #e2e8f0;
            font-family: 'JetBrains Mono', monospace;
            overflow-x: hidden;
            selection-background-color: rgba(16, 185, 129, 0.3);
        }

        ::selection {
            background: rgba(16, 185, 129, 0.35);
            color: #fff;
        }

        /* Subtle CRT Scanline Overlay */
        .scanlines {
            background: linear-gradient(
                to bottom,
                rgba(255, 255, 255, 0),
                rgba(255, 255, 255, 0) 50%,
                rgba(0, 0, 0, 0.28) 50%,
                rgba(0, 0, 0, 0.28)
            );
            background-size: 100% 4px;
            pointer-events: none;
        }

        /* Subtle Grid Background */
        .cyber-grid {
            background-size: 40px 40px;
            background-image: 
                linear-gradient(to right, rgba(16, 185, 129, 0.04) 1px, transparent 1px),
                linear-gradient(to bottom, rgba(6, 182, 212, 0.04) 1px, transparent 1px);
        }

        /* Overclock Mode Animation */
        body.overclock-mode {
            --accent-primary: #f43f5e;
            --accent-secondary: #f59e0b;
        }
        body.overclock-mode .border-emerald-500\/30 {
            border-color: rgba(244, 63, 94, 0.5) !important;
        }
        body.overclock-mode .text-emerald-400 {
            color: #fb7185 !important;
        }
        body.overclock-mode .text-cyan-400 {
            color: #fbbf24 !important;
        }

        /* Terminal Scrollbar */
        .terminal-scroll::-webkit-scrollbar {
            width: 6px;
        }
        .terminal-scroll::-webkit-scrollbar-track {
            background: #08090c;
        }
        .terminal-scroll::-webkit-scrollbar-thumb {
            background: #1f2937;
            border-radius: 3px;
        }
        .terminal-scroll::-webkit-scrollbar-thumb:hover {
            background: #10b981;
        }

        /* Blinking Cursor */
        @keyframes blink {
            0%, 49% { opacity: 1; }
            50%, 100% { opacity: 0; }
        }
        .cursor-blink {
            animation: blink 1s infinite;
        }

        /* Glitch Hover Effect */
        .glitch-text:hover {
            text-shadow: 2px 0 #06b6d4, -2px 0 #10b981;
        }
    </style>
</head>
<body class="min-h-screen bg-void text-slate-200 cyber-grid relative flex flex-col justify-between">

    <!-- Background Matrix Rain Canvas -->
    <canvas id="matrix-canvas" class="fixed inset-0 w-full h-full pointer-events-none opacity-0 transition-opacity duration-700 z-0"></canvas>

    <!-- Foreground Celebration Particle/Confetti Canvas -->
    <canvas id="fx-canvas" class="fixed inset-0 w-full h-full pointer-events-none z-40"></canvas>

    <!-- CRT Scanline Layer -->
    <div class="fixed inset-0 scanlines z-30 opacity-60"></div>

    <!-- BOOT SEQUENCE OVERLAY -->
    <div id="boot-screen" class="fixed inset-0 bg-void z-50 flex flex-col justify-center items-start p-6 sm:p-16 font-mono transition-opacity duration-700">
        <div class="max-w-2xl w-full mx-auto space-y-2 text-xs sm:text-sm">
            <div class="text-emerald-500 font-bold tracking-widest mb-4 flex items-center gap-2">
                <span class="inline-block w-2.5 h-2.5 bg-emerald-400 rounded-full animate-ping"></span>
                ONYX-BIOS v18.0.26 // SECURE BOOT
            </div>
            <div id="boot-logs" class="space-y-1.5 text-slate-300"></div>
            <div class="pt-4">
                <div class="w-full bg-slate-900 h-1.5 rounded-full overflow-hidden border border-slate-800">
                    <div id="boot-progress" class="bg-gradient-to-r from-emerald-500 to-cyan-400 h-full w-0 transition-all duration-300"></div>
                </div>
            </div>
            <button id="skip-boot-btn" class="mt-6 text-[11px] text-slate-500 hover:text-cyan-400 underline underline-offset-4 cursor-pointer">
                [PRESS ANY KEY OR CLICK TO BYPASS SEQUENCE]
            </button>
        </div>
    </div>

    <!-- MAIN HUD INTERFACE -->
    <main id="main-hud" class="relative z-10 max-w-6xl w-full mx-auto px-4 sm:px-6 py-6 sm:py-8 space-y-6 opacity-0 transition-opacity duration-700">
        
        <!-- Top Status Bar -->
        <header class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 bg-obsidian/90 backdrop-blur-md border border-emerald-500/30 rounded-xl p-4 shadow-neon-emerald">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-lg bg-emerald-500/10 border border-emerald-500/40 flex items-center justify-center text-emerald-400 font-bold text-lg shadow-inner">
                    Z18
                </div>
                <div>
                    <div class="flex items-center gap-2">
                        <h1 class="font-sans font-bold text-lg sm:text-xl tracking-wider text-white glitch-text">
                            ZANE <span class="text-emerald-400">//</span> v18.0
                        </h1>
                        <span class="px-2 py-0.5 text-[10px] font-bold uppercase tracking-widest bg-emerald-500/15 text-emerald-400 border border-emerald-500/30 rounded-full">
                            LEVEL 18 UNLOCKED
                        </span>
                    </div>
                    <p class="text-xs text-slate-400 mt-0.5">
                        OPERATOR: <span class="text-cyan-400 font-semibold">KRISH</span> &bull; ORIGIN: <span class="text-slate-300">NOV 30, 2008</span> &rarr; <span class="text-emerald-400">2026</span>
                    </p>
                </div>
            </div>

            <!-- Quick Action Toggles -->
            <div class="flex flex-wrap items-center gap-2">
                <button onclick="executeCommand('gift')" class="px-3 py-1.5 text-xs font-semibold bg-emerald-500/15 hover:bg-emerald-500/25 text-emerald-300 border border-emerald-500/40 rounded-lg transition flex items-center gap-1.5 cursor-pointer">
                    <span>⚡</span> UNLOCK GIFT
                </button>
                <button onclick="executeCommand('matrix')" id="btn-matrix" class="px-3 py-1.5 text-xs font-semibold bg-slate-900 hover:bg-slate-800 text-cyan-400 border border-cyan-500/30 rounded-lg transition cursor-pointer">
                    MATRIX: OFF
                </button>
                <button onclick="executeCommand('music')" id="btn-music" class="px-3 py-1.5 text-xs font-semibold bg-slate-900 hover:bg-slate-800 text-slate-300 border border-slate-700 rounded-lg transition cursor-pointer">
                    ♪ SYNTH: OFF
                </button>
            </div>
        </header>

        <!-- Primary Grid: Countdown + System Telemetry -->
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            
            <!-- Countdown / Uptime Module (7 Cols) -->
            <section class="lg:col-span-7 bg-obsidian/90 backdrop-blur-md border border-cyan-500/30 rounded-xl p-5 sm:p-6 shadow-neon-cyan flex flex-col justify-between relative overflow-hidden">
                <div class="flex items-center justify-between mb-4">
                    <span class="text-xs uppercase tracking-widest text-cyan-400 font-semibold flex items-center gap-2">
                        <span class="w-2 h-2 rounded-full bg-cyan-400 animate-pulse"></span>
                        <span id="timer-label">T-MINUS TO v18.0 DEPLOYMENT (IST)</span>
                    </span>
                    <span class="text-[11px] text-slate-500">TARGET: 30.11.2026 00:00</span>
                </div>

                <!-- 4-Block Digital Readout -->
                <div class="grid grid-cols-4 gap-2 sm:gap-4 my-2">
                    <div class="bg-void/90 border border-slate-800 rounded-lg p-3 sm:p-4 text-center">
                        <div id="cd-days" class="text-2xl sm:text-4xl font-bold text-white tracking-tight">00</div>
                        <div class="text-[10px] sm:text-xs text-slate-500 uppercase tracking-widest mt-1">Days</div>
                    </div>
                    <div class="bg-void/90 border border-slate-800 rounded-lg p-3 sm:p-4 text-center">
                        <div id="cd-hours" class="text-2xl sm:text-4xl font-bold text-cyan-400 tracking-tight">00</div>
                        <div class="text-[10px] sm:text-xs text-slate-500 uppercase tracking-widest mt-1">Hours</div>
                    </div>
                    <div class="bg-void/90 border border-slate-800 rounded-lg p-3 sm:p-4 text-center">
                        <div id="cd-mins" class="text-2xl sm:text-4xl font-bold text-emerald-400 tracking-tight">00</div>
                        <div class="text-[10px] sm:text-xs text-slate-500 uppercase tracking-widest mt-1">Mins</div>
                    </div>
                    <div class="bg-void/90 border border-slate-800 rounded-lg p-3 sm:p-4 text-center">
                        <div id="cd-secs" class="text-2xl sm:text-4xl font-bold text-white tracking-tight">00</div>
                        <div class="text-[10px] sm:text-xs text-slate-500 uppercase tracking-widest mt-1">Secs</div>
                    </div>
                </div>

                <div class="mt-4 pt-3 border-t border-slate-800/80 flex flex-wrap items-center justify-between gap-2 text-xs text-slate-400">
                    <span id="timer-subtext">STATUS: FINALIZING 17.x KERNEL PATCHES...</span>
                    <span class="text-emerald-400 font-semibold">NODE: BHOPAL // IN</span>
                </div>
            </section>

            <!-- Operator Specs & Telemetry (5 Cols) -->
            <section class="lg:col-span-5 bg-obsidian/90 backdrop-blur-md border border-slate-800 rounded-xl p-5 sm:p-6 flex flex-col justify-between space-y-4">
                <div class="flex items-center justify-between">
                    <span class="text-xs uppercase tracking-widest text-emerald-400 font-semibold">OPERATOR TELEMETRY</span>
                    <span id="sys-mode-badge" class="text-[10px] px-2 py-0.5 rounded bg-slate-800 text-slate-300">NOMINAL</span>
                </div>

                <div class="space-y-3 text-xs">
                    <div>
                        <div class="flex justify-between text-slate-400 mb-1">
                            <span>AGENTIC AI &amp; CODE ARCHITECTURE</span>
                            <span class="text-emerald-400">94%</span>
                        </div>
                        <div class="w-full bg-void h-1.5 rounded-full overflow-hidden border border-slate-800">
                            <div class="bg-emerald-500 h-full w-[94%]"></div>
                        </div>
                    </div>
                    <div>
                        <div class="flex justify-between text-slate-400 mb-1">
                            <span>CLASS 12 / EXAM OVERDRIVE</span>
                            <span class="text-cyan-400">88%</span>
                        </div>
                        <div class="w-full bg-void h-1.5 rounded-full overflow-hidden border border-slate-800">
                            <div class="bg-cyan-400 h-full w-[88%]"></div>
                        </div>
                    </div>
                    <div>
                        <div class="flex justify-between text-slate-400 mb-1">
                            <span>ONYX EDGE // CREATOR ENGINE</span>
                            <span class="text-emerald-400">91%</span>
                        </div>
                        <div class="w-full bg-void h-1.5 rounded-full overflow-hidden border border-slate-800">
                            <div class="bg-gradient-to-r from-emerald-500 to-cyan-400 h-full w-[91%]"></div>
                        </div>
                    </div>
                </div>

                <div class="bg-void/80 border border-slate-800/90 rounded-lg p-3 text-[11px] text-slate-400 leading-relaxed">
                    <span class="text-cyan-400 font-semibold">&gt; DIRECTIVE:</span> Execute <code class="text-emerald-400 bg-emerald-500/10 px-1 rounded">quest</code> in the terminal below to track your v18.0 milestones, or run <code class="text-emerald-400 bg-emerald-500/10 px-1 rounded">gift</code> to decrypt your birthday payload.
                </div>
            </section>
        </div>

        <!-- Dynamic Interactive Drawer (Revealed when 'gift' or 'quest' is triggered) -->
        <section id="dynamic-panel" class="hidden bg-obsidian/95 backdrop-blur-md border border-emerald-500/40 rounded-xl p-5 sm:p-6 shadow-neon-emerald transition-all">
            <div class="flex items-center justify-between border-b border-slate-800 pb-3 mb-4">
                <h2 id="panel-title" class="text-sm font-bold uppercase tracking-widest text-emerald-400">MODULE OUTPUT</h2>
                <button onclick="closeDynamicPanel()" class="text-xs text-slate-400 hover:text-white px-2 py-1 rounded bg-slate-900 border border-slate-700 cursor-pointer">
                    [ESC / CLOSE]
                </button>
            </div>
            <div id="panel-content"></div>
        </section>

        <!-- Interactive Command-Line Interface (CLI) -->
        <section class="bg-obsidian/95 backdrop-blur-md border border-slate-800 focus-within:border-emerald-500/50 rounded-xl shadow-2xl overflow-hidden transition-colors">
            <!-- Terminal Header Bar -->
            <div class="bg-panel px-4 py-2.5 border-b border-slate-800 flex items-center justify-between">
                <div class="flex items-center gap-2">
                    <span class="w-3 h-3 rounded-full bg-rose-500/80"></span>
                    <span class="w-3 h-3 rounded-full bg-amber-500/80"></span>
                    <span class="w-3 h-3 rounded-full bg-emerald-500/80"></span>
                    <span class="ml-2 text-xs text-slate-400 font-semibold">zane@onyx-os:~ (bash)</span>
                </div>
                <div class="flex flex-wrap gap-1.5">
                    <button onclick="executeCommand('help')" class="text-[11px] px-2 py-0.5 rounded bg-slate-800/80 hover:bg-slate-700 text-slate-300 cursor-pointer">help</button>
                    <button onclick="executeCommand('gift')" class="text-[11px] px-2 py-0.5 rounded bg-emerald-500/15 hover:bg-emerald-500/25 text-emerald-400 cursor-pointer">gift</button>
                    <button onclick="executeCommand('quest')" class="text-[11px] px-2 py-0.5 rounded bg-cyan-500/15 hover:bg-cyan-500/25 text-cyan-400 cursor-pointer">quest</button>
                    <button onclick="executeCommand('overclock')" class="text-[11px] px-2 py-0.5 rounded bg-slate-800/80 hover:bg-slate-700 text-slate-300 cursor-pointer">overclock</button>
                    <button onclick="executeCommand('clear')" class="text-[11px] px-2 py-0.5 rounded bg-slate-800/80 hover:bg-slate-700 text-slate-400 cursor-pointer">clear</button>
                </div>
            </div>

            <!-- Terminal Output Log -->
            <div id="terminal-output" class="p-4 h-64 sm:h-72 overflow-y-auto terminal-scroll space-y-2 text-xs sm:text-sm font-mono">
                <div class="text-emerald-400">Welcome to ZANE OS v18.0 [Kernel 2026.11.30-IST]</div>
                <div class="text-slate-400">Type <span class="text-cyan-400 font-semibold">'help'</span> to view available commands, or type <span class="text-emerald-400 font-semibold">'gift'</span> to unlock your 18th birthday surprise.</div>
            </div>

            <!-- Terminal Input Prompt -->
            <form id="terminal-form" class="px-4 py-3 bg-void/90 border-t border-slate-800/90 flex items-center gap-2">
                <span class="text-emerald-400 font-bold text-xs sm:text-sm select-none">zane@v18:~$</span>
                <input 
                    type="text" 
                    id="terminal-input" 
                    autocomplete="off" 
                    spellcheck="false"
                    placeholder="Enter command (help, gift, quest, matrix, overclock, music, clear)..." 
                    class="w-full bg-transparent text-white placeholder-slate-600 text-xs sm:text-sm focus:outline-none font-mono"
                />
            </form>
        </section>

    </main>

    <!-- Footer -->
    <footer class="relative z-10 max-w-6xl w-full mx-auto px-4 sm:px-6 py-4 text-center sm:flex sm:items-center sm:justify-between text-[11px] text-slate-500">
        <div>BUILT FOR KRISH (ZANE) // GITHUB PAGES READY</div>
        <div class="mt-1 sm:mt-0">NOV 30, 2008 &mdash; NOV 30, 2026 &bull; ALL SYSTEMS OPERATIONAL</div>
    </footer>

    <!-- JAVASCRIPT ENGINE -->
    <script>
        /* ==========================================================================
           1. BOOT SEQUENCE CONTROLLER
           ========================================================================== */
        const bootMessages = [
            "[OK] Initializing ONYX-KERNEL v18.0...",
            "[OK] Verifying Operator Identity: KRISH // ALIAS: ZANE",
            "[OK] Loading Temporal Coordinates: NOV 30, 2008 -> NOV 30, 2026 (IST)",
            "[OK] Mounting Agentic Workflows & Neural Modules...",
            "[OK] Checking Milestone Registry... [LEVEL 18 READY]",
            "[SUCCESS] Launching ZANE // v18.0 Interactive HUD..."
        ];

        const bootScreen = document.getElementById('boot-screen');
        const bootLogs = document.getElementById('boot-logs');
        const bootProgress = document.getElementById('boot-progress');
        const mainHud = document.getElementById('main-hud');
        let bootComplete = false;

        function finishBoot() {
            if (bootComplete) return;
            bootComplete = true;
            bootScreen.classList.add('opacity-0');
            setTimeout(() => {
                bootScreen.style.display = 'none';
                mainHud.classList.remove('opacity-0');
                document.getElementById('terminal-input').focus();
            }, 600);
        }

        function runBootSequence() {
            let i = 0;
            const interval = setInterval(() => {
                if (bootComplete) {
          
