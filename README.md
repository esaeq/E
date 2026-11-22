<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>三線統帥：4V4 戰略紀元</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700;900&display=swap" rel="stylesheet">
  <style>
    * {
      box-sizing: border-box;
      user-select: none;
      -webkit-user-select: none;
      font-family: 'Noto Sans TC', sans-serif;
    }
    body, html {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      overflow: hidden;
      background-color: #0b0f19;
      color: #e2e8f0;
    }
    /* 自訂捲軸 */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: rgba(15, 23, 42, 0.6);
      border-radius: 4px;
    }
    ::-webkit-scrollbar-thumb {
      background: rgba(71, 85, 105, 0.8);
      border-radius: 4px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: rgba(100, 116, 139, 1);
    }
    /* 柔和背景網格，無任何高頻閃爍動畫 */
    .tactical-grid {
      background-image: 
        linear-gradient(to right, rgba(30, 41, 59, 0.45) 1px, transparent 1px),
        linear-gradient(to bottom, rgba(30, 41, 59, 0.45) 1px, transparent 1px);
      background-size: 40px 40px;
    }
    .panel-glass {
      background: rgba(15, 23, 42, 0.92);
      border: 1px solid rgba(51, 65, 85, 0.85);
      box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.6);
    }
    .btn-tactical {
      transition: transform 0.1s ease, background-color 0.15s ease, border-color 0.15s ease;
    }
    .btn-tactical:active {
      transform: scale(0.97);
    }
    canvas {
      display: block;
      width: 100%;
      height: 100%;
      touch-action: none;
    }
    input[type="range"] {
      accent-color: #38bdf8;
    }
  </style>
