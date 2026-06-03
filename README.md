<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>古典文法 助動詞マスター</title>
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Google Fonts: Plus Jakarta Sans & Noto Sans JP -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700&family=Plus+Jakarta+Sans:wght@500;700&family=Shippori+Mincho:wght@600;800&display=swap" rel="stylesheet">
  <!-- Tabler Icons -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@latest/tabler-icons.min.css">
  <!-- Canvas Confetti (正解・満点時の演出用) -->
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['Noto Sans JP', 'Plus Jakarta Sans', 'sans-serif'],
            serif: ['Shippori Mincho', 'serif'],
          }
        }
      }
    }
  </script>

  <style>
    /* CSS変数による一貫したカラースキーム定義 */
    :root {
      --color-primary: #8b5cf6;       /* ヴァイオレット */
      --color-primary-glow: rgba(139, 92, 246, 0.15);
      --color-success: #10b981;       /* エメラルド */
      --color-danger: #f43f5e;        /* ローズ */
      --color-info: #0ea5e9;          /* スカイブルー */
    }

    body {
      background: radial-gradient(circle at 50% 0%, #1e1b4b 0%, #0f172a 100%);
      min-height: 100vh;
    }

    /* カスタムアニメーション */
    @keyframes bounce-subtle {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-4px); }
    }
    .animate-bounce-subtle {
      animation: bounce-subtle 3s ease-in-out infinite;
    }

    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      25% { transform: translateX(-6px); }
      75% { transform: translateX(6px); }
    }
    .animate-shake {
      animation: shake 0.2s ease-in-out 2;
    }

    @keyframes pop {
      0% { transform: scale(0.95); opacity: 0.8; }
      100% { transform: scale(1); opacity: 1; }
    }
    .animate-pop {
      animation: pop 0.2s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }

    /* JSから動的に制御する既存クラスのモダン・スタイリング */
    .screen { display: none; }
    .screen.active { display: block; animation: pop 0.35s cubic-bezier(0.16, 1, 0.3, 1) forwards; }

    /* 選択肢ボタンの基本・正解・不正解ステート */
    .choice-btn {
      width: 100%;
      padding: 1rem;
      font-size: 0.95rem;
      font-weight: 500;
      color: #cbd5e1;
      background: rgba(30, 41, 59, 0.6);
      border: 1px solid rgba(71, 85, 105, 0.4);
      border-radius: 1rem;
      cursor: pointer;
      transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
      text-align: center;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    }
    .choice-btn:hover:not(:disabled) {
      background: rgba(30, 41, 59, 0.9);
      border-color: rgba(139, 92, 246, 0.5);
      color: #ffffff;
      transform: translateY(-2px);
      box-shadow: 0 10px 15px -3px var(--color-primary-glow), 0 4px 6px -4px var(--color-primary-glow);
    }
    .choice-btn.selected {
      background: rgba(14, 165, 233, 0.15);
      border-color: var(--color-info) !important;
      color: #38bdf8;
    }
    .choice-btn.correct {
      background: rgba(16, 185, 129, 0.15) !important;
      border-color: var(--color-success) !important;
      color: #34d399 !important;
      box-shadow: 0 0 15px rgba(16, 185, 129, 0.2);
    }
    .choice-btn.wrong {
      background: rgba(244, 63, 94, 0.1) !important;
      border-color: var(--color-danger) !important;
      color: #f43f5e !important;
      animation: shake 0.25s ease-in-out;
    }

    /* 複数選択チェックボックスの基本・選択・正解・不正解ステート */
    .check-label {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 0.5rem;
      padding: 0.75rem 1rem;
      border: 1px solid rgba(71, 85, 105, 0.4);
      border-radius: 0.75rem;
      cursor: pointer;
      font-size: 0.9rem;
      font-weight: 500;
      color: #94a3b8;
      background: rgba(30, 41, 59, 0.5);
      transition: all 0.2s ease;
      user-select: none;
    }
    .check-label:hover {
      background: rgba(30, 41, 59, 0.8);
      color: #f1f5f9;
      border-color: rgba(139, 92, 246, 0.4);
    }
    .check-label.checked {
      background: rgba(139, 92, 246, 0.15);
      border-color: var(--color-primary);
      color: #c084fc;
    }
    .check-label.correct {
      background: rgba(16, 185, 129, 0.15) !important;
      border-color: var(--color-success) !important;
      color: #34d399 !important;
    }
    .check-label.wrong {
      background: rgba(244, 63, 94, 0.15) !important;
      border-color: var(--color-danger) !important;
      color: #f87171 !important;
      animation: shake 0.25s ease-in-out;
    }
    .check-label input { display: none; }

    /* フィードバックメッセージ */
    .feedback-msg {
      padding: 1rem 1.25rem;
      border-radius: 1rem;
      font-size: 0.95rem;
      font-weight: 500;
      margin-bottom: 1.25rem;
      display: none;
      border: 1px solid transparent;
      animation: pop 0.25s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }
    .feedback-msg.show { display: flex; align-items: center; gap: 0.5rem; }
    .feedback-msg.ok {
      background: rgba(16, 185, 129, 0.12);
      border-color: rgba(16, 185, 129, 0.3);
      color: #34d399;
    }
    .feedback-msg.ng {
      background: rgba(244, 63, 94, 0.12);
      border-color: rgba(244, 63, 94, 0.3);
      color: #f43f5e;
    }

    /* 正解確認時に、選択しなかったが「正解」だった選択肢に施すハイライト */
    .check-label.ans {
      background: rgba(16, 185, 129, 0.08) !important;
      border: 2px dashed var(--color-success) !important;
      color: #34d399 !important;
    }

    /* ピル（バッジ） */
    .pill {
      font-size: 0.8rem;
      padding: 0.25rem 0.75rem;
      border-radius: 99px;
      font-weight: 500;
    }
    .pill.my { background: rgba(244, 63, 94, 0.15); color: #fb7185; border: 1px solid rgba(244, 63, 94, 0.3); }
    .pill.ans { background: rgba(16, 185, 129, 0.15); color: #34d399; border: 1px solid rgba(16, 185, 129, 0.3); }

    .pill-item {
      display: inline-block;
      font-size: 0.75rem;
      padding: 0.2rem 0.6rem;
      border-radius: 99px;
      margin: 2px;
      font-weight: 500;
    }
    .pill-item.my { background: rgba(244, 63, 94, 0.15); color: #fb7185; border: 1px solid rgba(244, 63, 94, 0.3); }
    .pill-item.ans { background: rgba(16, 185, 129, 0.15); color: #34d399; border: 1px solid rgba(16, 185, 129, 0.3); }
  </style>
</head>
<body class="flex flex-col justify-between py-6 px-4 md:py-12 select-none">

  <!-- メインコンテンツラッパー（美しいガラス質感のカード） -->
  <div class="w-full max-w-xl mx-auto my-auto bg-slate-900/60 border border-slate-800/80 backdrop-blur-xl rounded-3xl shadow-2xl p-6 md:p-8">
    <div id="app">

      <!-- ==================== HOME SCREEN ==================== -->
      <div id="screen-home" class="screen active">
        <div class="text-center mb-8 md:mb-10">
          <div class="inline-flex items-center justify-center w-16 h-16 rounded-2xl bg-gradient-to-tr from-violet-600 to-indigo-500 text-white mb-4 shadow-lg shadow-indigo-500/20 animate-bounce-subtle">
            <i class="ti ti-book-2 text-3xl"></i>
          </div>
          <h1 class="text-3xl font-bold bg-gradient-to-r from-violet-400 via-indigo-200 to-cyan-300 bg-clip-text text-transparent font-sans tracking-wide">
            古典文法 助動詞マスター
          </h1>
          <p class="text-slate-400 text-sm mt-3 font-medium">
            助動詞の接続と意味をスタイリッシュに完全攻略
          </p>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 max-w-md mx-auto">
          <!-- モード①接続 -->
          <div class="group relative overflow-hidden bg-slate-800/40 border border-slate-700/50 rounded-2xl p-5 cursor-pointer hover:border-violet-500/80 transition-all duration-300 hover:scale-[1.02] hover:shadow-xl hover:shadow-violet-500/5" onclick="startMode('setsuzoku')">
            <div class="absolute top-0 right-0 w-24 h-24 bg-violet-600/10 rounded-full blur-xl group-hover:bg-violet-600/20 transition-all"></div>
            <div class="flex items-center justify-center w-12 h-12 rounded-xl bg-violet-500/10 text-violet-400 mb-4 group-hover:bg-violet-500 group-hover:text-white transition-all duration-300">
              <i class="ti ti-link text-2xl"></i>
            </div>
            <h2 class="text-lg font-bold text-slate-100 group-hover:text-white transition-all">① 接続</h2>
            <p class="text-xs text-slate-400 mt-2 leading-relaxed">
              助動詞がどの活用形に接続するかを答えます。
            </p>
          </div>

          <!-- モード②意味 -->
          <div class="group relative overflow-hidden bg-slate-800/40 border border-slate-700/50 rounded-2xl p-5 cursor-pointer hover:border-cyan-500/80 transition-all duration-300 hover:scale-[1.02] hover:shadow-xl hover:shadow-cyan-500/5" onclick="startMode('imi')">
            <div class="absolute top-0 right-0 w-24 h-24 bg-cyan-600/10 rounded-full blur-xl group-hover:bg-cyan-600/20 transition-all"></div>
            <div class="flex items-center justify-center w-12 h-12 rounded-xl bg-cyan-500/10 text-cyan-400 mb-4 group-hover:bg-cyan-500 group-hover:text-white transition-all duration-300">
              <i class="ti ti-align-left text-2xl"></i>
            </div>
            <h2 class="text-lg font-bold text-slate-100 group-hover:text-white transition-all">② 意味</h2>
            <p class="text-xs text-slate-400 mt-2 leading-relaxed">
              助動詞が持つすべての意味を正しく選択します。
            </p>
          </div>
        </div>
      </div>

      <!-- ==================== QUIZ SCREEN ==================== -->
      <div id="screen-quiz" class="screen">
        <!-- メタ情報 -->
        <div class="flex justify-between items-center mb-4">
          <span class="text-xs font-bold text-slate-400 bg-slate-800/80 px-3 py-1.5 rounded-full border border-slate-700/50" id="q-num">1 / 10</span>
          <span class="text-xs font-bold px-3 py-1.5 rounded-full border border-violet-500/30 bg-violet-950/40 text-violet-300" id="mode-label"></span>
        </div>

        <!-- プレミアムプログレスバー -->
        <div class="w-full bg-slate-850 rounded-full h-2 mb-6 overflow-hidden border border-slate-800">
          <div class="h-full bg-gradient-to-r from-violet-500 via-purple-500 to-cyan-400 rounded-full transition-all duration-300 shadow-[0_0_12px_rgba(139,92,246,0.5)]" id="prog-bar" style="width:0%"></div>
        </div>

        <!-- 問題カード -->
        <div class="relative overflow-hidden bg-gradient-to-b from-slate-800/40 to-slate-900/40 border border-slate-800 rounded-2xl p-6 mb-6 text-center shadow-inner">
          <div class="absolute inset-0 bg-grid-pattern opacity-5"></div>
          <div class="text-xs text-slate-400 uppercase tracking-wider font-semibold mb-2" id="q-label"></div>
          <!-- 助動詞を美しい明朝体（Serif）で表示 -->
          <div class="text-4xl md:text-5xl font-extrabold text-white py-4 font-serif tracking-widest bg-gradient-to-r from-slate-100 to-indigo-100 bg-clip-text text-transparent" id="q-text"></div>
        </div>

        <!-- 選択肢エリア -->
        <div id="choices-area" class="mb-5"></div>

        <!-- フィードバック（アニメーション対応） -->
        <div class="feedback-msg" id="feedback"></div>

        <!-- 次へ進むボタン -->
        <button class="w-full py-3.5 px-4 bg-gradient-to-r from-violet-600 to-indigo-600 hover:from-violet-500 hover:to-indigo-500 disabled:from-slate-800 disabled:to-slate-800 disabled:text-slate-600 text-white font-bold rounded-xl transition-all duration-200 shadow-lg shadow-indigo-500/20 disabled:shadow-none cursor-pointer focus:ring-2 focus:ring-violet-500 focus:ring-offset-2 focus:ring-offset-slate-900" id="next-btn" onclick="nextQuestion()" disabled>
          次の問題へ
        </button>
      </div>

      <!-- ==================== RESULT SCREEN ==================== -->
      <div id="screen-result" class="screen">
        <div class="text-center mb-8">
          <div class="relative inline-flex items-center justify-center mb-4">
            <!-- 円形チャート風の装飾を兼ねた外輪 -->
            <div class="w-32 h-32 rounded-full border-4 border-slate-800 flex items-center justify-center bg-slate-950/50 shadow-2xl">
              <div class="text-center">
                <div class="text-4xl font-bold text-white tracking-tight" id="score-display"></div>
                <div class="text-[10px] font-bold text-slate-500 tracking-wider uppercase mt-1">Score</div>
              </div>
            </div>
            <!-- 装飾用ネオンリング -->
            <div class="absolute inset-0 rounded-full border border-violet-500/30 blur-sm scale-105"></div>
          </div>
          <div class="text-xl font-bold bg-gradient-to-r from-violet-200 to-cyan-200 bg-clip-text text-transparent" id="score-sub"></div>
        </div>

        <!-- 間違えた問題リスト -->
        <div id="mistakes-section" class="max-h-[320px] overflow-y-auto pr-1 mb-6 custom-scrollbar"></div>

        <!-- ホームに戻るボタン -->
        <button class="w-full py-3 px-4 bg-slate-800 hover:bg-slate-700/80 border border-slate-700 text-slate-300 font-bold rounded-xl transition-all duration-200 flex items-center justify-center gap-2" onclick="goHome()">
          <i class="ti ti-home text-lg"></i>モード選択へ戻る
        </button>
      </div>

    </div>
  </div>

  <!-- フッター -->
  <footer class="w-full text-center mt-8 text-[11px] text-slate-500/60 font-mono">
    Classical Grammar App © 2026 • Designed for Excellence
  </footer>

  <!-- ==================== JAVASCRIPT ==================== -->
  <script>
    const DATA = {
      setsuzoku: [
        { word: "る・らる", answer: "未然" },
        { word: "す・さす", answer: "未然" },
        { word: "しむ", answer: "未然" },
        { word: "ず", answer: "未然" },
        { word: "む・むず", answer: "未然" },
        { word: "まし", answer: "未然" },
        { word: "まほし", answer: "未然" },
        { word: "じ", answer: "未然" },
        { word: "き", answer: "連用" },
        { word: "けり", answer: "連用" },
        { word: "つ", answer: "連用" },
        { word: "ぬ", answer: "連用" },
        { word: "たり（完了）", answer: "連用" },
        { word: "けむ", answer: "連用" },
        { word: "たし", answer: "連用" },
        { word: "めり", answer: "終止" },
        { word: "なり（伝聞）", answer: "終止" },
        { word: "らむ", answer: "終止" },
        { word: "らし", answer: "終止" },
        { word: "まじ", answer: "終止" },
        { word: "べし", answer: "終止" },
        { word: "なり（断定）", answer: "特殊" , note: "体言や連体形に接続します"},
        { word: "たり（断定）", answer: "特殊" , note: "体言に接続します"},
        { word: "ごとし", answer: "特殊" , note: "体言・連体形、助動詞「が」「の」に接続します"},
        { word: "り", answer: "特殊" , note: "サ変の未然形・四段の已然形（サ未四已）に接続します"},
      ],
      imi: [
        { word: "る・らる", answer: ["受身", "尊敬", "自発", "可能"] },
        { word: "す・さす", answer: ["使役", "尊敬"] },
        { word: "しむ", answer: ["使役", "尊敬"] },
        { word: "ず", answer: ["打消"] },
        { word: "き", answer: ["過去"] },
        { word: "けり", answer: ["過去", "詠嘆"] },
        { word: "つ", answer: ["完了", "強意"] },
        { word: "ぬ", answer: ["完了", "強意"] },
        { word: "たり（完了）", answer: ["完了", "存続"] },
        { word: "り", answer: ["完了", "存続"] },
        { word: "む", answer: ["推量", "意志", "勧誘", "仮定", "婉曲"] },
        { word: "むず", answer: ["推量", "意志"] },
        { word: "けむ", answer: ["過去推量", "過去の原因推量", "伝聞", "婉曲"] },
        { word: "らむ", answer: ["現在推量", "現在の原因推量", "伝聞", "婉曲"] },
        { word: "らし", answer: ["推量", "根拠ある推量"] },
        { word: "めり", answer: ["推量", "婉曲"] },
        { word: "べし", answer: ["推量", "意志", "当然", "命令", "可能", "適当"] },
        { word: "まじ", answer: ["打消推量", "打消意志", "打消当然", "禁止", "打消可能", "不適当"] },
        { word: "まし", answer: ["反実仮想", "ためらいの意志", "推量"] },
        { word: "じ", answer: ["打消推量", "打消意志"] },
        { word: "なり（断定）", answer: ["断定", "存在"] },
        { word: "なり（伝聞）", answer: ["伝聞", "推定"] },
        { word: "たり（断定）", answer: ["断定"] },
        { word: "ごとし", answer: ["比況", "例示"] },
        { word: "まほし", answer: ["願望"] },
        { word: "たし", answer: ["願望"] },
      ]
    };

    const SETSUZOKU_CHOICES = ["未然", "連用", "終止", "連体", "已然", "命令", "特殊"];
    const IMI_ALL = ["受身","尊敬","自発","可能","使役","打消","過去","詠嘆","完了","存続","強意","推量","意志","勧誘","仮定","婉曲","過去推量","過去の原因推量","現在推量","現在の原因推量","伝聞","根拠ある推量","当然","命令","適当","打消推量","打消意志","打消当然","禁止","打消可能","不適当","反実仮想","ためらいの意志","断定","存在","推定","比況","例示","願望"];

    let mode = '', questions = [], current = 0, score = 0, mistakes = [];
    let answered = false;

    function shuffle(arr) { return [...arr].sort(() => Math.random() - 0.5); }

    function startMode(m) {
      mode = m;
      const pool = shuffle(DATA[m]);
      questions = pool.slice(0, 10);
      current = 0; score = 0; mistakes = [];
      answered = false;
      show('screen-quiz');
      document.getElementById('mode-label').textContent = m === 'setsuzoku' ? '① 接続' : '② 意味';
      renderQuestion();
    }

    function show(id) {
      document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    function renderQuestion() {
      answered = false;
      const q = questions[current];
      document.getElementById('q-num').textContent = `${current + 1} / 10`;
      document.getElementById('prog-bar').style.width = `${(current / 10) * 100}%`;
      document.getElementById('feedback').className = 'feedback-msg';
      document.getElementById('next-btn').disabled = true;
      document.getElementById('next-btn').textContent = current < 9 ? '次の問題へ' : '結果を見る';

      if (mode === 'setsuzoku') {
        document.getElementById('q-label').textContent = '次の助動詞の接続を選んでください';
        document.getElementById('q-text').textContent = q.word;
        const area = document.getElementById('choices-area');
        area.innerHTML = '';
        const grid = document.createElement('div');
        grid.className = 'grid grid-cols-2 gap-3 sm:grid-cols-4'; // レスポンシブで洗練されたグリッド
        SETSUZOKU_CHOICES.forEach(c => {
          const btn = document.createElement('button');
          btn.className = 'choice-btn';
          btn.textContent = c;
          btn.onclick = () => selectChoice(btn, c, q.answer);
          grid.appendChild(btn);
        });
        area.appendChild(grid);
      } else {
        document.getElementById('q-label').textContent = '次の助動詞の意味をすべて選んでください';
        document.getElementById('q-text').textContent = q.word;
        const area = document.getElementById('choices-area');
        area.innerHTML = '';
        const grid = document.createElement('div');
        grid.className = 'grid grid-cols-2 gap-2 sm:grid-cols-3 max-h-[220px] overflow-y-auto pr-1'; // はみ出しを防ぐスクロール対応
        const opts = shuffle([...new Set([...IMI_ALL, ...q.answer])]);
        opts.forEach(opt => {
          const label = document.createElement('label');
          label.className = 'check-label';
          label.dataset.val = opt;
          const inp = document.createElement('input');
          inp.type = 'checkbox'; inp.value = opt;
          inp.addEventListener('change', () => {
            label.classList.toggle('checked', inp.checked);
            const anyChecked = grid.querySelectorAll('input:checked').length > 0;
            document.getElementById('next-btn').disabled = !anyChecked;
          });
          label.appendChild(inp);
          label.appendChild(document.createTextNode(opt));
          grid.appendChild(label);
        });
        area.appendChild(grid);

        document.getElementById('next-btn').textContent = current < 9 ? '答え合わせ → 次へ' : '答え合わせ → 結果へ';
        document.getElementById('next-btn').disabled = true;
        document.getElementById('next-btn').onclick = () => checkImi();
      }
    }

    function selectChoice(btn, chosen, correct) {
      if (answered) return;
      answered = true;
      const q = questions[current];
      document.querySelectorAll('.choice-btn').forEach(b => {
        b.disabled = true;
        if (b.textContent === correct) b.classList.add('correct');
      });
      const fb = document.getElementById('feedback');

      if (chosen === correct) {
        score++;
        btn.classList.add('correct');
        // 正解の心地よいミニ紙吹雪
        triggerSparkle();
        if (correct === '特殊' && q.note) {
          fb.innerHTML = `<i class="ti ti-circle-check text-xl"></i><span>正解！特殊（${q.note}）です。</span>`;
        } else {
          fb.innerHTML = `<i class="ti ti-circle-check text-xl"></i><span>正解です！</span>`;
        }
        fb.className = 'feedback-msg show ok animate-pop';
      } else {
        btn.classList.add('wrong');
        if (correct === '特殊' && q.note) {
          fb.innerHTML = `<i class="ti ti-circle-x text-xl"></i><span>不正解。正解は「特殊」です（${q.note}）。</span>`;
        } else {
          fb.innerHTML = `<i class="ti ti-circle-x text-xl"></i><span>不正解。正解は「${correct}」です。</span>`;
        }
        fb.className = 'feedback-msg show ng animate-shake';
        mistakes.push({ word: q.word, myAnswer: chosen, correct: correct });
      }
      
      document.getElementById('next-btn').disabled = false;
    }

    function checkImi() {
      if (answered) { nextQuestion(); return; }
      answered = true;
      const q = questions[current];
      const grid = document.getElementById('choices-area').querySelector('.grid');
      const selected = [...grid.querySelectorAll('input:checked')].map(i => i.value);
      const correct = q.answer;
      const isCorrect = selected.length === correct.length && correct.every(c => selected.includes(c));

      grid.querySelectorAll('.check-label').forEach(label => {
        const val = label.dataset.val;
        const inp = label.querySelector('input');
        inp.disabled = true;
        const inSelected = selected.includes(val);
        const inCorrect = correct.includes(val);
        if (inCorrect && inSelected) { label.classList.remove('checked'); label.classList.add('correct'); }
        else if (inCorrect && !inSelected) { label.classList.add('ans'); label.style.border = '1.5px solid'; label.classList.add('correct'); }
        else if (!inCorrect && inSelected) { label.classList.remove('checked'); label.classList.add('wrong'); }
      });

      const fb = document.getElementById('feedback');
      if (isCorrect) {
        score++;
        triggerSparkle();
        fb.innerHTML = `<i class="ti ti-circle-check text-xl"></i><span>正解！すべての意味を正しく選べました。</span>`;
        fb.className = 'feedback-msg show ok animate-pop';
      } else {
        fb.innerHTML = `<i class="ti ti-circle-x text-xl"></i><span>不正解。正解：${correct.join('・')}</span>`;
        fb.className = 'feedback-msg show ng animate-shake';
        mistakes.push({ word: q.word, myAnswer: selected, correct: correct });
      }
      document.getElementById('next-btn').textContent = current < 9 ? '次の問題へ' : '結果を見る';
      document.getElementById('next-btn').onclick = nextQuestion;
      document.getElementById('next-btn').disabled = false;
    }

    function nextQuestion() {
      current++;
      if (current >= 10) { showResult(); return; }
      answered = false;
      if (mode === 'setsuzoku') {
        document.getElementById('next-btn').onclick = nextQuestion;
      }
      renderQuestion();
    }

    function showResult() {
      document.getElementById('prog-bar').style.width = '100%';
      show('screen-result');
      document.getElementById('score-display').textContent = `${score} / 10`;
      
      const msgs = ['もう少し復習しよう！', '半分正解！ここから伸ばそう', 'なかなか良いセンス！', 'ほぼ完璧！素晴らしい', '満点！完全制覇達成！'];
      const idx = score <= 3 ? 0 : score <= 5 ? 1 : score <= 7 ? 2 : score <= 9 ? 3 : 4;
      document.getElementById('score-sub').textContent = msgs[idx];

      // スコアに応じたConfetti演出
      if (score === 10) {
        triggerGrandConfetti();
      } else if (score >= 7) {
        triggerNiceConfetti();
      }

      const sec = document.getElementById('mistakes-section');
      if (mistakes.length === 0) {
        sec.innerHTML = '<div class="all-correct py-8 text-center text-emerald-400 font-bold flex flex-col items-center gap-2"><i class="ti ti-award text-5xl animate-bounce-subtle"></i><span>全問正解！助動詞マスターです！</span></div>';
        return;
      }
      let html = `<p class="text-xs font-bold text-slate-400 mb-3 flex items-center gap-1.5"><i class="ti ti-alert-triangle"></i>弱点復習 (${mistakes.length}問)</p><div class="flex flex-col gap-3">`;
      mistakes.forEach(m => {
        html += `<div class="bg-slate-800/40 border border-slate-800 rounded-xl p-4">
                  <div class="text-md font-bold text-slate-100 font-serif mb-2 border-b border-slate-800 pb-1.5">${m.word}</div>`;
        if (mode === 'setsuzoku') {
          html += `<div class="flex items-center gap-2 text-xs mb-1">
                     <span class="text-slate-500 w-20">あなたの回答:</span>
                     <span class="pill my">${m.myAnswer}</span>
                   </div>`;
          html += `<div class="flex items-center gap-2 text-xs">
                     <span class="text-slate-500 w-20">正しい答え:</span>
                     <span class="pill ans">${m.correct}</span>
                   </div>`;
        } else {
          html += `<div class="flex items-start gap-2 text-xs mb-2">
                     <span class="text-slate-500 w-20 mt-1 flex-shrink-0">あなたの回答:</span>
                     <div class="flex flex-wrap gap-1">`;
          if (m.myAnswer.length === 0) {
            html += `<span class="pill-item text-slate-600 border border-slate-800">（未選択）</span>`;
          } else {
            const correctSet = new Set(m.correct);
            m.myAnswer.forEach(a => {
              html += `<span class="pill-item ${correctSet.has(a) ? 'ans' : 'my'}">${a}</span>`;
            });
          }
          html += `   </div>
                   </div>`;
          html += `<div class="flex items-start gap-2 text-xs">
                     <span class="text-slate-500 w-20 mt-1 flex-shrink-0">正しい答え:</span>
                     <div class="flex flex-wrap gap-1">`;
          m.correct.forEach(a => {
            html += `<span class="pill-item ans">${a}</span>`;
          });
          html += `   </div>
                   </div>`;
        }
        html += `</div>`;
      });
      html += '</div>';
      sec.innerHTML = html;
    }

    function goHome() {
      show('screen-home');
    }

    /* ---- 気持ち良い学習のためのビジュアル演出(Confetti) ---- */
    // 1問正解時の小さな火花
    function triggerSparkle() {
      confetti({
        particleCount: 15,
        spread: 40,
        origin: { y: 0.8 },
        colors: ['#a78bfa', '#34d399', '#38bdf8']
      });
    }

    // 7〜9点獲得時の紙吹雪
    function triggerNiceConfetti() {
      const duration = 1.5 * 1000;
      const end = Date.now() + duration;

      (function frame() {
        confetti({
          particleCount: 3,
          angle: 60,
          spread: 55,
          origin: { x: 0 },
          colors: ['#a78bfa', '#818cf8', '#34d399']
        });
        confetti({
          particleCount: 3,
          angle: 120,
          spread: 55,
          origin: { x: 1 },
          colors: ['#a78bfa', '#818cf8', '#34d399']
        });

        if (Date.now() < end) {
          requestAnimationFrame(frame);
        }
      }());
    }

    // 10点満点時の豪華なシャワー
    function triggerGrandConfetti() {
      var count = 200;
      var defaults = {
        origin: { y: 0.7 }
      };

      function fire(particleRatio, opts) {
        confetti(Object.assign({}, defaults, opts, {
          particleCount: Math.floor(count * particleRatio)
        }));
      }

      fire(0.25, {
        spread: 26,
        startVelocity: 55,
      });
      fire(0.2, {
        spread: 60,
      });
      fire(0.35, {
        spread: 100,
        decay: 0.91,
        scalar: 0.8
      });
      fire(0.1, {
        spread: 120,
        startVelocity: 25,
        decay: 0.92,
        scalar: 1.2
      });
      fire(0.1, {
        spread: 120,
        startVelocity: 45,
      });
    }
  </script>
</body>
</html>
