# <!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- SEO & OGP -->
    <title>「休日、何もしてない」罪悪感測定【本格診断】</title>
    <meta name="description" content="全12問であなたの「虚無の重さ」を精密に測定。休日の罪悪感と向き合う5分間のメンタルチェック。">
    <meta property="og:title" content="「休日、何もしてない」罪悪感測定【本格診断】">
    <meta property="og:description" content="全12問であなたの「虚無の重さ」を精密に測定。休日の罪悪感と向き合う5分間のメンタルチェック。">
    <meta property="og:type" content="website">
    <meta name="twitter:card" content="summary_large_image">

    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700;900&display=swap');
        
        :root {
            --primary: #6366f1;
            --bg-gradient: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
        }

        body {
            font-family: 'Noto Sans JP', sans-serif;
            background: var(--bg-gradient);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #1e293b;
        }

        .app-container {
            background: rgba(255, 255, 255, 0.98);
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.15);
            border-radius: 2.5rem;
            width: 100%;
            max-width: 32rem;
            overflow: hidden;
            position: relative;
        }

        .fade-in { animation: fadeIn 0.5s ease-out forwards; }
        .slide-up { animation: slideUp 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards; }

        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        @keyframes slideUp { 
            from { opacity: 0; transform: translateY(20px); } 
            to { opacity: 1; transform: translateY(0); } 
        }

        .btn-hover {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .btn-hover:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(99, 102, 241, 0.3);
        }

        .progress-fill {
            transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
        }

        /* カスタムスクロールバー */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
    </style>