</head>
<body class="tactical-grid">

  <!-- ==================== 1. 主畫面 (MAIN MENU) ==================== -->
  <div id="screen-menu" class="w-full h-full flex flex-col items-center justify-center p-4 relative">
    <div class="max-w-3xl w-full panel-glass rounded-2xl p-6 sm:p-10 flex flex-col items-center text-center relative overflow-hidden">
      <div class="inline-block px-3 py-1 rounded-full bg-sky-500/15 border border-sky-500/40 text-sky-400 text-xs font-bold tracking-widest mb-3">
        THREE-LANE COMMANDER // 4V4 RTS
      </div>
      <h1 class="text-3xl sm:text-5xl font-black tracking-tight text-white mb-2">
        三線統帥：<span class="text-sky-400">4V4 戰略紀元</span>
      </h1>
      <p class="text-slate-400 text-sm sm:text-base max-w-xl mb-6">
        1 位指揮官 ＋ 3 位將軍的軍團協同。掌控三路小營升旗、分配信任點轉換人力與科研、研發兵種與戰術核彈，一舉摧毀敵方本營！
      </p>

      <!-- 指揮官代號設定 -->
      <div class="w-full max-w-md mb-6 flex flex-col sm:flex-row items-center gap-2 bg-slate-900/90 p-3 rounded-xl border border-slate-700">
        <span class="text-xs font-bold text-slate-400 whitespace-nowrap px-2">統帥代號：</span>
        <input id="player-name-input" type="text" maxlength="14" value="指揮官_蒼狼"
          class="w-full bg-slate-800 text-white text-sm font-bold px-3 py-2 rounded-lg border border-slate-600 focus:outline-none focus:border-sky-400"
          placeholder="輸入你的名稱..." />
      </div>

      <!-- 主選單按鈕 -->
      <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 w-full max-w-xl mb-6">
        <button onclick="App.goToLobby()"
          class="btn-tactical py-4 px-6 rounded-xl bg-sky-600 hover:bg-sky-500 text-white font-bold text-lg shadow-lg border border-sky-400/40 flex items-center justify-center gap-2">
          <span>🏛️ 進入大廳 / 聊天室</span>
        </button>
        <button onclick="App.quickStart4v4()"
          class="btn-tactical py-4 px-6 rounded-xl bg-emerald-600 hover:bg-emerald-500 text-white font-bold text-lg shadow-lg border border-emerald-400/40 flex items-center justify-center gap-2">
          <span>⚔️ 快速開始 4V4 演練</span>
        </button>
      </div>

      <!-- 簡明規則卡片 -->
      <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 w-full text-left text-xs text-slate-300 bg-slate-900/70 p-4 rounded-xl border border-slate-800">
        <div>
          <div class="font-bold text-sky-400 mb-1">🏰 據點與升旗</div>
          <div class="text-slate-400">中立營只產錢 150%，佔領需 20 秒（每多 5 兵加速 10%）。</div>
        </div>
        <div>
          <div class="font-bold text-amber-400 mb-1">⚖️ 信任點轉換</div>
          <div class="text-slate-400">初始 100 點，每 10 點轉 +1 人力/分，每 15 點轉 +1 研究/分。</div>
        </div>
        <div>
          <div class="font-bold text-emerald-400 mb-1">🚩 4V4 分隊微操</div>
          <div class="text-slate-400">進攻 / 防守 / 支援選點 / 回家；空缺將軍由指揮官接管。</div>
        </div>
        <div>
          <div class="font-bold text-rose-400 mb-1">☢️ 研發與核彈</div>
          <div class="text-slate-400">強化三兵種、解鎖砲兵騎兵醫護，發射戰術核彈改變戰局。</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ==================== 2. 大廳與聊天室 (LOBBY & CHAT) ==================== -->
  <div id="screen-lobby" class="hidden w-full h-full flex flex-col p-3 sm:p-5 gap-3">
    <!-- 頂部導覽列 -->
    <div class="panel-glass rounded-xl px-4 py-3 flex items-center justify-between">
      <div class="flex items-center gap-3">
        <button onclick="App.showScreen('screen-menu')" class="btn-tactical px-3 py-1.5 rounded-lg bg-slate-800 hover:bg-slate-700 text-xs font-bold border border-slate-600">
          ← 返回主畫面
        </button>
        <h2 class="text-lg sm:text-xl font-black text-white">戰略指揮大廳 & 即時頻道</h2>
      </div>
      <div class="flex items-center gap-3">
        <span class="text-xs text-slate-400 hidden sm:inline">當前身分：<strong id="lobby-player-name" class="text-sky-400">指揮官_蒼狼</strong></span>
        <button onclick="App.createAndEnterRoom('自訂 4V4 榮耀戰場')" class="btn-tactical px-4 py-2 rounded-lg bg-sky-600 hover:bg-sky-500 text-white text-sm font-bold shadow">
          + 創建 4V4 房間
        </button>
      </div>
    </div>

    <!-- 主體左右分欄：左側房間列表 / 右側大廳聊天室 -->
    <div class="flex-1 grid grid-cols-1 lg:grid-cols-12 gap-3 min-h-0">
      <!-- 房間列表 -->
      <div class="lg:col-span-7 panel-glass rounded-xl p-4 flex flex-col min-h-0">
        <div class="flex items-center justify-between mb-3 pb-2 border-b border-slate-800">
          <span class="font-bold text-slate-200">🌐 公開對戰房間列表 (4V4 + 20 觀戰席)</span>
          <span class="text-xs text-emerald-400">● 連線狀態：正常</span>
        </div>
        <div id="lobby-room-list" class="flex-1 overflow-y-auto space-y-2.5 pr-1">
          <!-- 動態產生房間卡片 -->
        </div>
      </div>

      <!-- 大廳聊天室 -->
      <div class="lg:col-span-5 panel-glass rounded-xl p-4 flex flex-col min-h-0">
        <div class="flex items-center justify-between mb-2 pb-2 border-b border-slate-800">
          <span class="font-bold text-sky-400">💬 大廳公共聊天室</span>
          <span class="text-xs text-slate-400">文明發言・自動過濾敏感詞</span>
        </div>
        <div id="lobby-chat-messages" class="flex-1 overflow-y-auto space-y-2 p-2 bg-slate-950/60 rounded-lg border border-slate-800/80 text-sm">
          <!-- 聊天訊息 -->
        </div>
        <form onsubmit="App.handleChatSubmit(event, 'lobby-chat-input')" class="mt-2 flex gap-2">
          <input id="lobby-chat-input" type="text" maxlength="80" placeholder="輸入聊天訊息..."
            class="flex-1 bg-slate-900 text-white text-sm px-3 py-2 rounded-lg border border-slate-700 focus:outline-none focus:border-sky-400" />
          <button type="submit" class="btn-tactical px-4 py-2 rounded-lg bg-sky-600 hover:bg-sky-500 text-white text-sm font-bold">
            發送
          </button>
        </form>
      </div>
    </div>
  </div>

  <!-- ==================== 3. 4V4 + 20 觀戰席房間 (4V4 ROOM) ==================== -->
  <div id="screen-room" class="hidden w-full h-full flex flex-col p-3 sm:p-5 gap-3">
    <!-- 房間頂欄 -->
    <div class="panel-glass rounded-xl px-4 py-3 flex flex-wrap items-center justify-between gap-2">
      <div class="flex items-center gap-3">
        <button onclick="App.showScreen('screen-lobby')" class="btn-tactical px-3 py-1.5 rounded-lg bg-slate-800 hover:bg-slate-700 text-xs font-bold border border-slate-600">
          ← 離開房間
        </button>
        <div>
          <h2 id="room-title-display" class="text-base sm:text-lg font-black text-white">自訂 4V4 榮耀戰場</h2>
          <div class="text-xs text-slate-400">規則：1 指揮官 + 3 將軍（空缺將軍自動轉交指揮官接管）</div>
        </div>
      </div>
      <div class="flex items-center gap-2">
        <button onclick="App.startBattle()" class="btn-tactical px-6 py-2.5 rounded-xl bg-emerald-600 hover:bg-emerald-500 text-white font-black text-sm sm:text-base shadow-lg border border-emerald-400/50">
          ⚔️ 開始戰役
        </button>
      </div>
    </div>

    <!-- 房間主體：藍方 4 席 vs 紅方 4 席 + 20 觀戰席 + 房內聊天 -->
    <div class="flex-1 grid grid-cols-1 lg:grid-cols-12 gap-3 min-h-0 overflow-y-auto lg:overflow-hidden">
      <!-- 左側 8 欄：對戰席 + 20 觀戰席 -->
      <div class="lg:col-span-8 flex flex-col gap-3 min-h-0">
        <!-- 藍方 vs 紅方 -->
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
          <!-- 藍方隊伍 -->
          <div class="panel-glass rounded-xl p-3 border-t-4 border-t-sky-500 flex flex-col">
            <div class="flex items-center justify-between mb-2.5 pb-2 border-b border-slate-800">
              <div class="flex items-center gap-2">
                <span class="w-3 h-3 rounded-full bg-sky-500"></span>
                <span class="font-black text-sky-400">藍方軍團 (<span id="blue-count">0</span>/4)</span>
              </div>
              <div class="flex items-center gap-1.5">
                <button onclick="App.addAIToTeam('blue')" class="btn-tactical px-2.5 py-1 rounded-lg bg-sky-600 hover:bg-sky-500 text-white text-xs font-bold shadow">
                  + AI
                </button>
                <button onclick="App.clearTeamAI('blue')" class="btn-tactical px-2 py-1 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs">
                  清空AI
                </button>
              </div>
            </div>
            <div id="blue-slots-container" class="space-y-2">
              <!-- 4 個藍方席位 -->
            </div>
          </div>

          <!-- 紅方隊伍 -->
          <div class="panel-glass rounded-xl p-3 border-t-4 border-t-rose-500 flex flex-col">
            <div class="flex items-center justify-between mb-2.5 pb-2 border-b border-slate-800">
              <div class="flex items-center gap-2">
                <span class="w-3 h-3 rounded-full bg-rose-500"></span>
                <span class="font-black text-rose-400">紅方軍團 (<span id="red-count">0</span>/4)</span>
              </div>
              <div class="flex items-center gap-1.5">
                <button onclick="App.addAIToTeam('red')" class="btn-tactical px-2.5 py-1 rounded-lg bg-rose-600 hover:bg-rose-500 text-white text-xs font-bold shadow">
                  + AI
                </button>
                <button onclick="App.clearTeamAI('red')" class="btn-tactical px-2 py-1 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs">
                  清空AI
                </button>
              </div>
            </div>
            <div id="red-slots-container" class="space-y-2">
              <!-- 4 個紅方席位 -->
            </div>
          </div>
        </div>

        <!-- 20 個觀戰席 -->
        <div class="panel-glass rounded-xl p-3 flex-1 flex flex-col min-h-[150px]">
          <div class="flex items-center justify-between mb-2 pb-1.5 border-b border-slate-800">
            <span class="text-xs sm:text-sm font-bold text-amber-400">👁️ 觀戰席 (<span id="spec-count">0</span>/20) — 觀戰可開啟全圖上帝視角</span>
            <div class="flex gap-1.5">
              <button onclick="App.joinSpectator()" class="btn-tactical px-2.5 py-1 rounded-lg bg-amber-600/80 hover:bg-amber-500 text-white text-xs font-bold">
                切換至觀戰席
              </button>
              <button onclick="App.addSpectatorAI()" class="btn-tactical px-2 py-1 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs">
                + 模擬觀眾
              </button>
            </div>
          </div>
          <div id="spectator-slots-grid" class="grid grid-cols-4 sm:grid-cols-5 gap-1.5 overflow-y-auto pr-1">
            <!-- 20 格觀戰席 -->
          </div>
        </div>
      </div>

      <!-- 右側 4 欄：房間聊天室 -->
      <div class="lg:col-span-4 panel-glass rounded-xl p-3 flex flex-col min-h-[220px]">
        <div class="flex items-center justify-between mb-2 pb-1.5 border-b border-slate-800">
          <span class="font-bold text-sm text-sky-400">💬 戰前房間頻道</span>
          <span class="text-xs text-slate-400">溝通路線分配</span>
        </div>
        <div id="room-chat-messages" class="flex-1 overflow-y-auto space-y-2 p-2 bg-slate-950/60 rounded-lg border border-slate-800/80 text-sm">
          <!-- 共用聊天訊息 -->
        </div>
        <form onsubmit="App.handleChatSubmit(event, 'room-chat-input')" class="mt-2 flex gap-2">
          <input id="room-chat-input" type="text" maxlength="80" placeholder="輸入戰術對話..."
            class="flex-1 bg-slate-900 text-white text-sm px-3 py-2 rounded-lg border border-slate-700 focus:outline-none focus:border-sky-400" />
          <button type="submit" class="btn-tactical px-3 py-2 rounded-lg bg-sky-600 hover:bg-sky-500 text-white text-sm font-bold">
            發送
          </button>
        </form>
      </div>
    </div>
  </div>

  <!-- ==================== 4. 三線策略戰場 (BATTLEFIELD) ==================== -->
  <div id="screen-battle" class="hidden w-full h-full flex flex-col relative overflow-hidden">
    <!-- 頂部資源與本營血量狀態列 -->
    <div class="bg-slate-950/95 border-b border-slate-800 px-3 py-2 flex flex-wrap items-center justify-between gap-2 z-20">
      <!-- 左側：己方資源 -->
      <div class="flex items-center gap-2 sm:gap-3 flex-wrap">
        <div class="flex items-center gap-1.5 bg-slate-900 px-2.5 py-1 rounded-lg border border-amber-500/30">
          <span class="text-amber-400 text-sm">💰</span>
          <div>
            <div class="text-[10px] text-slate-400 leading-none">資金 (<span id="hud-money-rate">+120</span>/分)</div>
            <div id="hud-money" class="text-sm font-black text-amber-300">500</div>
          </div>
        </div>
        <div class="flex items-center gap-1.5 bg-slate-900 px-2.5 py-1 rounded-lg border border-emerald-500/30">
          <span class="text-emerald-400 text-sm">👥</span>
          <div>
            <div class="text-[10px] text-slate-400 leading-none">人力 (<span id="hud-manpower-rate">+0</span>/分)</div>
            <div id="hud-manpower" class="text-sm font-black text-emerald-300">20</div>
          </div>
        </div>
        <div class="flex items-center gap-1.5 bg-slate-900 px-2.5 py-1 rounded-lg border border-sky-500/30">
          <span class="text-sky-400 text-sm">🔬</span>
          <div>
            <div class="text-[10px] text-slate-400 leading-none">研究點 (<span id="hud-research-rate">+0</span>/分)</div>
            <div id="hud-research" class="text-sm font-black text-sky-300">10</div>
          </div>
        </div>
        <button onclick="BattleUI.toggleTrustModal()" class="btn-tactical flex items-center gap-1.5 bg-indigo-950/90 hover:bg-indigo-900 px-2.5 py-1 rounded-lg border border-indigo-400/50">
          <span class="text-indigo-300 text-sm">⚖️</span>
          <div class="text-left">
            <div class="text-[10px] text-indigo-300 leading-none">信任點 (點擊分配)</div>
            <div id="hud-trust" class="text-sm font-black text-white">100</div>
          </div>
        </button>
      </div>

      <!-- 中央：藍紅本營血條與時間 -->
      <div class="flex items-center gap-3 bg-slate-900/90 px-3 py-1 rounded-xl border border-slate-700">
        <div class="text-right">
          <div class="text-[10px] font-bold text-sky-400">藍方本營</div>
          <div class="w-20 sm:w-28 h-2.5 bg-slate-800 rounded-full overflow-hidden">
            <div id="hud-blue-base-hp" class="h-full bg-sky-500 transition-all duration-200" style="width: 100%"></div>
          </div>
        </div>
        <div id="hud-timer" class="text-xs sm:text-sm font-black text-slate-200 px-2 py-0.5 bg-slate-800 rounded">00:00</div>
        <div class="text-left">
          <div class="text-[10px] font-bold text-rose-400">紅方本營</div>
          <div class="w-20 sm:w-28 h-2.5 bg-slate-800 rounded-full overflow-hidden">
            <div id="hud-red-base-hp" class="h-full bg-rose-500 transition-all duration-200" style="width: 100%"></div>
          </div>
        </div>
      </div>

      <!-- 右側：功能按鈕列 -->
      <div class="flex items-center gap-1.5">
        <button onclick="BattleUI.togglePanel('panel-recruit')" class="btn-tactical px-2.5 py-1.5 rounded-lg bg-emerald-700 hover:bg-emerald-600 text-white text-xs font-bold">
          ⚔️ 徵兵/擴建
        </button>
        <button onclick="BattleUI.togglePanel('panel-rd')" class="btn-tactical px-2.5 py-1.5 rounded-lg bg-purple-700 hover:bg-purple-600 text-white text-xs font-bold">
          🔬 研發部門
        </button>
        <button onclick="BattleUI.togglePanel('panel-battle-chat')" class="btn-tactical px-2.5 py-1.5 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-200 text-xs font-bold border border-slate-600">
          💬 戰場頻道
        </button>
        <button onclick="Battle.exitToMenu()" class="btn-tactical px-2.5 py-1.5 rounded-lg bg-rose-900/80 hover:bg-rose-800 text-rose-200 text-xs font-bold">
          撤離
        </button>
      </div>
    </div>

    <!-- 中間：Canvas 戰場主視窗 -->
    <div id="canvas-container" class="flex-1 relative min-h-0 bg-[#0d1322]">
      <canvas id="battle-canvas"></canvas>

      <!-- 操作提示橫幅（例如「請點擊地圖選擇支援目標」或「請點擊核彈落點」） -->
      <div id="targeting-banner" class="hidden absolute top-3 left-1/2 -translate-x-1/2 bg-amber-600/95 text-white px-4 py-2 rounded-full text-xs sm:text-sm font-bold shadow-lg flex items-center gap-3 z-20">
        <span id="targeting-banner-text">請在地圖上點選目標位置</span>
        <button onclick="Battle.cancelTargeting()" class="px-2 py-0.5 bg-black/30 rounded-full text-xs hover:bg-black/50">取消</button>
      </div>

      <!-- 五條悟專屬召喚與冷卻狀態列（解鎖 Gojo 彩蛋後自動出現） -->
      <div id="gojo-hud-widget" class="hidden absolute top-3 left-3 panel-glass rounded-xl p-2.5 border border-cyan-400/60 flex items-center gap-2.5 z-20">
        <div class="w-9 h-9 rounded-lg bg-cyan-950 border border-cyan-400 flex items-center justify-center text-lg font-black text-cyan-300">
          悟
        </div>
        <div>
          <div class="text-xs font-black text-cyan-300">特級術師・五條悟</div>
          <button id="gojo-summon-btn" onclick="Battle.summonGojoManual()" class="mt-1 px-2.5 py-1 rounded bg-cyan-600 hover:bg-cyan-500 text-white text-[11px] font-bold">
            已就緒・點擊召喚
          </button>
        </div>
      </div>

      <!-- 四向戰術指令浮動盤（點擊分隊或路線據點時顯示：進攻/防守/支援/回家） -->
      <div id="command-wheel" class="hidden absolute bottom-20 left-1/2 -translate-x-1/2 panel-glass rounded-2xl p-3 border border-sky-400/50 z-20 w-[92%] max-w-md">
        <div class="flex items-center justify-between mb-2 pb-1 border-b border-slate-700">
          <span id="cmd-target-label" class="text-xs font-black text-sky-400">指揮分隊：上路先鋒隊</span>
          <button onclick="BattleUI.hideCommandWheel()" class="text-xs text-slate-400 hover:text-white px-1">✕</button>
        </div>
        <div class="grid grid-cols-4 gap-2">
          <button onclick="Battle.issueCommand('attack')" class="btn-tactical py-2 rounded-xl bg-rose-600/90 hover:bg-rose-500 text-white text-xs font-bold flex flex-col items-center">
            <span>⚔️ 進攻</span>
            <span class="text-[10px] opacity-80">全速推進</span>
          </button>
          <button onclick="Battle.issueCommand('defend')" class="btn-tactical py-2 rounded-xl bg-sky-600/90 hover:bg-sky-500 text-white text-xs font-bold flex flex-col items-center">
            <span>🛡️ 防守</span>
            <span class="text-[10px] opacity-80">駐守據點</span>
          </button>
          <button onclick="Battle.issueCommand('support')" class="btn-tactical py-2 rounded-xl bg-amber-600/90 hover:bg-amber-500 text-white text-xs font-bold flex flex-col items-center">
            <span>🎯 支援</span>
            <span class="text-[10px] opacity-80">自選目標</span>
          </button>
          <button onclick="Battle.issueCommand('retreat')" class="btn-tactical py-2 rounded-xl bg-slate-700 hover:bg-slate-600 text-white text-xs font-bold flex flex-col items-center">
            <span>🏠 回家</span>
            <span class="text-[10px] opacity-80">撤回本營</span>
          </button>
        </div>
      </div>

      <!-- 信任點分配彈窗 -->
      <div id="modal-trust" class="hidden absolute top-14 left-4 panel-glass rounded-xl p-4 w-80 z-30 border border-indigo-400/50">
        <div class="flex justify-between items-center mb-2 pb-1.5 border-b border-slate-700">
          <span class="font-bold text-sm text-indigo-300">⚖️ 信任點戰略轉換</span>
          <button onclick="BattleUI.toggleTrustModal()" class="text-slate-400 hover:text-white text-xs">✕</button>
        </div>
        <p class="text-[11px] text-slate-400 mb-3">
          每分鐘自然獲得 +30 信任點，擊殺敵兵 +1 點。將可用信任點投入轉換產能：
        </p>
        <div class="space-y-3 text-xs">
          <div class="bg-slate-900 p-2.5 rounded-lg border border-slate-800">
            <div class="flex justify-between mb-1">
              <span class="text-emerald-400 font-bold">👥 轉化人力 (每 10 點 = +1/分)</span>
              <span id="trust-manpower-alloc" class="font-black text-white">0 點 (+0/分)</span>
            </div>
            <div class="flex gap-2 mt-1.5">
              <button onclick="Battle.allocateTrust('manpower', -10)" class="flex-1 py-1 bg-slate-800 hover:bg-slate-700 rounded font-bold">-10</button>
              <button onclick="Battle.allocateTrust('manpower', 10)" class="flex-1 py-1 bg-emerald-700 hover:bg-emerald-600 rounded font-bold">+10</button>
            </div>
          </div>
          <div class="bg-slate-900 p-2.5 rounded-lg border border-slate-800">
            <div class="flex justify-between mb-1">
              <span class="text-sky-400 font-bold">🔬 轉化研究點 (每 15 點 = +1/分)</span>
              <span id="trust-research-alloc" class="font-black text-white">0 點 (+0/分)</span>
            </div>
            <div class="flex gap-2 mt-1.5">
              <button onclick="Battle.allocateTrust('research', -15)" class="flex-1 py-1 bg-slate-800 hover:bg-slate-700 rounded font-bold">-15</button>
              <button onclick="Battle.allocateTrust('research', 15)" class="flex-1 py-1 bg-sky-700 hover:bg-sky-600 rounded font-bold">+15</button>
            </div>
          </div>
        </div>
      </div>

      <!-- 徵兵與產兵擴建面板 -->
      <div id="panel-recruit" class="hidden absolute top-14 right-3 panel-glass rounded-xl p-4 w-80 sm:w-96 max-h-[75vh] overflow-y-auto z-30 border border-emerald-500/40">
        <div class="flex justify-between items-center mb-2 pb-1.5 border-b border-slate-700">
          <span class="font-bold text-sm text-emerald-400">⚔️ 兵營徵召與產線擴建</span>
          <button onclick="BattleUI.togglePanel('panel-recruit')" class="text-slate-400 hover:text-white text-xs">✕</button>
        </div>
        <!-- 產兵效率升級 -->
        <div class="bg-slate-900/90 p-2.5 rounded-lg border border-slate-700 mb-3">
          <div class="flex justify-between items-center text-xs mb-1">
            <span class="font-bold text-amber-300">🏭 本營自動產兵效率 Lv.<span id="spawn-lv-text">1</span></span>
            <span class="text-slate-400">基準 2隻/分</span>
          </div>
          <button id="btn-upgrade-spawn" onclick="Battle.upgradeSpawnRate()" class="btn-tactical w-full py-1.5 rounded bg-amber-600 hover:bg-amber-500 text-white text-xs font-bold">
            升級產兵效率 (400錢 + 15人力)
          </button>
        </div>
        <!-- 手動徵兵清單 -->
        <div class="text-xs font-bold text-slate-300 mb-2">手動緊急徵召（立即於己方本營集結）：</div>
        <div id="recruit-unit-list" class="space-y-2">
          <!-- 動態生成兵種按鈕 -->
        </div>
      </div>

      <!-- 研發部門面板 -->
      <div id="panel-rd" class="hidden absolute top-14 right-3 panel-glass rounded-xl p-4 w-80 sm:w-[420px] max-h-[78vh] overflow-y-auto z-30 border border-purple-500/40">
        <div class="flex justify-between items-center mb-2 pb-1.5 border-b border-slate-700">
          <span class="font-bold text-sm text-purple-400">🔬 軍事研發部門 (R&D)</span>
          <button onclick="BattleUI.togglePanel('panel-rd')" class="text-slate-400 hover:text-white text-xs">✕</button>
        </div>
        <!-- 當前研發進度條 -->
        <div id="rd-progress-box" class="hidden bg-slate-900 p-2.5 rounded-lg border border-purple-500/50 mb-3">
          <div class="flex justify-between text-xs mb-1">
            <span id="rd-active-name" class="font-bold text-purple-300">正在研發...</span>
            <span id="rd-active-pct" class="text-slate-300">0%</span>
          </div>
          <div class="w-full h-2 bg-slate-800 rounded-full overflow-hidden">
            <div id="rd-active-bar" class="h-full bg-purple-500 transition-all duration-150" style="width: 0%"></div>
          </div>
        </div>
        <!-- 三大研發分支標籤 -->
        <div class="grid grid-cols-3 gap-1.5 mb-3 text-xs">
          <button onclick="BattleUI.switchRdTab('tech')" id="rd-tab-tech" class="py-1.5 rounded bg-purple-600 text-white font-bold">兵種強化</button>
          <button onclick="BattleUI.switchRdTab('units')" id="rd-tab-units" class="py-1.5 rounded bg-slate-800 text-slate-300 font-bold">新兵種解鎖</button>
          <button onclick="BattleUI.switchRdTab('items')" id="rd-tab-items" class="py-1.5 rounded bg-slate-800 text-slate-300 font-bold">戰術物品/核彈</button>
        </div>
        <div id="rd-items-container" class="space-y-2">
          <!-- 動態產生研發項目 -->
        </div>
      </div>

      <!-- 戰場內聊天室浮動視窗 -->
      <div id="panel-battle-chat" class="hidden absolute top-14 right-3 panel-glass rounded-xl p-3 w-80 h-72 flex flex-col z-30 border border-slate-600">
        <div class="flex justify-between items-center mb-1.5 pb-1 border-b border-slate-700">
          <span class="font-bold text-xs text-sky-400">💬 戰場即時頻道</span>
          <button onclick="BattleUI.togglePanel('panel-battle-chat')" class="text-slate-400 hover:text-white text-xs">✕</button>
        </div>
        <div id="battle-chat-messages" class="flex-1 overflow-y-auto space-y-1.5 p-2 bg-slate-950/70 rounded text-xs"></div>
        <form onsubmit="App.handleChatSubmit(event, 'battle-chat-input')" class="mt-2 flex gap-1.5">
          <input id="battle-chat-input" type="text" maxlength="80" placeholder="輸入指令或對話..."
            class="flex-1 bg-slate-900 text-white text-xs px-2.5 py-1.5 rounded border border-slate-700 focus:outline-none focus:border-sky-400" />
          <button type="submit" class="px-3 py-1.5 rounded bg-sky-600 text-white text-xs font-bold">發送</button>
        </form>
      </div>

      <!-- 勝負結算畫面 -->
      <div id="battle-result-overlay" class="hidden absolute inset-0 bg-slate-950/85 flex items-center justify-center p-4 z-40">
        <div class="panel-glass rounded-2xl p-6 sm:p-8 max-w-md w-full text-center border border-slate-600">
          <div id="result-badge" class="inline-block px-3 py-1 rounded-full text-xs font-bold mb-2">戰役結束</div>
          <h2 id="result-title" class="text-3xl font-black text-white mb-2">藍方獲得勝利！</h2>
          <p id="result-desc" class="text-sm text-slate-300 mb-6">敵方本營已被徹底摧毀，三線戰場全面克捷。</p>
          <button onclick="Battle.exitToMenu()" class="btn-tactical w-full py-3 rounded-xl bg-sky-600 hover:bg-sky-500 text-white font-bold">
            返回主畫面
          </button>
        </div>
      </div>
    </div>

    <!-- 底部：4V4 分隊控制列 & 已解鎖戰術道具快捷列 -->
    <div class="bg-slate-950/95 border-t border-slate-800 px-3 py-2 flex flex-wrap items-center justify-between gap-2 z-20">
      <!-- 4 支編制分隊按鈕 -->
      <div id="squad-bar" class="flex items-center gap-1.5 sm:gap-2 overflow-x-auto">
        <!-- 動態產生分隊按鈕 -->
      </div>

      <!-- 戰術物品快捷欄（含戰術核彈） -->
      <div id="tactical-items-bar" class="flex items-center gap-1.5 overflow-x-auto">
        <span class="text-[11px] text-slate-500">戰術物品：尚未研發</span>
      </div>
    </div>
  </div>

  <!-- ==================== 隱藏外掛作弊面板（僅限輸入 5 次 YEE 的本人螢幕直接出現，無閃爍、無提示） ==================== -->
  <div id="secret-cheat-panel" class="hidden fixed bottom-16 right-4 w-72 panel-glass rounded-xl p-3 border border-emerald-500/70 z-50 shadow-2xl">
    <div id="cheat-panel-header" class="flex items-center justify-between pb-1.5 mb-2 border-b border-slate-700 cursor-move">
      <span class="text-xs font-black text-emerald-400">🛠️ DEV OVERRIDE // 作弊控制台</span>
      <button onclick="CheatSystem.toggleMinimize()" class="text-xs text-slate-400 hover:text-white px-1.5 py-0.5 bg-slate-800 rounded">
        收合/展開
      </button>
    </div>
    <div id="cheat-panel-body" class="grid grid-cols-2 gap-1.5 text-xs">
      <button onclick="CheatSystem.addMoney()" class="btn-tactical py-1.5 px-2 rounded bg-amber-700/90 hover:bg-amber-600 text-white font-bold">
        +5000 資金
      </button>
      <button onclick="CheatSystem.addResources()" class="btn-tactical py-1.5 px-2 rounded bg-sky-700/90 hover:bg-sky-600 text-white font-bold">
        +100 人力/研究
      </button>
      <button onclick="CheatSystem.addTrust()" class="btn-tactical py-1.5 px-2 rounded bg-indigo-700/90 hover:bg-indigo-600 text-white font-bold">
        +100 信任點
      </button>
      <button onclick="CheatSystem.instantResearch()" class="btn-tactical py-1.5 px-2 rounded bg-purple-700/90 hover:bg-purple-600 text-white font-bold">
        瞬間完成研發
      </button>
      <button onclick="CheatSystem.grantNuke()" class="btn-tactical py-1.5 px-2 rounded bg-rose-700/90 hover:bg-rose-600 text-white font-bold">
        ☢️ 免費戰術核彈
      </button>
      <button onclick="CheatSystem.instantFlags()" class="btn-tactical py-1.5 px-2 rounded bg-emerald-700/90 hover:bg-emerald-600 text-white font-bold">
        🚩 全線瞬間升旗
      </button>
      <button onclick="CheatSystem.toggleGodMode()" id="cheat-god-btn" class="btn-tactical py-1.5 px-2 rounded bg-slate-800 hover:bg-slate-700 text-slate-200 font-bold border border-slate-600">
        無敵模式：關
      </button>
      <button onclick="CheatSystem.instantWin()" class="btn-tactical py-1.5 px-2 rounded bg-teal-700/90 hover:bg-teal-600 text-white font-bold">
        🏆 立即獲勝
      </button>
    </div>
  </div>

