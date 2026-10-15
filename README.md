<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HIKAKIN Special Fan Web App - バーチャルヒカキンワールド</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Tone.js for Sound Effects -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.49/Tone.js"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Custom Styles -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=M+PLUS+Rounded+1c:wght@400;700;800;900&display=swap');
        
        body {
            font-family: 'M PLUS Rounded 1c', sans-serif;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 50%, #f12711 100%);
            min-height: 100vh;
        }

        .hikakin-gold {
            background: linear-gradient(135deg, #FFE000 0%, #799F0C 100%);
        }

        .youtube-red {
            background-color: #FF0000;
        }

        .glass-card {
            background: rgba(255, 255, 255, 0.92);
            backdrop-filter: blur(10px);
            border: 3px solid rgba(255, 215, 0, 0.6);
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3), 0 10px 10px -5px rgba(0, 0, 0, 0.1);
        }

        .pulse-btn:hover {
            transform: scale(1.05);
            transition: all 0.2s ease-in-out;
        }

        /* Confetti Animation Canvas */
        #confetti-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 999;
        }

        /* Custom Bounce Effects */
        @keyframes pop {
            0% { transform: scale(0.8); }
            50% { transform: scale(1.15); }
            100% { transform: scale(1); }
        }

        .animate-pop {
            animation: pop 0.3s ease-out forwards;
        }
    </style>