</head>
<body class="p-4 sm:p-6">

    <div class="app-container" id="app-root">
        
        <!-- ヘッダー装飾 -->
        <div class="h-2 w-full bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500"></div>

        <div class="p-8 sm:p-12">
            <!-- 1. START SCREEN -->
            <div id="view-start" class="text-center slide-up">
                <div class="inline-block p-4 bg-indigo-50 rounded-full mb-6">
                    <span class="text-5xl">🛌</span>
                </div>
                <h1 class="text-3xl font-black text-gray-900 mb-4 tracking-tight">
                    休日、何もしてない<br><span class="text-indigo-600">罪悪感測定</span>
                </h1>
                <p class="text-gray-500 mb-10 leading-relaxed font-medium">
                    「気づいたら外が暗い」「YouTubeを見てたら1日終わった」<br>
                    心の奥に潜む「虚無」を、全12問で精密に測定。
                </p>
                
                <div class="space-y-4">
                    <button onclick="router.go('quiz')" class="btn-hover w-full bg-indigo-600 text-white py-4 rounded-2xl font-bold text-lg shadow-lg">
                        測定を開始する
                    </button>
                    <p id="resume-msg" class="text-xs text-indigo-400 font-bold hidden cursor-pointer hover:underline" onclick="quizManager.resume()">
                        前回の続きから再開する
                    </p>
                </div>
                
                <p class="mt-8 text-xs text-gray-400 uppercase tracking-widest font-bold">Estimated time: 5 mins</p>
            </div>

            <!-- 2. QUIZ SCREEN -->
            <div id="view-quiz" class="hidden">
                <div class="mb-10">
                    <div class="flex justify-between items-end mb-4">
                        <div>
                            <span id="q-category" class="text-xs font-black text-indigo-500 uppercase tracking-[0.2em] mb-1 block">Category</span>
                            <h3 class="text-gray-400 text-sm font-bold">Progress</h3>
                        </div>
                        <span class="text-2xl font-black text-gray-800" id="q-count-display">01<span class="text-gray-300 text-sm font-bold ml-1">/ 12</span></span>
                    </div>
                    <div class="w-full bg-gray-100 h-3 rounded-full overflow-hidden p-0.5">
                        <div id="progress-bar" class="progress-fill bg-indigo-500 h-full rounded-full w-0 shadow-[0_0_10px_rgba(99,102,241,0.5)]"></div>
                    </div>
                </div>

                <div class="min-h-[100px] mb-10">
                    <h2 class="text-2xl font-bold text-gray-800 leading-snug tracking-tight" id="q-text">Loading question...</h2>
                </div>

                <div class="grid gap-3" id="q-options">
                    <!-- Options injected here -->
                </div>
            </div>

            <!-- 3. RESULT SCREEN -->
            <div id="view-result" class="hidden text-center slide-up">
                <div id="res-emoji" class="text-7xl mb-6 transform hover:scale-110 transition-transform cursor-default"></div>
                <h2 class="text-gray-400 text-xs font-black uppercase tracking-[0.3em] mb-2">Analysis Result</h2>
                <div id="res-rank" class="text-3xl font-black text-gray-900 mb-6"></div>
                
                <div class="bg-gray-50 rounded-[2rem] p-8 mb-8 border border-gray-100 relative overflow-hidden">
                    <div class="absolute top-0 right-0 p-4 opacity-5">
                        <svg class="w-24 h-24" fill="currentColor" viewBox="0 0 24 24"><path d="M13 14.725c0-5.141 3.892-10.519 10-11.725l.984 2.126c-2.215.835-4.163 3.742-4.38 5.746 2.491.392 4.396 2.547 4.396 5.149 0 3.182-2.584 4.979-5.199 4.979-3.015 0-5.801-2.305-5.801-6.275zm-13 0c0-5.141 3.892-10.519 10-11.725l.984 2.126c-2.215.835-4.163 3.742-4.38 5.746 2.491.392 4.396 2.547 4.396 5.149 0 3.182-2.584 4.979-5.199 4.979-3.015 0-5.801-2.305-5.801-6.275z"/></svg>
                    </div>
                    <div class="relative z-10">
                        <div class="text-xs text-indigo-400 font-black mb-1 uppercase">Score</div>
                        <div id="res-score" class="text-6xl font-black text-rose-500 mb-6 tabular-nums"></div>
                        <p id="res-desc" class="text-gray-600 leading-relaxed font-medium text-left"></p>
                    </div>
                </div>
                
                <div class="grid gap-4">
                    <a href="#" id="share-link" target="_blank" class="btn-hover bg-black text-white py-4 px-6 rounded-2xl font-bold flex items-center justify-center gap-3">
                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
                        結果をポストして供養する
                    </a>
                    <button onclick="quizManager.reset()" class="text-gray-400 text-sm font-bold hover:text-indigo-600 transition-colors uppercase tracking-widest">
                        Restart Measurement
                    </button>
                </div>
            </div>

        </div>
    </div>

    <script>
        /**
         * 1. QUESTION DATA
         */
        const QUESTIONS = [
            { cat: "Phase: Morning", q: "目が覚めてから、最初にしたことは？", ops: [
                { t: "布団の中でスマホを30分以上いじった", s: 10 },
                { t: "とりあえず一度起きて顔を洗った", s: 0 },
                { t: "二度寝して昼まで記憶を飛ばした", s: 15 },
                { t: "カーテンを開けて朝日を浴びた", s: -10 }
            ]},
            { cat: "Phase: Morning", q: "「今日の予定」を考えた時の正直な気持ちは？", ops: [
                { t: "やるべきことが山積みで気が重い", s: 15 },
                { t: "特に何もないことに安心した", s: 0 },
                { t: "やりたいことが多すぎてワクワクしている", s: -10 },
                { t: "予定を考えるのをやめて、スマホを手に取った", s: 10 }
            ]},
            { cat: "Phase: Morning", q: "朝食（あるいは昼食）の内容はどうだった？", ops: [
                { t: "バランスを考えて自炊した", s: -10 },
                { t: "菓子パンやスナックで済ませた", s: 10 },
                { t: "食べていない（空腹より面倒が勝った）", s: 15 },
                { t: "Uber Eats 等で少し贅沢をした", s: 0 }
            ]},
            { cat: "Phase: Afternoon", q: "午後の時間の使い方のメインは？", ops: [
                { t: "SNS、動画の無限スクロール", s: 20 },
                { t: "溜まっていた家事や片付け", s: -5 },
                { t: "趣味やスキルの勉強など", s: -15 },
                { t: "目的なくテレビや動画を流し見", s: 10 }
            ]},
            { cat: "Phase: Afternoon", q: "友人からの「何してる？」へのあなたの反応は？", ops: [
                { t: "「忙しい」と嘘をついて断った", s: 15 },
                { t: "返信せず未読のまま時間を稼いだ", s: 10 },
                { t: "正直に「死ぬほどダラダラしてる」と返した", s: 0 },
                { t: "即座に「暇！遊ぼう！」と食いついた", s: -10 }
            ]},
            { cat: "Phase: Afternoon", q: "ふと鏡に映った自分の姿、どう見えた？", ops: [
                { t: "「ひどい顔してるな…」と絶望した", s: 15 },
                { t: "特に何も思わなかった（無）", s: 5 },
                { t: "「休日の自分、リラックスしてて最高」", s: -5 },
                { t: "一度も鏡を見ていない（見たくない）", s: 10 }
            ]},
            { cat: "Phase: Evening", q: "外が暗くなってきた時の心の変化は？", ops: [
                { t: "暗い部屋でスマホの光だけを見ていた", s: 15 },
                { t: "「もう1日が終わる…」と泣きたくなった", s: 20 },
                { t: "「よし、夜の時間を楽しもう」と切り替えた", s: -5 },
                { t: "特に何も感じず、淡々と過ごした", s: 5 }
            ]},
            { cat: "Phase: Evening", q: "今日一日、声を出して誰かと会話した？", ops: [
                { t: "一言も話していない（独り言はカウント外）", s: 15 },
                { t: "店員さんと最低限のやり取りのみ", s: 5 },
                { t: "家族や友人としっかり話した", s: -10 },
                { t: "通話やボイチャをたっぷりした", s: -5 }
            ]},
            { cat: "Phase: Evening", q: "「明日の準備」を考えた時、どう思う？", ops: [
                { t: "考えるだけで胃がキリキリする", s: 15 },
                { t: "まだ考えないように現実逃避している", s: 10 },
                { t: "しっかり休んだので頑張れそうだ", s: -10 },
                { t: "いっそこのまま時間が止まってほしい", s: 20 }
            ]},
            { cat: "Phase: Mindset", q: "あなたにとって「充実した休日」とは？", ops: [
                { t: "外に出てアクティブに動くこと", s: 10 },
                { t: "誰にも邪魔されず完全に休むこと", s: -5 },
                { t: "何らかの成果（勉強や家事）を出すこと", s: 15 },
                { t: "何も考えずに過ぎ去ること", s: 0 }
            ]},
            { cat: "Phase: Mindset", q: "今、この瞬間の本音を聞かせて。", ops: [
                { t: "このまま一生寝ていたい", s: 15 },
                { t: "どこか遠くへ、誰も知らない所へ行きたい", s: 10 },
                { t: "規則正しい、キラキラした生活に戻りたい", s: 5 },
                { t: "今のままで、十分に満足だ", s: -10 }
            ]},
            { cat: "Phase: Mindset", q: "最後に。この診断を受けている今の気持ちは？", ops: [
                { t: "図星すぎて心が痛い", s: 20 },
                { t: "ただの暇つぶしだ", s: 0 },
                { t: "自分の状況を客観視できてスッキリした", s: -10 },
                { t: "早く終わらせて、また布団に戻りたい", s: 5 }
            ]}
        ];

        /**
         * 2. ROUTING & STATE MANAGEMENT
         */
        const router = {
            go(viewId) {
                ['start', 'quiz', 'result'].forEach(id => {
                    document.getElementById(`view-${id}`).classList.add('hidden');
                });
                document.getElementById(`view-${viewId}`).classList.remove('hidden');
                window.scrollTo(0, 0);
            }
        };

        const quizManager = {
            currentIndex: 0,
            totalScore: 0,

            init() {
                const saved = localStorage.getItem('guilt_test_state');
                if (saved) {
                    const data = JSON.parse(saved);
                    if (data.currentIndex > 0 && data.currentIndex < QUESTIONS.length) {
                        document.getElementById('resume-msg').classList.remove('hidden');
                    }
                }
            },

            resume() {
                const saved = JSON.parse(localStorage.getItem('guilt_test_state'));
                this.currentIndex = saved.currentIndex;
                this.totalScore = saved.totalScore;
                router.go('quiz');
                this.renderQuestion();
            },

            start() {
                this.currentIndex = 0;
                this.totalScore = 0;
                this.saveState();
                this.renderQuestion();
            },

            saveState() {
                localStorage.setItem('guilt_test_state', JSON.stringify({
                    currentIndex: this.currentIndex,
                    totalScore: this.totalScore
                }));
            },

            renderQuestion() {
                const q = QUESTIONS[this.currentIndex];
                const qText = document.getElementById('q-text');
                const optDiv = document.getElementById('q-options');
                
                // Update UI
                document.getElementById('q-category').innerText = q.cat;
                document.getElementById('q-count-display').innerHTML = 
                    `${(this.currentIndex + 1).toString().padStart(2, '0')}<span class="text-gray-300 text-sm font-bold ml-1">/ ${QUESTIONS.length}</span>`;
                document.getElementById('progress-bar').style.width = `${((this.currentIndex) / QUESTIONS.length) * 100}%`;
                
                qText.classList.remove('fade-in');
                void qText.offsetWidth;
                qText.classList.add('fade-in');
                qText.innerText = q.q;

                optDiv.innerHTML = '';
                q.ops.forEach((o, i) => {
                    const btn = document.createElement('button');
                    btn.className = "group w-full text-left p-4 sm:p-5 rounded-2xl border-2 border-gray-100 font-medium text-gray-700 hover:border-indigo-500 hover:bg-indigo-50 transition-all slide-up flex justify-between items-center";
                    btn.style.animationDelay = `${i * 0.05}s`;
                    btn.innerHTML = `<span>${o.t}</span><span class="opacity-0 group-hover:opacity-100 transition-opacity text-indigo-400">→</span>`;
                    btn.onclick = () => this.handleAnswer(o.s);
                    optDiv.appendChild(btn);
                });
            },

            handleAnswer(score) {
                this.totalScore += score;
                this.currentIndex++;
                this.saveState();

                if (this.currentIndex < QUESTIONS.length) {
                    this.renderQuestion();
                } else {
                    this.showResult();
                }
            },

            showResult() {
                localStorage.removeItem('guilt_test_state');
                router.go('result');

                let rank, desc, emoji, theme;
                const s = this.totalScore;

                if(s <= 0) {
                    rank = "解脱の達人"; emoji = "🧘"; theme = "#10b981";
                    desc = "あなたは「何もしない」ことのプロフェッショナル。罪悪感を超越し、真の意味での休息を享受できています。現代社会においてこのマインドは希少な才能です。";
                } else if(s <= 45) {
                    rank = "健全な休息者"; emoji = "🍀"; theme = "#6366f1";
                    desc = "休日のダラダラを「必要なコスト」として割り切れています。多少の焦燥感はあっても、それは社会との繋がりがある証拠。月曜日にはまた歩き出せるタイプです。";
                } else if(s <= 95) {
                    rank = "慢性的な虚無症候群"; emoji = "🌪️"; theme = "#f59e0b";
                    desc = "理想の休日像に縛られ、動けない自分を責めていませんか？「何かしたい」のに「体が重い」。その葛藤こそが疲れの正体。まずはハードルを極限まで下げましょう。";
                } else if(s <= 145) {
                    rank = "罪悪感の奴隷"; emoji = "⛓️"; theme = "#ef4444";
                    desc = "心が悲鳴を上げています。休日が終わる時、自分を責めるエネルギーでさらに疲弊するという悪循環。あなたは十分頑張っています。まずは自分を許す練習を。";
                } else {
                    rank = "虚無の特異点"; emoji = "🕳️"; theme = "#1e293b";
                    desc = "測定不能なレベルの深い虚無に到達。自分を責めることすら忘れるほどの完全なる空白。今のあなたに必要なのは、診断ではなく、深い眠りと温かいスープです。";
                }

                document.getElementById('res-rank').innerText = rank;
                document.getElementById('res-emoji').innerText = emoji;
                document.getElementById('res-score').innerText = s + "pts";
                document.getElementById('res-desc').innerText = desc;
                document.getElementById('res-score').style.color = theme;

                const shareText = encodeURIComponent(`「休日、何もしてない」罪悪感測定の結果は…\n\nランク：【${rank}】\n罪悪感スコア：${s}pts\n${desc}\n\n#休日何もしてない診断 #虚無診断`);
                document.getElementById('share-link').href = `https://twitter.com/intent/tweet?text=${shareText}`;
            },

            reset() {
                localStorage.removeItem('guilt_test_state');
                location.reload();
            }
        };

        // Initialize on load
        window.onload = () => {
            quizManager.init();
        };

        // Override start if needed
        const originalGo = router.go;
        router.go = function(id) {
            if (id === 'quiz' && quizManager.currentIndex === 0) {
                quizManager.start();
            }
            originalGo.apply(this, arguments);
        };
    </script>
</body>
</html>