<script>
/**
 * ============================================================================
 * 1. 全域狀態與彩蛋系統 (YEE -> ### x5 外掛面板 / Gojo -> #### x5 召喚五條悟)
 * ============================================================================
 */
const GlobalState = {
  playerName: '指揮官_蒼狼',
  yeeCount: 0,
  cheatUnlocked: false,
  gojoCount: 0,
  gojoUnlocked: false,
  gojoPendingSpawn: false, // 若在大廳/房間解鎖，進戰場時直接於己方本營召喚
  chatHistory: [
    { sender: '系統', text: '歡迎來到《三線統帥：4V4 戰略紀元》大廳！', color: 'text-slate-400' },
    { sender: '破軍將軍', text: '有人要開 4V4 嗎？中立小營 150% 產錢開局必搶！', color: 'text-sky-400' },
    { sender: '赤焰統帥', text: '進房直接按隊伍上方的 +AI 就能秒開局了。', color: 'text-rose-400' }
  ],
  rooms: [
    { id: 101, name: '新手教學演練房 (可+AI)', host: '破軍將軍', players: '3/8', specs: '2/20', status: '等待中' },
    { id: 102, name: '高手三線微操對決', host: '赤焰統帥', players: '6/8', specs: '5/20', status: '等待中' },
    { id: 103, name: '全 AI 觀戰測試局', host: '戰術觀察員', players: '8/8', specs: '12/20', status: '準備中' }
  ],
  currentRoom: null
};

