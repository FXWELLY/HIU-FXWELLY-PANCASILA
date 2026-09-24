# HIU-FXWELLY-PANCASILA
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Petualangan Pancasila Bawah Laut - Kelas 7 SMP</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;600;700&family=Nunito:wght@400;700;800&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        body {
            font-family: 'Nunito', sans-serif;
            user-select: none;
            overflow: hidden;
            background-color: #03182e;
        }
        .font-fredoka {
            font-family: 'Fredoka', cursive;
        }
        canvas {
            display: block;
            touch-action: none;
        }
        .glass-panel {
            background: rgba(8, 38, 74, 0.85);
            backdrop-filter: blur(12px);
            border: 2px solid rgba(56, 189, 248, 0.3);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
        }
        .btn-bubbles {
            position: relative;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .btn-bubbles:hover {
            transform: translateY(-3px) scale(1.02);
            box-shadow: 0 10px 20px rgba(14, 165, 233, 0.4);
        }
        .btn-bubbles:active {
            transform: translateY(1px);
        }
        /* Certificate Styling */
        @media print {
            body * {
                visibility: hidden;
            }
            #certificateContainer, #certificateContainer * {
                visibility: visible;
            }
            #certificateContainer {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
                height: 100%;
            }
        }
    </style>
</head>
<body class="relative w-screen h-screen text-white">

    <!-- Main Canvas element for Ocean Simulation & Game -->
    <canvas id="gameCanvas" class="absolute inset-0 w-full h-full z-0"></canvas>

    <!-- START SCREEN OVERLAY -->
    <div id="startScreen" class="absolute inset-0 z-20 flex items-center justify-center p-4">
        <div class="glass-panel p-6 md:p-10 rounded-3xl max-w-lg w-full text-center border-4 border-cyan-400/40 shadow-2xl">
            <div class="inline-block p-3 bg-cyan-500/20 rounded-full mb-3 text-cyan-300">
                <i class="fa-solid fa-water-ladder text-5xl animate-bounce"></i>
            </div>
            <h1 class="text-3xl md:text-4xl font-fredoka font-bold text-transparent bg-clip-text bg-gradient-to-r from-cyan-300 via-teal-200 to-amber-300 drop-shadow mb-2">
                Petualangan Bawah Laut
            </h1>
            <p class="text-cyan-200 text-sm md:text-base mb-6 font-semibold">
                Mata Pelajaran Pancasila • Kelas 7 SMP
            </p>

            <div class="bg-blue-950/60 p-4 rounded-2xl mb-6 text-left border border-cyan-500/20 text-xs md:text-sm text-cyan-100 space-y-2">
                <p class="font-bold text-amber-300 text-center mb-1">🎮 Misi Kamu:</p>
                <p><i class="fa-solid fa-fish text-cyan-400 mr-2"></i>Kendalikan <b>Lumba-Lumba Lucu</b> menggunakan <b>Panah/WASD</b> atau <b>Sentuh/Geser Layar</b>.</p>
                <p><i class="fa-solid fa-ghost text-teal-300 mr-2"></i>Tabrak <b>10 Ikan Hiu Lucu</b> untuk membuka Soal Kuis Pancasila!</p>
                <p><i class="fa-solid fa-trophy text-amber-400 mr-2"></i>Setiap kuis bernilai 10 Poin. Raih 100 Poin untuk dapatkan Piala Emas & Sertifikat Kelulusan!</p>
            </div>

            <div class="mb-6 text-left">
                <label for="playerName" class="block text-cyan-200 text-xs font-bold mb-2 uppercase tracking-wide">Nama Lengkap Siswa:</label>
                <input type="text" id="playerName" placeholder="Masukkan nama kamu di sini..." 
                    class="w-full px-4 py-3 bg-blue-900/80 border-2 border-cyan-400/50 rounded-xl text-white placeholder-cyan-400/50 focus:outline-none focus:border-amber-400 text-sm md:text-base font-bold transition">
            </div>

            <button onclick="startGame()" class="btn-bubbles w-full py-4 bg-gradient-to-r from-amber-400 to-amber-500 hover:from-amber-300 hover:to-amber-400 text-blue-950 font-fredoka text-xl font-bold rounded-2xl shadow-lg border-2 border-amber-200 flex items-center justify-center gap-3">
                <i class="fa-solid fa-play"></i> Mulai Petualangan!
            </button>
        </div>
    </div>

    <!-- HUD OVERLAY (In-Game) -->
    <div id="gameHUD" class="hidden absolute top-0 left-0 right-0 z-10 p-4 pointer-events-none flex justify-between items-start">
        <!-- Player Info & Score -->
        <div class="glass-panel px-4 py-2 rounded-2xl flex items-center gap-4 border border-cyan-400/30">
            <div class="flex items-center gap-2">
                <div class="w-10 h-10 rounded-full bg-amber-400/20 border border-amber-400 flex items-center justify-center text-amber-300">
                    <i class="fa-solid fa-user-graduate"></i>
                </div>
                <div>
                    <span id="hudPlayerName" class="block font-bold text-xs md:text-sm text-cyan-200">Siswa</span>
                    <span class="text-xs text-cyan-400">Kelas 7 SMP</span>
                </div>
            </div>
            <div class="h-8 w-px bg-cyan-500/30"></div>
            <div>
                <span class="block text-[10px] text-cyan-300 uppercase tracking-wider font-bold">Skor Kamu</span>
                <span id="hudScore" class="text-2xl font-fredoka font-bold text-amber-300">0 / 100</span>
            </div>
        </div>

        <!-- Quiz Progress -->
        <div class="glass-panel px-4 py-2 rounded-2xl flex items-center gap-3 border border-cyan-400/30">
            <i class="fa-solid fa-shield-halved text-2xl text-cyan-300"></i>
            <div>
                <span class="block text-[10px] text-cyan-300 uppercase tracking-wider font-bold">Hiu Ditabrak</span>
                <span id="hudProgress" class="text-lg font-fredoka font-bold text-cyan-100">0 dari 10</span>
            </div>
        </div>
    </div>

    <!-- Touch Mobile Joystick Hint -->
    <div id="touchControlsHint" class="hidden md:hidden absolute bottom-6 left-1/2 transform -translate-x-1/2 z-10 text-xs text-cyan-200/70 bg-blue-950/60 px-4 py-2 rounded-full border border-cyan-500/20 pointer-events-none">
        👆 Sentuh & Geser layar untuk menggerakkan lumba-lumba
    </div>

    <!-- QUIZ MODAL OVERLAY -->
    <div id="quizModal" class="hidden absolute inset-0 z-30 flex items-center justify-center p-4 bg-blue-950/80 backdrop-blur-md">
        <div class="glass-panel p-6 md:p-8 rounded-3xl max-w-2xl w-full border-4 border-cyan-400/50 shadow-2xl relative animate-fadeIn">
            <!-- Badge Header -->
            <div class="flex items-center justify-between mb-4 pb-3 border-b border-cyan-500/30">
                <div class="flex items-center gap-2">
                    <span class="bg-amber-400 text-blue-950 font-bold px-3 py-1 rounded-full text-xs font-fredoka" id="quizNumberBadge">
                        Soal #1
                    </span>
                    <span class="text-xs text-cyan-300 font-semibold">Mata Pelajaran Pancasila</span>
                </div>
                <div class="text-amber-300 text-xs font-bold flex items-center gap-1">
                    <i class="fa-solid fa-star"></i> Poin: +10
                </div>
            </div>

            <!-- Question Text -->
            <div class="mb-6">
                <h2 id="questionText" class="text-base md:text-xl font-bold text-cyan-50 leading-relaxed">
                    Pertanyaan akan muncul di sini...
                </h2>
            </div>

            <!-- Options Grid -->
            <div id="optionsContainer" class="grid grid-cols-1 md:grid-cols-2 gap-3 mb-6">
                <!-- Options JS Injected -->
            </div>

            <!-- Result Feedback Box -->
            <div id="feedbackBox" class="hidden p-3 rounded-xl mb-4 text-center font-bold text-sm"></div>

            <!-- Action Button -->
            <button id="nextBtn" onclick="closeQuizModal()" class="hidden btn-bubbles w-full py-3 bg-gradient-to-r from-cyan-400 to-blue-500 hover:from-cyan-300 hover:to-blue-400 text-blue-950 font-fredoka font-bold text-base rounded-xl transition">
                Lanjutkan Petualangan <i class="fa-solid fa-arrow-right ml-1"></i>
            </button>
        </div>
    </div>

    <!-- VICTORY / RESULTS MODAL -->
    <div id="victoryModal" class="hidden absolute inset-0 z-30 flex items-center justify-center p-4 bg-blue-950/90 backdrop-blur-md">
        <div class="glass-panel p-6 md:p-8 rounded-3xl max-w-lg w-full text-center border-4 border-amber-400/60 shadow-2xl relative">
            
            <!-- Floating Trophy Graphic -->
            <div class="relative inline-block my-2">
                <div class="absolute inset-0 bg-amber-400/20 rounded-full blur-xl animate-pulse"></div>
                <i class="fa-solid fa-trophy text-7xl md:text-8xl text-amber-400 drop-shadow-lg relative z-10"></i>
            </div>

            <h2 class="text-3xl font-fredoka font-bold text-amber-300 mb-1">Misi Selesai!</h2>
            <p class="text-cyan-200 text-sm mb-4">Selamat, kamu telah menjelajahi seluruh lautan Pancasila!</p>

            <!-- Score Summary Card -->
            <div class="bg-blue-900/60 p-4 rounded-2xl border border-cyan-400/30 mb-4">
                <div class="text-xs text-cyan-300 font-bold uppercase mb-1">Nilai Akhir Kamu:</div>
                <div id="finalScoreDisplay" class="text-5xl font-fredoka font-bold text-amber-300 my-1">100</div>
                <p id="gradePredicate" class="text-sm font-bold text-teal-300 mt-2">Sangat Memuaskan! Ksatria Pancasila Utama</p>
            </div>

            <!-- Input Name for Certificate -->
            <div class="mb-5 text-left bg-blue-950/70 p-3 rounded-2xl border border-amber-400/30">
                <label for="victoryPlayerName" class="block text-amber-300 text-xs font-bold mb-1 uppercase tracking-wide flex items-center gap-1">
                    <i class="fa-solid fa-pen-to-square"></i> Nama Lengkap untuk Sertifikat:
                </label>
                <input type="text" id="victoryPlayerName" placeholder="Masukkan nama kamu di sini..." 
                    class="w-full px-3 py-2 bg-blue-900/90 border border-cyan-400/50 rounded-xl text-white placeholder-cyan-400/50 focus:outline-none focus:border-amber-400 text-sm font-bold transition">
            </div>

            <div class="flex flex-col md:flex-row gap-3">
                <button onclick="openCertificateModal()" class="btn-bubbles flex-1 py-3 bg-gradient-to-r from-amber-400 to-amber-500 text-blue-950 font-fredoka font-bold text-sm md:text-base rounded-xl flex items-center justify-center gap-2">
                    <i class="fa-solid fa-certificate"></i> Lihat Sertifikat
                </button>
                <button onclick="restartGame()" class="btn-bubbles flex-1 py-3 bg-blue-800 hover:bg-blue-700 text-cyan-200 font-fredoka font-bold text-sm md:text-base rounded-xl border border-cyan-400/30 flex items-center justify-center gap-2">
                    <i class="fa-solid fa-rotate-right"></i> Main Lagi
                </button>
            </div>
        </div>
    </div>

    <!-- CERTIFICATE MODAL -->
    <div id="certificateModal" class="hidden absolute inset-0 z-40 flex items-center justify-center p-4 bg-black/80 backdrop-blur-md overflow-y-auto">
        <div class="relative max-w-3xl w-full my-auto">
            <!-- Close Button -->
            <button onclick="closeCertificateModal()" class="absolute -top-10 right-0 text-white hover:text-amber-300 text-2xl font-bold">
                <i class="fa-solid fa-xmark"></i> Tutup
            </button>

            <!-- Printable Certificate Paper -->
            <div id="certificateContainer" class="bg-amber-50 text-slate-900 p-8 md:p-12 rounded-2xl border-8 border-amber-600 shadow-2xl relative overflow-hidden font-serif">
                <!-- Watermark Decor -->
                <div class="absolute inset-0 opacity-5 flex items-center justify-center pointer-events-none">
                    <i class="fa-solid fa-shield-halved text-[280px] text-amber-900"></i>
                </div>
                
                <!-- Certificate Border Lines -->
                <div class="border-2 border-amber-800/40 p-6 md:p-8 rounded-lg relative z-10 text-center">
                    
                    <!-- Header Icon -->
                    <div class="flex justify-center mb-3">
                        <div class="w-16 h-16 rounded-full bg-amber-100 border-2 border-amber-700 flex items-center justify-center text-amber-800 text-3xl">
                            <i class="fa-solid fa-award"></i>
                        </div>
                    </div>

                    <h2 class="text-2xl md:text-3xl font-bold tracking-widest text-amber-900 uppercase font-fredoka mb-1">
                        Sertifikat Penghargaan
                    </h2>
                    <p class="text-xs md:text-sm text-amber-800 font-sans tracking-wide uppercase font-semibold mb-6">
                        Petualangan Bawah Laut Mata Pelajaran Pancasila Kelas 7 SMP
                    </p>

                    <p class="text-sm text-slate-700 italic mb-2">Diberikan dengan bangga kepada:</p>
                    
                    <!-- Student Name -->
                    <h1 id="certStudentName" class="text-2xl md:text-4xl font-bold text-blue-950 underline decoration-amber-500 decoration-2 underline-offset-8 my-3 font-fredoka">
                        Nama Siswa
                    </h1>

                    <p class="text-xs md:text-sm text-slate-700 max-w-lg mx-auto leading-relaxed my-4 font-sans">
                        Telah berhasil menyelesaikan tantangan eksplorasi kuis edukatif bawah laut dan menguasai nilai-nilai dasar Pancasila, Norma, dan Undang-Undang Dasar 1945 dengan raihan nilai:
                    </p>

                    <!-- Score Badge -->
                    <div class="inline-block bg-amber-100 border-2 border-amber-600 px-6 py-2 rounded-full my-2">
                        <span class="text-xs text-amber-900 font-sans font-bold uppercase mr-2">Nilai Akhir:</span>
                        <span id="certScore" class="text-2xl font-bold text-amber-800 font-fredoka">100 / 100</span>
                    </div>

                    <!-- Date and Seal -->
                    <div class="mt-8 pt-6 border-t border-amber-800/20 flex justify-between items-end text-xs text-slate-600 font-sans">
                        <div class="text-left">
                            <p>Tanggal: <span id="certDate">24 September 2026</span></p>
                            <p>Status: <span class="text-emerald-700 font-bold">LULUS DENGAN PUJIAN</span></p>
                        </div>
                        <div class="text-center">
                            <div class="w-16 h-16 mx-auto mb-1 rounded-full border-2 border-dashed border-amber-700 flex items-center justify-center text-amber-800 text-xs font-bold">
                                Ksatria Pancasila
                            </div>
                            <p class="font-bold text-slate-800">Panitia Edukasi</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Print Action Button -->
            <div class="mt-4 text-center">
                <button onclick="window.print()" class="px-6 py-3 bg-amber-400 hover:bg-amber-300 text-blue-950 font-bold font-fredoka rounded-xl shadow-lg inline-flex items-center gap-2">
                    <i class="fa-solid fa-print"></i> Cetak / Simpan Sertifikat
                </button>
            </div>
        </div>
    </div>

    <script>
        // Web Audio API Sound Synthesizer
        const AudioEngine = {
            ctx: null,
            init() {
                if (!this.ctx) {
                    this.ctx = new (window.AudioContext || window.webkitAudioContext)();
                }
            },
            playBubbleSound() {
                if (!this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(300, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(800, this.ctx.currentTime + 0.15);
                gain.gain.setValueAtTime(0.15, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.15);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start();
                osc.stop(this.ctx.currentTime + 0.15);
            },
            playCorrectSound() {
                if (!this.ctx) return;
                const notes = [523.25, 659.25, 783.99, 1046.50]; // C5, E5, G5, C6
                notes.forEach((freq, index) => {
                    const osc = this.ctx.createOscillator();
                    const gain = this.ctx.createGain();
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(freq, this.ctx.currentTime + index * 0.08);
                    gain.gain.setValueAtTime(0.2, this.ctx.currentTime + index * 0.08);
                    gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + index * 0.08 + 0.2);
                    osc.connect(gain);
                    gain.connect(this.ctx.destination);
                    osc.start(this.ctx.currentTime + index * 0.08);
                    osc.stop(this.ctx.currentTime + index * 0.08 + 0.2);
                });
            },
            playWrongSound() {
                if (!this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(180, this.ctx.currentTime);
                osc.frequency.linearRampToValueAtTime(110, this.ctx.currentTime + 0.3);
                gain.gain.setValueAtTime(0.2, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.3);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start();
                osc.stop(this.ctx.currentTime + 0.3);
            }
        };

        // 10 Quiz Questions for Pancasila Grade 7 SMP
        const pancasilaQuizzes = [
            {
                q: "Siapakah tokoh bangsa yang menyampaikan usulan lima dasar negara dan memperkenalkan istilah 'Pancasila' pada tanggal 1 Juni 1945?",
                options: ["Drs. Mohammad Hatta", "Ir. Soekarno", "Mr. Soepomo", "Muhammad Yamin"],
                answer: 1
            },
            {
                q: "Rumusan resmi dan sah Pancasila sebagai dasar negara Republik Indonesia tercantum di dalam...",
                options: ["Batang Tubuh UUD 1945", "Pembukaan UUD 1945 Alinea Ke-4", "Piagam Jakarta", "Dekrit Presiden 5 Juli 1959"],
                answer: 1
            },
            {
                q: "Sikap saling menghormati dan bekerja sama antarumat beragama merupakan wujud pengamalan Pancasila sila ke-...",
                options: ["Sila Pertama (1)", "Sila Kedua (2)", "Sila Ketiga (3)", "Sila Kelima (5)"],
                answer: 0
            },
            {
                q: "Lambang Rantai Emas berbentuk lingkaran dan segi empat yang saling berkait pada dada Garuda Pancasila melambangkan sila...",
                options: ["Ketiga", "Kedua", "Keempat", "Kesatu"],
                answer: 1
            },
            {
                q: "Mengutamakan musyawarah untuk mencapai mufakat dalam menyelesaikan masalah bersama sesuai dengan nilai Pancasila sila ke-...",
                options: ["2", "3", "4", "5"],
                answer: 2
            },
            {
                q: "Lembaga pendiri bangsa yang merumuskan dan mengesahkan UUD 1945 serta menetapkan Presiden dan Wapres pada 18 Agustus 1945 adalah...",
                options: ["BPUPKI", "PPKI", "DPR-GR", "MPRS"],
                answer: 1
            },
            {
                q: "Menjaga keseimbangan antara hak dan kewajiban serta bersikap adil kepada seluruh teman sekolah merupakan nilai sila ke-...",
                options: ["Ke-2", "Ke-3", "Ke-4", "Ke-5"],
                answer: 3
            },
            {
                q: "Semboyan 'Bhinneka Tunggal Ika' yang cengkeram oleh kaki Burung Garuda Pancasila bermakna...",
                options: ["Berbeda-beda tetapi tetap satu jua", "Bersatu kita teguh bercerai kita runtuh", "Keberagaman adalah persaingan", "Berbeda-beda namun tetap berpisah"],
                answer: 0
            },
            {
                q: "Kaidah atau aturan hidup yang mengikat warga masyarakat dan dijadikan pedoman dalam bertingkah laku disebut...",
                options: ["Hukum Adat", "Norma", "Kebiasaan Local", "Statuta"],
                answer: 1
            },
            {
                q: "Menjaga persatuan dan kesatuan bangsa serta rasa cinta tanah air di atas kepentingan pribadi/golongan adalah penerapan sila ke-...",
                options: ["Sila Pertama", "Sila Kedua", "Sila Ketiga", "Sila Keempat"],
                answer: 2
            }
        ];

        let canvas, ctx;
        let playerName = "Siswa";
        let score = 0;
        let quizzesAnswered = 0;
        let activeQuizIndex = null;
        let gameRunning = false;

        // Player Object (Friendly Explorer Fish)
        const player = {
            x: 100,
            y: 300,
            radius: 24,
            speed: 5,
            vx: 0,
            vy: 0,
            angle: 0,
            color: '#fbbf24'
        };

        // Inputs
        const keys = {};
        let touchTarget = null;

        // Entities arrays
        let seaweedList = [];
        let bubblesList = [];
        let smallFishType1 = [];
        let smallFishType2 = [];
        let sharkQuizzes = [];

        function initCanvas() {
            canvas = document.getElementById('gameCanvas');
            ctx = canvas.getContext('2d');
            resizeCanvas();
            window.addEventListener('resize', resizeCanvas);
        }

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            generateBackgroundElements();
        }

        function generateBackgroundElements() {
            // Generate Seaweed at bottom
            seaweedList = [];
            const count = Math.floor(canvas.width / 40);
            for (let i = 0; i < count; i++) {
                seaweedList.push({
                    x: i * 40 + Math.random() * 20,
                    height: 120 + Math.random() * 140,
                    width: 12 + Math.random() * 10,
                    phase: Math.random() * Math.PI * 2,
                    color: Math.random() > 0.5 ? '#10b981' : '#059669'
                });
            }

            // Generate Bubbles
            bubblesList = [];
            for (let i = 0; i < 35; i++) {
                bubblesList.push({
                    x: Math.random() * canvas.width,
                    y: Math.random() * canvas.height,
                    radius: 2 + Math.random() * 6,
                    speed: 0.5 + Math.random() * 1.5,
                    opacity: 0.2 + Math.random() * 0.5
                });
            }

            // Generate Type 1 Small Fish (Yellow/Orange Angel Fish)
            smallFishType1 = [];
            for (let i = 0; i < 8; i++) {
                smallFishType1.push({
                    x: Math.random() * canvas.width,
                    y: 80 + Math.random() * (canvas.height - 200),
                    speed: 1 + Math.random() * 1.5,
                    size: 14 + Math.random() * 8,
                    direction: Math.random() > 0.5 ? 1 : -1,
                    tailPhase: Math.random() * Math.PI
                });
            }

            // Generate Type 2 Small Fish (Teal/Pink Stripe Fish)
            smallFishType2 = [];
            for (let i = 0; i < 8; i++) {
                smallFishType2.push({
                    x: Math.random() * canvas.width,
                    y: 80 + Math.random() * (canvas.height - 200),
                    speed: 1.2 + Math.random() * 1.8,
                    size: 12 + Math.random() * 6,
                    direction: Math.random() > 0.5 ? 1 : -1,
                    tailPhase: Math.random() * Math.PI
                });
            }
        }

        function spawnSharkQuizzes() {
            sharkQuizzes = [];
            const marginX = 150;
            const marginY = 100;
            
            for (let i = 0; i < 10; i++) {
                sharkQuizzes.push({
                    id: i,
                    x: marginX + Math.random() * (canvas.width - marginX * 2),
                    y: marginY + Math.random() * (canvas.height - marginY * 2),
                    radius: 32,
                    vx: (Math.random() - 0.5) * 1.2,
                    vy: (Math.random() - 0.5) * 1.2,
                    active: true,
                    pulse: Math.random() * Math.PI
                });
            }
        }

        function setupEventListeners() {
            window.addEventListener('keydown', e => keys[e.code] = true);
            window.addEventListener('keyup', e => keys[e.code] = false);

            // Touch events for mobile movement
            window.addEventListener('pointerdown', handleTouch);
            window.addEventListener('pointermove', handleTouch);
            window.addEventListener('pointerup', () => touchTarget = null);
        }

        function handleTouch(e) {
            if (!gameRunning) return;
            // Ignore touches on UI overlay elements
            if (e.target.tagName !== 'CANVAS') return;
            touchTarget = { x: e.clientX, y: e.clientY };
        }

        function startGame() {
            AudioEngine.init();
            AudioEngine.playBubbleSound();

            const nameInput = document.getElementById('playerName').value.trim();
            playerName = nameInput.length > 0 ? nameInput : "Siswa Kelas 7";

            document.getElementById('hudPlayerName').innerText = playerName;
            document.getElementById('startScreen').classList.add('hidden');
            document.getElementById('gameHUD').classList.remove('hidden');
            document.getElementById('touchControlsHint').classList.remove('hidden');

            score = 0;
            quizzesAnswered = 0;
            updateHUD();

            player.x = canvas.width / 2;
            player.y = canvas.height / 2;

            spawnSharkQuizzes();
            gameRunning = true;
        }

        function restartGame() {
            document.getElementById('victoryModal').classList.add('hidden');
            startGame();
        }

        function updateHUD() {
            document.getElementById('hudScore').innerText = `${score} / 100`;
            document.getElementById('hudProgress').innerText = `${quizzesAnswered} dari 10`;
        }

        function drawEnvironment(time) {
            // Water Gradient
            const bgGrad = ctx.createLinearGradient(0, 0, 0, canvas.height);
            bgGrad.addColorStop(0, '#0284c7');  // Shallow clear blue
            bgGrad.addColorStop(0.5, '#0369a1'); // Mid ocean
            bgGrad.addColorStop(1, '#0f172a');   // Deep ocean floor
            ctx.fillStyle = bgGrad;
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Sun Rays effect from top
            ctx.save();
            ctx.fillStyle = 'rgba(255, 255, 255, 0.04)';
            for (let i = 0; i < 5; i++) {
                ctx.beginPath();
                ctx.moveTo((i * canvas.width / 4) + Math.sin(time * 0.001 + i) * 30, 0);
                ctx.lineTo((i * canvas.width / 4) + 100 + Math.sin(time * 0.001 + i) * 50, canvas.height);
                ctx.lineTo((i * canvas.width / 4) - 50, canvas.height);
                ctx.closePath();
                ctx.fill();
            }
            ctx.restore();

            // Animated Seaweed
            seaweedList.forEach(sw => {
                const swing = Math.sin(time * 0.002 + sw.phase) * 25;
                ctx.fillStyle = sw.color;
                ctx.beginPath();
                ctx.moveTo(sw.x - sw.width / 2, canvas.height);
                ctx.quadraticCurveTo(sw.x + swing, canvas.height - sw.height / 2, sw.x + swing * 1.5, canvas.height - sw.height);
                ctx.quadraticCurveTo(sw.x + swing, canvas.height - sw.height / 2, sw.x + sw.width / 2, canvas.height);
                ctx.closePath();
                ctx.fill();
            });

            // Floating Bubbles
            ctx.fillStyle = 'rgba(224, 242, 254, 0.5)';
            bubblesList.forEach(b => {
                b.y -= b.speed;
                b.x += Math.sin(time * 0.003 + b.y * 0.05) * 0.5;
                if (b.y < -10) {
                    b.y = canvas.height + 10;
                    b.x = Math.random() * canvas.width;
                }
                ctx.beginPath();
                ctx.arc(b.x, b.y, b.radius, 0, Math.PI * 2);
                ctx.fill();
            });
        }

        function drawSmallFishes(time) {
            // Type 1 Small Fish (Yellow/Pink Tropical Fish)
            smallFishType1.forEach(f => {
                f.x += f.speed * f.direction;
                if (f.x > canvas.width + 40) f.x = -40;
                if (f.x < -40) f.x = canvas.width + 40;

                ctx.save();
                ctx.translate(f.x, f.y);
                if (f.direction < 0) ctx.scale(-1, 1);

                // Tail
                const tailWiggle = Math.sin(time * 0.01 + f.tailPhase) * 5;
                ctx.fillStyle = '#f43f5e';
                ctx.beginPath();
                ctx.moveTo(-f.size, 0);
                ctx.lineTo(-f.size - 10, -6 + tailWiggle);
                ctx.lineTo(-f.size - 10, 6 + tailWiggle);
                ctx.closePath();
                ctx.fill();

                // Body
                ctx.fillStyle = '#facc15';
                ctx.beginPath();
                ctx.ellipse(0, 0, f.size, f.size * 0.6, 0, 0, Math.PI * 2);
                ctx.fill();

                // Eye
                ctx.fillStyle = '#000';
                ctx.beginPath();
                ctx.arc(f.size * 0.4, -2, 2, 0, Math.PI * 2);
                ctx.fill();

                ctx.restore();
            });

            // Type 2 Small Fish (Cyan/Pink Striped Fish)
            smallFishType2.forEach(f => {
                f.x += f.speed * f.direction;
                if (f.x > canvas.width + 40) f.x = -40;
                if (f.x < -40) f.x = canvas.width + 40;

                ctx.save();
                ctx.translate(f.x, f.y);
                if (f.direction < 0) ctx.scale(-1, 1);

                // Body
                ctx.fillStyle = '#06b6d4';
                ctx.beginPath();
                ctx.ellipse(0, 0, f.size, f.size * 0.7, 0, 0, Math.PI * 2);
                ctx.fill();

                // Stripe
                ctx.fillStyle = '#ec4899';
                ctx.fillRect(-2, -f.size * 0.6, 4, f.size * 1.2);

                // Eye
                ctx.fillStyle = '#fff';
                ctx.beginPath();
                ctx.arc(f.size * 0.4, -2, 3, 0, Math.PI * 2);
                ctx.fill();
                ctx.fillStyle = '#000';
                ctx.beginPath();
                ctx.arc(f.size * 0.4 + 1, -2, 1.5, 0, Math.PI * 2);
                ctx.fill();

                ctx.restore();
            });
        }

        function drawSharkQuizzes(time) {
            sharkQuizzes.forEach((shark, idx) => {
                if (!shark.active) return;

                // Gentle floating movement
                shark.x += shark.vx;
                shark.y += shark.vy;

                // Bounce off walls
                if (shark.x < 50 || shark.x > canvas.width - 50) shark.vx *= -1;
                if (shark.y < 50 || shark.y > canvas.height - 50) shark.vy *= -1;

                ctx.save();
                ctx.translate(shark.x, shark.y);

                // Pulse Glowing Aura
                const glow = Math.sin(time * 0.005 + shark.pulse) * 8 + 12;
                ctx.shadowColor = '#38bdf8';
                ctx.shadowBlur = glow;

                const dir = shark.vx >= 0 ? 1 : -1;
                if (dir < 0) ctx.scale(-1, 1);

                // Cute Shark Body (Grey/Blue)
                ctx.fillStyle = '#64748b';
                ctx.beginPath();
                ctx.ellipse(0, 0, 36, 22, 0, 0, Math.PI * 2);
                ctx.fill();

                // White Belly
                ctx.fillStyle = '#f1f5f9';
                ctx.beginPath();
                ctx.ellipse(4, 8, 24, 10, 0, 0, Math.PI * 2);
                ctx.fill();

                // Shark Fin (Dorsal)
                ctx.fillStyle = '#475569';
                ctx.beginPath();
                ctx.moveTo(-5, -20);
                ctx.quadraticCurveTo(5, -32, 15, -18);
                ctx.closePath();
                ctx.fill();

                // Tail Fin
                ctx.beginPath();
                ctx.moveTo(-32, 0);
                ctx.lineTo(-46, -16);
                ctx.lineTo(-40, 0);
                ctx.lineTo(-46, 16);
                ctx.closePath();
                ctx.fill();

                // Cute Big Eyes
                ctx.fillStyle = '#fff';
                ctx.beginPath();
                ctx.arc(16, -6, 6, 0, Math.PI * 2);
                ctx.fill();
                ctx.fillStyle = '#0f172a';
                ctx.beginPath();
                ctx.arc(18, -6, 3, 0, Math.PI * 2);
                ctx.fill();

                // Cute Smile & Cheeks
                ctx.strokeStyle = '#334155';
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.arc(20, 4, 5, 0, Math.PI * 0.8);
                ctx.stroke();

                ctx.fillStyle = '#f43f5e';
                ctx.beginPath();
                ctx.arc(10, 4, 3, 0, Math.PI * 2);
                ctx.fill();

                ctx.restore();

                // Question Badge / Number Indicator
                ctx.save();
                ctx.shadowBlur = 0;
                ctx.fillStyle = '#f59e0b';
                ctx.beginPath();
                ctx.arc(shark.x, shark.y - 32, 12, 0, Math.PI * 2);
                ctx.fill();
                ctx.fillStyle = '#000';
                ctx.font = 'bold 11px Fredoka, sans-serif';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText(`Q${idx + 1}`, shark.x, shark.y - 32);
                ctx.restore();
            });
        }

        function updateAndDrawPlayer(time) {
            // Velocity handling
            let dx = 0;
            let dy = 0;

            if (keys['ArrowLeft'] || keys['KeyA']) dx -= 1;
            if (keys['ArrowRight'] || keys['KeyD']) dx += 1;
            if (keys['ArrowUp'] || keys['KeyW']) dy -= 1;
            if (keys['ArrowDown'] || keys['KeyS']) dy += 1;

            if (touchTarget) {
                const tx = touchTarget.x - player.x;
                const ty = touchTarget.y - player.y;
                const dist = Math.hypot(tx, ty);
                if (dist > 10) {
                    dx = tx / dist;
                    dy = ty / dist;
                }
            }

            // Normalize vector
            if (dx !== 0 && dy !== 0 && !touchTarget) {
                dx *= 0.7071;
                dy *= 0.7071;
            }

            player.x += dx * player.speed;
            player.y += dy * player.speed;

            // Boundaries
            player.x = Math.max(player.radius, Math.min(canvas.width - player.radius, player.x));
            player.y = Math.max(player.radius, Math.min(canvas.height - player.radius, player.y));

            if (dx !== 0 || dy !== 0) {
                player.angle = Math.atan2(dy, dx);
            }

            // Draw Cute Dolphin Player
            ctx.save();
            ctx.translate(player.x, player.y);
            ctx.rotate(player.angle);

            const tailWiggle = Math.sin(time * 0.015) * 8;

            // Dolphin Tail & Fluke
            ctx.fillStyle = '#0284c7';
            ctx.beginPath();
            ctx.moveTo(-10, 0);
            ctx.quadraticCurveTo(-22, tailWiggle * 0.5, -28, tailWiggle);
            ctx.lineTo(-38, -12 + tailWiggle);
            ctx.quadraticCurveTo(-32, tailWiggle, -38, 12 + tailWiggle);
            ctx.lineTo(-28, tailWiggle);
            ctx.closePath();
            ctx.fill();

            // Dorsal Fin
            ctx.fillStyle = '#0284c7';
            ctx.beginPath();
            ctx.moveTo(-5, -12);
            ctx.quadraticCurveTo(-2, -26, 12, -10);
            ctx.closePath();
            ctx.fill();

            // Main Body (Cute cyan dolphin shape)
            ctx.fillStyle = '#38bdf8';
            ctx.beginPath();
            ctx.ellipse(2, 0, 26, 16, 0, 0, Math.PI * 2);
            ctx.fill();

            // Lighter Belly
            ctx.fillStyle = '#f0f9ff';
            ctx.beginPath();
            ctx.ellipse(4, 5, 18, 9, 0.1, 0, Math.PI * 2);
            ctx.fill();

            // Dolphin Snout
            ctx.fillStyle = '#38bdf8';
            ctx.beginPath();
            ctx.moveTo(20, -4);
            ctx.quadraticCurveTo(34, 0, 32, 6);
            ctx.quadraticCurveTo(20, 8, 18, 2);
            ctx.closePath();
            ctx.fill();

            // Pectoral Fin
            ctx.fillStyle = '#0284c7';
            ctx.beginPath();
            ctx.moveTo(4, 4);
            ctx.quadraticCurveTo(-2, 18, -12, 12);
            ctx.quadraticCurveTo(-4, 6, 4, 4);
            ctx.fill();

            // Cute Eye
            ctx.fillStyle = '#ffffff';
            ctx.beginPath();
            ctx.arc(14, -4, 5, 0, Math.PI * 2);
            ctx.fill();

            ctx.fillStyle = '#0f172a';
            ctx.beginPath();
            ctx.arc(15, -4, 2.5, 0, Math.PI * 2);
            ctx.fill();

            // Eye sparkle
            ctx.fillStyle = '#ffffff';
            ctx.beginPath();
            ctx.arc(16, -5, 1, 0, Math.PI * 2);
            ctx.fill();

            // Cute Rosy Cheek
            ctx.fillStyle = 'rgba(244, 63, 94, 0.4)';
            ctx.beginPath();
            ctx.ellipse(10, 3, 3.5, 2.5, 0, 0, Math.PI * 2);
            ctx.fill();

            // Cute Smile
            ctx.strokeStyle = '#0284c7';
            ctx.lineWidth = 1.5;
            ctx.beginPath();
            ctx.arc(22, 3, 4, 0.1, Math.PI * 0.6);
            ctx.stroke();

            ctx.restore();
        }

        function checkCollisions() {
            if (!gameRunning) return;

            sharkQuizzes.forEach((shark) => {
                if (!shark.active) return;

                const dist = Math.hypot(player.x - shark.x, player.y - shark.y);
                if (dist < player.radius + shark.radius) {
                    // Trigger Quiz!
                    AudioEngine.playBubbleSound();
                    openQuizModal(shark.id);
                }
            });
        }

        function openQuizModal(quizIndex) {
            gameRunning = false;
            activeQuizIndex = quizIndex;
            const quiz = pancasilaQuizzes[quizIndex];

            document.getElementById('quizNumberBadge').innerText = `Soal #${quizIndex + 1}`;
            document.getElementById('questionText').innerText = quiz.q;

            const optionsContainer = document.getElementById('optionsContainer');
            optionsContainer.innerHTML = '';

            quiz.options.forEach((opt, idx) => {
                const btn = document.createElement('button');
                btn.className = 'p-4 bg-blue-900/80 hover:bg-cyan-600/50 border-2 border-cyan-400/40 rounded-2xl text-left font-semibold text-sm md:text-base text-cyan-100 transition flex items-center gap-3';
                btn.onclick = () => selectOption(idx);
                btn.innerHTML = `
                    <span class="w-8 h-8 rounded-full bg-cyan-500/20 text-cyan-300 font-fredoka flex items-center justify-center border border-cyan-400/30 flex-shrink-0">
                        ${String.fromCharCode(65 + idx)}
                    </span>
                    <span>${opt}</span>
                `;
                optionsContainer.appendChild(btn);
            });

            document.getElementById('feedbackBox').className = 'hidden p-3 rounded-xl mb-4 text-center font-bold text-sm';
            document.getElementById('nextBtn').classList.add('hidden');
            document.getElementById('quizModal').classList.remove('hidden');
        }

        function selectOption(selectedIndex) {
            const quiz = pancasilaQuizzes[activeQuizIndex];
            const optionsBtns = document.getElementById('optionsContainer').children;
            const feedbackBox = document.getElementById('feedbackBox');

            // Disable all options
            for (let btn of optionsBtns) {
                btn.onclick = null;
                btn.classList.add('opacity-60', 'cursor-not-allowed');
            }

            if (selectedIndex === quiz.answer) {
                // Correct
                AudioEngine.playCorrectSound();
                score += 10;
                optionsBtns[selectedIndex].classList.remove('opacity-60', 'bg-blue-900/80', 'border-cyan-400/40');
                optionsBtns[selectedIndex].classList.add('bg-emerald-600/80', 'border-emerald-400', 'text-white');
                
                feedbackBox.innerHTML = `✨ <b>JAWABAN BENAR!</b> (+10 Poin)<br><span class="text-xs text-amber-300 font-bold mt-1 block">🏆 Total Akumulasi Poin: ${score} / 100 Poin</span>`;
                feedbackBox.className = 'p-3 rounded-xl mb-4 text-center font-bold text-sm bg-emerald-500/20 text-emerald-300 border border-emerald-400/40 block animate-bounce';
            } else {
                // Wrong
                AudioEngine.playWrongSound();
                optionsBtns[selectedIndex].classList.remove('opacity-60', 'bg-blue-900/80', 'border-cyan-400/40');
                optionsBtns[selectedIndex].classList.add('bg-rose-600/80', 'border-rose-400', 'text-white');

                // Highlight correct
                optionsBtns[quiz.answer].classList.remove('opacity-60');
                optionsBtns[quiz.answer].classList.add('bg-emerald-600/80', 'border-emerald-400', 'text-white');

                feedbackBox.innerHTML = `❌ <b>Jawaban Kurang Tepat.</b><br><span class="text-xs">Jawaban yang benar: (${String.fromCharCode(65 + quiz.answer)}) ${quiz.options[quiz.answer]}</span><br><span class="text-xs text-cyan-200 mt-1 block">🏆 Total Akumulasi Poin Saat Ini: ${score} / 100 Poin</span>`;
                feedbackBox.className = 'p-3 rounded-xl mb-4 text-center font-bold text-sm bg-rose-500/20 text-rose-300 border border-rose-400/40 block';
            }

            // Deactivate shark
            sharkQuizzes[activeQuizIndex].active = false;
            quizzesAnswered++;
            updateHUD();

            document.getElementById('nextBtn').classList.remove('hidden');
        }

        function closeQuizModal() {
            document.getElementById('quizModal').classList.add('hidden');

            if (quizzesAnswered >= 10) {
                showVictoryModal();
            } else {
                gameRunning = true;
            }
        }

        function showVictoryModal() {
            gameRunning = false;
            document.getElementById('gameHUD').classList.add('hidden');
            document.getElementById('touchControlsHint').classList.add('hidden');

            document.getElementById('finalScoreDisplay').innerText = score;
            document.getElementById('victoryPlayerName').value = playerName;
            
            const predicate = document.getElementById('gradePredicate');
            if (score === 100) {
                predicate.innerText = '🌟 Sempurna! Ksatria Pancasila Utama';
            } else if (score >= 70) {
                predicate.innerText = '👍 Hebat! Memahami Nilai-Nilai Pancasila';
            } else {
                predicate.innerText = '📚 Cukup Baik! Mari Belajar Pancasila Lebih Giat Lagi';
            }

            document.getElementById('victoryModal').classList.remove('hidden');
        }

        function openCertificateModal() {
            const inputName = document.getElementById('victoryPlayerName').value.trim();
            if (inputName) {
                playerName = inputName;
                document.getElementById('hudPlayerName').innerText = playerName;
            }

            document.getElementById('certStudentName').innerText = playerName || "Siswa Kelas 7";
            document.getElementById('certScore').innerText = `${score} / 100`;

            const today = new Date();
            const options = { year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('certDate').innerText = today.toLocaleDateString('id-ID', options);

            document.getElementById('certificateModal').classList.remove('hidden');
        }

        function closeCertificateModal() {
            document.getElementById('certificateModal').classList.add('hidden');
        }

        function gameLoop(time) {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            drawEnvironment(time);
            drawSmallFishes(time);
            drawSharkQuizzes(time);

            if (gameRunning) {
                updateAndDrawPlayer(time);
                checkCollisions();
            }

            requestAnimationFrame(gameLoop);
        }

        window.onload = function() {
            initCanvas();
            setupEventListeners();
            requestAnimationFrame(gameLoop);
        };
    </script>
</body>
</html>