</head>
<body class="text-gray-800 pb-12">
    <!-- Confetti Canvas -->
    <canvas id="confetti-canvas"></canvas>

    <!-- Header Banner -->
    <header class="bg-red-600 text-white shadow-2xl border-b-4 border-yellow-400 sticky top-0 z-50">
        <div class="max-w-5xl mx-auto px-4 py-3 flex flex-wrap justify-between items-center">
            <div class="flex items-center space-x-3">
                <i class="fa-brands fa-youtube text-4xl text-white animate-bounce"></i>
                <h1 class="text-2xl md:text-3xl font-black tracking-wider text-yellow-300 drop-shadow-md">
                    ブンブン！HIKAKIN Web App
                </h1>
            </div>
            <div class="mt-2 sm:mt-0 bg-black/40 px-4 py-1.5 rounded-full border border-yellow-400/50 flex items-center space-x-2">
                <span class="w-3 h-3 bg-red-500 rounded-full animate-ping"></span>
                <span class="text-xs sm:text-sm font-bold text-yellow-200">HikakinTV チャンネル登録者数風</span>
            </div>
        </div>
    </header>

    <main class="max-w-5xl mx-auto px-4 mt-8 space-y-8">

        <!-- 1. Live Subscriber Counter Component -->
        <section class="glass-card rounded-3xl p-6 md:p-8 text-center relative overflow-hidden">
            <div class="absolute -right-10 -bottom-10 opacity-10 text-red-600 pointer-events-none">
                <i class="fa-brands fa-youtube text-[200px]"></i>
            </div>
            <h2 class="text-xl md:text-2xl font-extrabold text-gray-700 mb-2">
                🎉 HikakinTV リアルタイムカウンター（風）
            </h2>
            <div class="text-4xl sm:text-6xl md:text-7xl font-black text-red-600 my-4 tracking-wider font-mono bg-black/5 py-4 rounded-2xl shadow-inner border border-gray-200" id="sub-counter">
                18,800,000人
            </div>
            <div class="flex flex-wrap justify-center gap-4 mt-4">
                <button onclick="boostSubs()" class="youtube-red hover:bg-red-700 text-white font-black py-3 px-6 rounded-full shadow-lg pulse-btn flex items-center space-x-2 text-lg border-2 border-yellow-300">
                    <i class="fa-solid fa-bell"></i>
                    <span>チャンネル登録＆高評価ボタンを押す！</span>
                </button>
                <button onclick="resetSubs()" class="bg-gray-600 hover:bg-gray-700 text-white font-bold py-3 px-4 rounded-full text-sm">
                    リセット
                </button>
            </div>
        </div>

        <!-- 2. Hikakin Voice Soundboard / Beatbox Pads -->
        <section class="glass-card rounded-3xl p-6 md:p-8">
            <div class="flex items-center space-x-3 mb-6">
                <div class="bg-yellow-400 text-black p-3 rounded-2xl text-2xl shadow">
                    <i class="fa-solid fa-microphone-lines"></i>
                </div>
                <div>
                    <h2 class="text-2xl font-extrabold text-gray-800">ボイパ＆名台詞サウンドボード</h2>
                    <p class="text-xs text-gray-500">ボタンを押してヒカキンボイス・ボイパ音声を鳴らそう！</p>
                </div>
            </div>

            <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4">
                <button onclick="playSound('bunbun')" class="bg-gradient-to-br from-yellow-300 to-yellow-500 hover:from-yellow-400 hover:to-yellow-600 text-gray-900 font-extrabold p-4 rounded-2xl shadow-md border-2 border-yellow-200 pulse-btn text-center flex flex-col items-center justify-center space-y-1">
                    <span class="text-2xl">🐝</span>
                    <span>「ブンブン！」</span>
                    <span class="text-[10px] opacity-75">Kick Drum</span>
                </button>
                <button onclick="playSound('hello')" class="bg-gradient-to-br from-red-400 to-red-600 hover:from-red-500 hover:to-red-700 text-white font-extrabold p-4 rounded-2xl shadow-md border-2 border-red-300 pulse-btn text-center flex flex-col items-center justify-center space-y-1">
                    <span class="text-2xl">👋</span>
                    <span>「Hi Everyone!」</span>
                    <span class="text-[10px] opacity-75">Hi-Hat</span>
                </button>
                <button onclick="playSound('seikatsu')" class="bg-gradient-to-br from-orange-400 to-orange-600 hover:from-orange-500 hover:to-orange-700 text-white font-extrabold p-4 rounded-2xl shadow-md border-2 border-orange-300 pulse-btn text-center flex flex-col items-center justify-center space-y-1">
                    <span class="text-2xl">✨</span>
                    <span>「バーン！」</span>
                    <span class="text-[10px] opacity-75">Snare</span>
                </button>
                <button onclick="playSound('seikatsukawaru')" class="bg-gradient-to-br from-purple-500 to-indigo-600 hover:from-purple-600 hover:to-indigo-700 text-white font-extrabold p-4 rounded-2xl shadow-md border-2 border-purple-300 pulse-btn text-center flex flex-col items-center justify-center space-y-1">
                    <span class="text-2xl">🚀</span>
                    <span>「シュバーン！」</span>
                    <span class="text-[10px] opacity-75">Synthesizer</span>
                </button>
                <button onclick="playSound('ramen')" class="bg-gradient-to-br from-green-400 to-emerald-600 hover:from-green-500 hover:to-emerald-700 text-white font-extrabold p-4 rounded-2xl shadow-md border-2 border-green-300 pulse-btn text-center flex flex-col items-center justify-center space-y-1">
                    <span class="text-2xl">🍜</span>
                    <span>みそきんウマい！</span>
                    <span class="text-[10px] opacity-75">Bass Drop</span>
                </button>
                <button onclick="playSound('pupo')" class="bg-gradient-to-br from-pink-400 to-rose-500 hover:from-pink-500 hover:to-rose-600 text-white font-extrabold p-4 rounded-2xl shadow-md border-2 border-pink-300 pulse-btn text-center flex flex-col items-center justify-center space-y-1">
                    <span class="text-2xl">🤪</span>
                    <span>変顔ポーズ音</span>
                    <span class="text-[10px] opacity-75">Funny Sound</span>
                </button>
                <button onclick="playSound('fanfare')" class="bg-gradient-to-br from-amber-300 to-amber-500 hover:from-amber-400 hover:to-amber-600 text-black font-extrabold p-4 rounded-2xl shadow-md border-2 border-amber-200 pulse-btn text-center flex flex-col items-center justify-center space-y-1 col-span-2 sm:col-span-1">
                    <span class="text-2xl">🎺</span>
                    <span>ファンファーレ！</span>
                    <span class="text-[10px] opacity-75">Victory Trumpet</span>
                </button>
            </div>
        </section>

        <!-- 3. Hikakin Today's Quote / Message Generator -->
        <section class="glass-card rounded-3xl p-6 md:p-8">
            <div class="flex items-center space-x-3 mb-4">
                <div class="bg-red-500 text-white p-3 rounded-2xl text-2xl shadow">
                    <i class="fa-solid fa-comment-dots"></i>
                </div>
                <div>
                    <h2 class="text-2xl font-extrabold text-gray-800">本日のヒカキン名言＆アドバイス</h2>
                    <p class="text-xs text-gray-500">今日のあなたへ贈る夢と努力のヒカキンメッセージ</p>
                </div>
            </div>

            <div class="bg-amber-50 border-2 border-amber-200 rounded-2xl p-6 my-4 text-center min-h-[120px] flex items-center justify-center shadow-inner">
                <p id="quote-display" class="text-lg md:text-xl font-black text-amber-900 leading-relaxed">
                    「ボタンを押して今日のヒカキンからのメッセージを受け取ろう！」
                </p>
            </div>

            <div class="text-center">
                <button onclick="generateQuote()" class="bg-gradient-to-r from-yellow-400 to-amber-500 hover:from-yellow-500 hover:to-amber-600 text-black font-black py-3 px-8 rounded-full shadow-lg pulse-btn text-lg border-2 border-yellow-200">
                    🎲 メッセージを引く！
                </button>
            </div>
        </section>

        <!-- 4. Hikakin Quiz Game -->
        <section class="glass-card rounded-3xl p-6 md:p-8">
            <div class="flex items-center space-x-3 mb-6">
                <div class="bg-blue-500 text-white p-3 rounded-2xl text-2xl shadow">
                    <i class="fa-solid fa-circle-question"></i>
                </div>
                <div>
                    <h2 class="text-2xl font-extrabold text-gray-800">ヒカキン物知りクイズ！</h2>
                    <p class="text-xs text-gray-500">あなたはいくつ答えることができるかな？</p>
                </div>
            </div>

            <div id="quiz-container" class="bg-white rounded-2xl p-6 border-2 border-gray-100 shadow-sm">
                <div class="flex justify-between items-center mb-4 text-sm font-bold text-gray-500">
                    <span id="quiz-step">問題 1 / 3</span>
                    <span id="quiz-score" class="text-red-500 font-extrabold">スコア: 0点</span>
                </div>

                <h3 id="quiz-question" class="text-lg sm:text-xl font-bold text-gray-800 mb-6">
                    質問がここに表示されます
                </h3>

                <div id="quiz-options" class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                    <!-- Options will be rendered dynamically -->
                </div>

                <div id="quiz-result-message" class="mt-4 font-bold text-center text-lg hidden"></div>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="mt-12 text-center text-white/80 text-xs sm:text-sm">
        <p class="font-bold">© HIKAKIN Fan Appreciation Project | Created with Tone.js & Tailwind CSS</p>
        <p class="mt-1 opacity-70">※このWebアプリは課題制作のためのファン制作シミュレーションアプリです。</p>
    </footer>

    <!-- Logic Script -->
    <script>
        // --------------------------------------------------
        // Tone.js Sound Synthesizers for Soundboard
        // --------------------------------------------------
        let synth, noiseSynth, polySynth;

        function initAudio() {
            if (!synth) {
                synth = new Tone.MembraneSynth().toDestination();
                noiseSynth = new Tone.NoiseSynth({
                    noise: { type: 'white' },
                    envelope: { attack: 0.005, decay: 0.1, sustain: 0 }
                }).toDestination();
                polySynth = new Tone.PolySynth(Tone.Synth).toDestination();
            }
        }

        function playSound(type) {
            Tone.start();
            initAudio();

            const counter = document.getElementById('sub-counter');
            counter.classList.remove('animate-pop');
            void counter.offsetWidth; // Trigger reflow

            if (type === 'bunbun') {
                // Kick / BunBun
                synth.triggerAttackRelease('C1', '8n');
                counter.classList.add('animate-pop');
            } else if (type === 'hello') {
                // Hi-Hat
                noiseSynth.triggerAttackRelease('16n');
            } else if (type === 'seikatsu') {
                // Snare / Baan
                synth.triggerAttackRelease('G2', '16n');
                noiseSynth.triggerAttackRelease('8n');
            } else if (type === 'seikatsukawaru') {
                // Synth sweep
                polySynth.triggerAttackRelease(["C4", "E4", "G4", "B4"], "8n");
            } else if (type === 'ramen') {
                // Low bass
                synth.triggerAttackRelease('E0', '4n');
            } else if (type === 'pupo') {
                // Funny pitch bend
                const now = Tone.now();
                synth.triggerAttackRelease('F4', '16n', now);
                synth.triggerAttackRelease('C3', '16n', now + 0.1);
            } else if (type === 'fanfare') {
                // Fanfare chords
                const now = Tone.now();
                polySynth.triggerAttackRelease(["C4", "E4", "G4"], "8n", now);
                polySynth.triggerAttackRelease(["G4", "B4", "D5"], "8n", now + 0.15);
                polySynth.triggerAttackRelease(["C5", "E5", "G5"], "4n", now + 0.3);
                triggerConfetti();
            }
        }

        // --------------------------------------------------
        // 1. Live Subscriber Counter Logic
        // --------------------------------------------------
        let currentSubs = 18800000;

        function boostSubs() {
            currentSubs += Math.floor(Math.random() * 500) + 100;
            updateCounterDisplay();
            playSound('fanfare');
        }

        function resetSubs() {
            currentSubs = 18800000;
            updateCounterDisplay();
        }

        function updateCounterDisplay() {
            document.getElementById('sub-counter').innerText = currentSubs.toLocaleString() + "人";
        }

        // Auto increment subs occasionally for real-time feel
        setInterval(() => {
            currentSubs += Math.floor(Math.random() * 3) + 1;
            updateCounterDisplay();
        }, 3000);

        // --------------------------------------------------
        // 2. Quote Generator
        // --------------------------------------------------
        const quotes = [
            "「好きなことで、生きていく。でもそのためには好きなことの何倍も努力が必要なんだ！」",
            "「ブンブンハローYouTube！今日も一日全力で楽しんでいきましょう！」",
            "「夢を叶えるコツは、とにかく諦めずに毎日コツコツ続けること！」",
            "「失敗したっていい。それが次の面白い動画のネタになるからね！」",
            "「みそきん食べて、今日も元気に頑張りましょう！！」",
            "「自分の限界を決めるのは自分。いつでもチャレンジ精神を忘れるな！」"
        ];

        function generateQuote() {
            playSound('seikatsukawaru');
            const randomQuote = quotes[Math.floor(Math.random() * quotes.length)];
            const display = document.getElementById('quote-display');
            display.innerText = randomQuote;
            display.classList.remove('animate-pop');
            void display.offsetWidth;
            display.classList.add('animate-pop');
        }

        // --------------------------------------------------
        // 3. Quiz Game Engine
        // --------------------------------------------------
        const quizData = [
            {
                question: "ヒカキンがYouTube動画の冒頭で使うおなじみの挨拶はどれ？",
                options: ["ブンブンハローYouTube！", "パチパチハローYouTube！", "ウキウキハローYouTube！", "ヤッホーYouTube！"],
                answer: 0
            },
            {
                question: "ヒカキンがプロデュースして大ヒットしたカップ麺の名称は？",
                options: ["ヒカラーメン", "みそきん", "ブンブンメン", "ボイパの素"],
                answer: 1
            },
            {
                question: "ヒカキンの愛猫（2匹のネコ）の名前の組み合わせで正しいものは？",
                options: ["タマ ＆ トラ", "まる ＆ くろ", "まるお ＆ もふこ", "ポチ ＆ タマ"],
                answer: 2
            }
        ];

        let currentQuizIndex = 0;
        let score = 0;

        function loadQuiz() {
            const quiz = quizData[currentQuizIndex];
            document.getElementById('quiz-step').innerText = `問題 ${currentQuizIndex + 1} / ${quizData.length}`;
            document.getElementById('quiz-question').innerText = quiz.question;

            const optionsContainer = document.getElementById('quiz-options');
            optionsContainer.innerHTML = '';

            quiz.options.forEach((optionText, index) => {
                const btn = document.createElement('button');
                btn.className = "w-full text-left bg-gray-50 hover:bg-yellow-100 border-2 border-gray-200 hover:border-yellow-400 p-3.5 rounded-xl font-bold text-gray-700 transition duration-150 flex items-center justify-between";
                btn.innerHTML = `<span>${index + 1}. ${optionText}</span> <i class="fa-solid fa-chevron-right text-gray-400 text-xs"></i>`;
                btn.onclick = () => selectAnswer(index);
                optionsContainer.appendChild(btn);
            });

            document.getElementById('quiz-result-message').classList.add('hidden');
        }

        function selectAnswer(selectedIndex) {
            const quiz = quizData[currentQuizIndex];
            const resultMsg = document.getElementById('quiz-result-message');
            resultMsg.classList.remove('hidden');

            if (selectedIndex === quiz.answer) {
                score += 10;
                playSound('fanfare');
                resultMsg.className = "mt-4 font-black text-center text-lg text-green-600 animate-pop";
                resultMsg.innerText = "⭕️ 正解！素晴らしい！";
            } else {
                playSound('pupo');
                resultMsg.className = "mt-4 font-black text-center text-lg text-red-500 animate-pop";
                resultMsg.innerText = `❌ 残念！正解は「${quiz.options[quiz.answer]}」でした！`;
            }

            document.getElementById('quiz-score').innerText = `スコア: ${score}点`;

            setTimeout(() => {
                currentQuizIndex++;
                if (currentQuizIndex < quizData.length) {
                    loadQuiz();
                } else {
                    showFinalQuizResult();
                }
            }, 1800);
        }

        function showFinalQuizResult() {
            const container = document.getElementById('quiz-container');
            container.innerHTML = `
                <div class="text-center py-6">
                    <div class="text-5xl mb-3">🏆</div>
                    <h3 class="text-2xl font-black text-gray-800">クイズ終了！</h3>
                    <p class="text-xl font-bold text-red-600 my-2">最終スコア: ${score} / ${quizData.length * 10} 点</p>
                    <p class="text-sm text-gray-600 mb-6">${score === 30 ? '全問正解！あなたこそ真のヒカキンマニア！' : '惜しい！またチャレンジしてね！'}</p>
                    <button onclick="resetQuiz()" class="bg-red-600 hover:bg-red-700 text-white font-bold py-2.5 px-6 rounded-full text-sm">
                        もう一度挑戦する
                    </button>
                </div>
            `;
            triggerConfetti();
        }

        function resetQuiz() {
            currentQuizIndex = 0;
            score = 0;
            document.getElementById('quiz-container').innerHTML = `
                <div class="flex justify-between items-center mb-4 text-sm font-bold text-gray-500">
                    <span id="quiz-step"></span>
                    <span id="quiz-score" class="text-red-500 font-extrabold">スコア: 0点</span>
                </div>
                <h3 id="quiz-question" class="text-lg sm:text-xl font-bold text-gray-800 mb-6"></h3>
                <div id="quiz-options" class="grid grid-cols-1 sm:grid-cols-2 gap-3"></div>
                <div id="quiz-result-message" class="mt-4 font-bold text-center text-lg hidden"></div>
            `;
            loadQuiz();
        }

        // --------------------------------------------------
        // 4. Confetti Effect Animation
        // --------------------------------------------------
        function triggerConfetti() {
            const canvas = document.getElementById('confetti-canvas');
            const ctx = canvas.getContext('2d');
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;

            const particles = [];
            const colors = ['#FF0000', '#FFE000', '#00E5FF', '#FF007F', '#76FF03'];

            for (let i = 0; i < 80; i++) {
                particles.push({
                    x: canvas.width / 2,
                    y: canvas.height / 2,
                    vx: (Math.random() - 0.5) * 12,
                    vy: (Math.random() - 0.7) * 12,
                    color: colors[Math.floor(Math.random() * colors.length)],
                    size: Math.random() * 8 + 4,
                    life: 100
                });
            }

            function render() {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                let alive = false;

                particles.forEach(p => {
                    if (p.life > 0) {
                        alive = true;
                        p.x += p.vx;
                        p.y += p.vy;
                        p.vy += 0.2; // gravity
                        p.life--;
                        ctx.fillStyle = p.color;
                        ctx.fillRect(p.x, p.y, p.size, p.size);
                    }
                });

                if (alive) {
                    requestAnimationFrame(render);
                } else {
                    ctx.clearRect(0, 0, canvas.width, canvas.height);
                }
            }

            render();
        }

        // Initialize quiz on load
        window.onload = function() {
            loadQuiz();
        };
    </script>
</body>
</html>