const CheatSystem = {
  godMode: false,
  reset() {
    this.godMode = false;
    const btn = document.getElementById('cheat-god-btn');
    if (btn) {
      btn.textContent = '無敵模式：關';
      btn.classList.remove('bg-emerald-700');
    }
    const panel = document.getElementById('secret-cheat-panel');
    if (panel) {
      panel.classList.add('hidden');
    }
    const body = document.getElementById('cheat-panel-body');
    if (body) {
      body.classList.remove('hidden');
    }
  },
  unlockSilently() {
    if (GlobalState.cheatUnlocked) return;
    GlobalState.cheatUnlocked = true;
// ... existing code ... -->
  goToLobby() {
    this.syncPlayerName();
    Battle.hardResetAll();
    this.renderLobbyRooms();
    this.renderAllChatBoxes();
    this.showScreen('screen-lobby');
  },

  quickStart4v4() {
    this.syncPlayerName();
    Battle.hardResetAll();
    this.createAndEnterRoom('4V4 快速演練戰場', true);
    this.startBattle();
  },

  renderLobbyRooms() {
// ... existing code ... -->
  createAndEnterRoom(roomName, autoFillAI = false) {
    this.syncPlayerName();
    Battle.hardResetAll();
    GlobalState.currentRoom = {
      name: roomName,
      blue: [
// ... existing code ... -->
  startBattle() {
    Battle.hardResetAll();
    this.showScreen('screen-battle');
    Battle.init(GlobalState.currentRoom);
  }
};

/**
 * ============================================================================
 * 3. 兵種規格與軍事研發部門資料表
 * ============================================================================
 */
// ... existing code ... -->
  effects: [],
  radiationZones: [],

  // 每一局開局與離場時的徹底硬重置（資源、研發、據點、兵力、彩蛋計數與冷卻全數歸零）
  hardResetAll() {
    this.running = false;
    this.elapsed = 0;
    this.selectedSquadIdx = 0;
    this.targetingMode = null;

    // 1. 清空戰場實體、彈道、特效與輻射區
    this.teams = {};
    this.camps = [];
    this.units = [];
    this.projectiles = [];
    this.effects = [];
    this.radiationZones = [];

    // 2. 重置雙彩蛋計數、外掛面板與五條悟解鎖／冷卻狀態
    GlobalState.yeeCount = 0;
    GlobalState.cheatUnlocked = false;
    GlobalState.gojoCount = 0;
    GlobalState.gojoUnlocked = false;
    GlobalState.gojoPendingSpawn = false;
    CheatSystem.reset();

    const gojoWidget = document.getElementById('gojo-hud-widget');
    if (gojoWidget) gojoWidget.classList.add('hidden');
    const gojoBtn = document.getElementById('gojo-summon-btn');
    if (gojoBtn) {
      gojoBtn.textContent = '已就緒・點擊召喚';
      gojoBtn.className = 'mt-1 px-2.5 py-1 rounded bg-cyan-600 hover:bg-cyan-500 text-white text-[11px] font-bold cursor-pointer';
    }

    // 3. 關閉所有浮動視窗、指令盤與結算遮罩
    ['targeting-banner', 'command-wheel', 'modal-trust', 'panel-recruit', 'panel-rd', 'panel-battle-chat', 'battle-result-overlay'].forEach(id => {
      const el = document.getElementById(id);
      if (el) el.classList.add('hidden');
    });
  },

  init(room) {
    this.canvas = document.getElementById('battle-canvas');
    this.ctx = this.canvas.getContext('2d');
    this.resizeCanvas();
    if (!this._resizeBound) {
      this._resizeBound = true;
      window.addEventListener('resize', () => this.resizeCanvas());
    }

    // 判定玩家陣營或觀戰身分
    this.playerTeam = 'blue';
// ... existing code ... -->
    // 開局雙方先派出一波初始巡邏兵與統帥單位
    this.spawnInitialWaves();

    this.bindCanvasEvents();
    BattleUI.switchRdTab('tech');
    BattleUI.initAll();

    this.running = true;
// ... existing code ... -->
  exitToMenu() {
    this.hardResetAll();
    App.showScreen('screen-menu');
  }
};

/**
 * ============================================================================
 * 5. 戰場 HUD 介面渲染與互動控制器
 * ============================================================================
 */
// ... existing code ... -->
