<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>三線統帥：4V4 戰略紀元</title>
  <style>
    :root {
      --bg: #0b1017;
      --surface: #131b26;
      --surface-2: #1a2534;
      --surface-3: #223044;
      --line: #2f425c;
      --text: #e8eff8;
      --sub: #9cb0c9;
      --blue: #3b82f6;
      --blue-bg: rgba(59, 130, 246, 0.16);
      --red: #ef4444;
      --red-bg: rgba(239, 68, 68, 0.16);
      --gold: #f59e0b;
      --green: #10b981;
      --violet: #a855f7;
    }

    * {
      box-sizing: border-box;
      user-select: none;
    }

    html, body {
      margin: 0;
      padding: 0;
      background: var(--bg);
      color: var(--text);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Noto Sans TC", sans-serif;
      min-height: 100vh;
    }

    button, input, select {
      font-family: inherit;
    }

    button {
      background: var(--surface-3);
      color: var(--text);
      border: 1px solid var(--line);
      border-radius: 8px;
      padding: 7px 13px;
      font-size: 13px;
      cursor: pointer;
      transition: background 0.16s ease, border-color 0.16s ease, opacity 0.16s ease;
    }

    button:hover:not(:disabled) {
      background: #2c3e58;
      border-color: #4d6b94;
    }

    button:disabled {
      opacity: 0.42;
      cursor: not-allowed;
    }

    button.btn-blue {
      background: var(--blue);
      border-color: #60a5fa;
      color: #fff;
      font-weight: 600;
    }

    button.btn-blue:hover:not(:disabled) {
      background: #2563eb;
    }

    button.btn-green {
      background: rgba(16, 185, 129, 0.22);
      border-color: rgba(16, 185, 129, 0.65);
      color: #6ee7b7;
      font-weight: 600;
    }

    button.btn-red {
      background: rgba(239, 68, 68, 0.22);
      border-color: rgba(239, 68, 68, 0.65);
      color: #fca5a5;
      font-weight: 600;
    }

    button.btn-gold {
      background: rgba(245, 158, 11, 0.2);
      border-color: rgba(245, 158, 11, 0.65);
      color: #fcd34d;
      font-weight: 600;
    }

    .view {
      display: none;
      min-height: 100vh;
      padding: 16px;
    }

    .view.is-active {
      display: block;
    }

    .panel {
      background: var(--surface);
      border: 1px solid var(--line);
      border-radius: 12px;
      padding: 14px;
    }

    .panel-hd {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
      margin-bottom: 10px;
    }

    .panel-hd h2, .panel-hd h3 {
      margin: 0;
      font-size: 16px;
    }

    .muted {
      color: var(--sub);
      font-size: 12px;
    }

    .tag {
      background: var(--surface-2);
      border: 1px solid var(--line);
      border-radius: 999px;
      padding: 4px 11px;
      font-size: 12px;
      color: var(--sub);
    }

    .nav-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 10px;
      background: var(--surface);
      border: 1px solid var(--line);
      border-radius: 12px;
      padding: 11px 16px;
      margin-bottom: 14px;
    }

    .nav-group {
      display: flex;
      align-items: center;
      flex-wrap: wrap;
      gap: 8px;
    }

    /* 主畫面 */
    .home-box {
      max-width: 820px;
      margin: 44px auto;
      background: var(--surface);
      border: 1px solid var(--line);
      border-radius: 16px;
      padding: 34px;
      text-align: center;
    }

    .home-box h1 {
      margin: 0 0 10px;
      font-size: 30px;
      letter-spacing: 1.5px;
    }

    .home-box p {
      color: var(--sub);
      margin: 0 0 24px;
      line-height: 1.65;
    }

    .home-btns {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      gap: 12px;
    }

    .home-btns button {
      padding: 12px 22px;
      font-size: 15px;
    }

    /* 聊天室共用樣式 */
    .chat-wrap {
      display: flex;
      flex-direction: column;
      height: 100%;
      min-height: 220px;
    }

    .chat-msgs {
      flex: 1;
      overflow-y: auto;
      background: var(--bg);
      border: 1px solid var(--line);
      border-radius: 8px;
      padding: 9px 11px;
      margin-bottom: 9px;
      font-size: 13px;
      line-height: 1.5;
    }

    .chat-msg {
      margin-bottom: 5px;
      word-break: break-word;
    }

    .chat-who {
      color: #7dd3fc;
      font-weight: 600;
      margin-right: 5px;
    }

    .chat-form {
      display: flex;
      gap: 8px;
    }

    .chat-form input {
      flex: 1;
      background: var(--bg);
      border: 1px solid var(--line);
      color: var(--text);
      border-radius: 8px;
      padding: 8px 11px;
      font-size: 13px;
    }

    /* 大廳 */
    .lobby-layout {
      max-width: 1320px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: 1.25fr 1fr;
      gap: 14px;
    }

    @media (max-width: 920px) {
      .lobby-layout {
        grid-template-columns: 1fr;
      }
    }

    .room-card {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
      background: var(--surface-2);
      border: 1px solid var(--line);
      border-radius: 10px;
      padding: 11px 14px;
      margin-bottom: 9px;
    }

    /* 房間（4V4 + 20 觀戰席） */
    .room-layout {
      max-width: 1360px;
      margin: 0 auto;
    }

    .room-teams {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 14px;
      margin-bottom: 14px;
    }

    @media (max-width: 840px) {
      .room-teams {
        grid-template-columns: 1fr;
      }
    }

    .team-box.blue {
      border-color: rgba(59, 130, 246, 0.55);
      background: linear-gradient(180deg, var(--blue-bg), var(--surface) 42%);
    }

    .team-box.red {
      border-color: rgba(239, 68, 68, 0.55);
      background: linear-gradient(180deg, var(--red-bg), var(--surface) 42%);
    }

    .slot-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: rgba(11, 16, 23, 0.72);
      border: 1px solid var(--line);
      border-radius: 8px;
      padding: 8px 11px;
      margin-bottom: 7px;
      font-size: 13px;
    }

    .role-badge {
      color: #fcd34d;
      font-weight: 700;
      margin-right: 8px;
      font-size: 12px;
    }

    .spec-list {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      gap: 7px;
    }

    .spec-slot {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: var(--surface-2);
      border: 1px solid var(--line);
      border-radius: 7px;
      padding: 7px 9px;
      font-size: 12px;
    }

    /* 戰場操控介面 */
    .battle-grid {
      max-width: 1480px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: 270px 1fr 330px;
      gap: 12px;
      align-items: start;
    }

    @media (max-width: 1180px) {
      .battle-grid {
        grid-template-columns: 1fr;
      }
    }

    .squad-card {
      background: var(--surface-2);
      border: 1px solid var(--line);
      border-radius: 9px;
      padding: 9px 10px;
      margin-bottom: 8px;
      cursor: pointer;
    }

    .squad-card.selected {
      border-color: #60a5fa;
      background: rgba(59, 130, 246, 0.14);
    }

    .order-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 6px;
      margin-top: 8px;
    }

    .res-strip {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-bottom: 10px;
    }

    .res-chip {
      background: var(--surface);
      border: 1px solid var(--line);
      border-radius: 8px;
      padding: 6px 11px;
      font-size: 12px;
    }

    canvas#battleMap {
      width: 100%;
      height: 410px;
      display: block;
      background: #0e1621;
      border: 1px solid var(--line);
      border-radius: 12px;
    }

    .battle-chat-box {
      margin-top: 10px;
      height: 195px;
    }

    .sub-tabs {
      display: flex;
      gap: 5px;
      margin-bottom: 10px;
    }

    .sub-tabs button {
      flex: 1;
      padding: 6px 8px;
      font-size: 12px;
    }

    .sub-tabs button.active {
      background: var(--blue);
      border-color: #60a5fa;
      color: #fff;
    }

    .sub-pane {
      display: none;
      max-height: 560px;
      overflow-y: auto;
    }

    .sub-pane.active {
      display: block;
    }

    .ctrl-card {
      background: var(--surface-2);
      border: 1px solid var(--line);
      border-radius: 8px;
      padding: 9px 10px;
      margin-bottom: 8px;
      font-size: 12px;
    }

    .ctrl-top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 6px;
      margin-bottom: 4px;
    }

    #gojoBox {
      display: none;
      margin-top: 8px;
      background: rgba(168, 85, 247, 0.14);
      border: 1px solid rgba(168, 85, 247, 0.6);
      border-radius: 9px;
      padding: 9px 10px;
    }

    /* 隱藏外掛面板（僅在本人輸入 YEE 5 次後默默出現，無閃爍） */
    #yeeCheatPanel {
      display: none;
      position: fixed;
      right: 16px;
      bottom: 16px;
      width: 268px;
      background: rgba(15, 21, 31, 0.97);
      border: 1px solid #475569;
      border-radius: 12px;
      padding: 11px;
      z-index: 950;
      box-shadow: 0 10px 26px rgba(0, 0, 0, 0.45);
    }

    .yee-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 6px;
      margin-top: 8px;
    }

    .yee-grid button {
      font-size: 12px;
      padding: 6px 7px;
    }

    .modal-bg {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.65);
      z-index: 1200;
      align-items: center;
      justify-content: center;
      padding: 16px;
    }

    .modal-bg.open {
      display: flex;
    }

    .modal-box {
      background: var(--surface);
      border: 1px solid var(--line);
      border-radius: 14px;
      padding: 24px;
      max-width: 420px;
      width: 100%;
      text-align: center;
    }
  </style>
</head>
<body>

  <!-- 主畫面 -->
  <section id="view-home" class="view is-active">
    <div class="home-box">
      <h1>三線統帥：4V4 戰略紀元</h1>
      <p>
        4V4 指揮官與三路將軍編制、20 席觀戰、三線對稱據點與 20 秒升旗佔領。<br />
        摧毀敵方本營即可獲勝；每次開新局都會完整重置所有數值與彩蛋狀態。
      </p>
      <div class="home-btns">
        <button id="goLobbyBtn" class="btn-blue">進入大廳與聊天室</button>
        <button id="quickBattleBtn" class="btn-green">快速 4V4 演練開局</button>
      </div>
    </div>
  </section>

  <!-- 作戰大廳 -->
  <section id="view-lobby" class="view">
    <div class="nav-bar">
      <div class="nav-group">
        <button id="backHomeBtn">返回主畫面</button>
        <strong>作戰大廳</strong>
        <span class="tag" id="playerTag">玩家：統帥_01</span>
      </div>
      <div class="nav-group">
        <button id="createRoomBtn" class="btn-blue">+ 建立 4V4 房間</button>
      </div>
    </div>

    <div class="lobby-layout">
      <div class="panel">
        <div class="panel-hd">
          <h2>對戰房間列表（4V4 + 20 觀戰席）</h2>
        </div>
        <div id="roomListBox"></div>
      </div>

      <div class="panel chat-wrap" style="height: 410px;">
        <div class="panel-hd">
          <h3>大廳聊天室</h3>
        </div>
        <div id="lobbyChatMsgs" class="chat-msgs"></div>
        <form id="lobbyChatForm" class="chat-form">
          <input id="lobbyChatInput" type="text" placeholder="輸入訊息..." autocomplete="off" />
          <button type="submit" class="btn-blue">發送</button>
        </form>
      </div>
    </div>
  </section>

  <!-- 4V4 + 20 觀戰席房間 -->
  <section id="view-room" class="view">
    <div class="nav-bar">
      <div class="nav-group">
        <button id="leaveRoomBtn">離開房間</button>
        <strong id="roomNameTitle">三線演習房 #101</strong>
        <span class="tag" id="roomSummaryTag">對戰 0/8・觀戰 0/20</span>
      </div>
      <div class="nav-group">
        <button id="startMatchBtn" class="btn-green">開始對戰</button>
      </div>
    </div>

    <div class="room-layout">
      <div class="room-teams">
        <div class="panel team-box blue">
          <div class="panel-hd">
            <h3>藍方隊伍（BLUE・4 席）</h3>
            <button id="addBlueAiBtn">+ AI</button>
          </div>
          <div id="blueSlotsBox"></div>
        </div>

        <div class="panel team-box red">
          <div class="panel-hd">
            <h3>紅方隊伍（RED・4 席）</h3>
            <button id="addRedAiBtn">+ AI</button>
          </div>
          <div id="redSlotsBox"></div>
        </div>
      </div>

      <div class="panel" style="margin-bottom: 14px;">
        <div class="panel-hd">
          <h3 id="specHeader">觀戰席（0 / 20）</h3>
          <button id="joinSpecBtn">加入觀戰席</button>
        </div>
        <div id="specSlotsBox" class="spec-list"></div>
      </div>

      <div class="panel chat-wrap" style="height: 240px;">
        <div class="panel-hd">
          <h3>房間聊天室</h3>
        </div>
        <div id="roomChatMsgs" class="chat-msgs"></div>
        <form id="roomChatForm" class="chat-form">
          <input id="roomChatInput" type="text" placeholder="輸入訊息..." autocomplete="off" />
          <button type="submit" class="btn-blue">發送</button>
        </form>
      </div>
    </div>
  </section>

  <!-- 三線策略戰場 -->
  <section id="view-battle" class="view">
    <div class="nav-bar">
      <div class="nav-group">
        <button id="exitBattleBtn">撤離並重置本局</button>
        <strong id="battleIdentity">身分：藍方 指揮官</strong>
        <span class="tag" id="battleTimerTag">00:00</span>
      </div>
      <div class="nav-group">
        <span class="tag" id="battleHintTag">選擇分隊與路線，或點選四向指令</span>
      </div>
    </div>

    <div class="battle-grid">
      <!-- 左側：分隊與四向指令 -->
      <div class="panel">
        <div class="panel-hd">
          <h3>分隊與戰線指令</h3>
        </div>
        <div style="margin-bottom: 8px;">
          <span class="muted">目標路線：</span>
          <select id="cmdLaneSelect">
            <option value="top">上路</option>
            <option value="mid" selected>中路</option>
            <option value="bot">下路</option>
          </select>
        </div>
        <div id="squadCardsBox"></div>
        <div class="order-grid">
          <button id="orderAttackBtn" class="btn-red">進攻</button>
          <button id="orderDefendBtn" class="btn-green">防守</button>
          <button id="orderSupportBtn" class="btn-blue">支援（選點）</button>
          <button id="orderRetreatBtn">回家</button>
        </div>

        <div id="gojoBox">
          <div class="ctrl-top">
            <strong>五條悟支援</strong>
            <span class="tag" id="gojoCdLabel">可召喚</span>
          </div>
          <div class="muted" style="margin-bottom: 6px;">於己方本營召喚五條悟，冷卻 5 分鐘。</div>
          <button id="summonGojoBtn" class="btn-blue" style="width: 100%;">於己方本營召喚五條悟</button>
        </div>
      </div>

      <!-- 中央：資源條、三線地圖、戰場聊天室 -->
      <div>
        <div class="res-strip" id="resStripBox"></div>
        <canvas id="battleMap" width="840" height="410"></canvas>

        <div class="panel chat-wrap battle-chat-box">
          <div class="panel-hd">
            <h3>戰場聊天室</h3>
          </div>
          <div id="battleChatMsgs" class="chat-msgs"></div>
          <form id="battleChatForm" class="chat-form">
            <input id="battleChatInput" type="text" placeholder="輸入訊息..." autocomplete="off" />
            <button type="submit" class="btn-blue">發送</button>
          </form>
        </div>
      </div>

      <!-- 右側：徵召與產兵 / 信任轉換 / 研發與道具 -->
      <div class="panel">
        <div class="sub-tabs">
          <button id="subTabProd" class="active">徵召/產兵</button>
          <button id="subTabTrust">信任轉換</button>
          <button id="subTabRd">研發/核彈</button>
        </div>
        <div id="subPaneProd" class="sub-pane active"></div>
        <div id="subPaneTrust" class="sub-pane"></div>
        <div id="subPaneRd" class="sub-pane"></div>
      </div>
    </div>
  </section>

  <!-- 隱藏外掛面板（僅在本人輸入 YEE 5 次後默默出現，無閃爍） -->
  <aside id="yeeCheatPanel">
    <div class="ctrl-top">
      <strong style="font-size: 13px;">作弊控制台</strong>
      <button id="toggleYeeCollapseBtn" style="padding: 2px 7px; font-size: 11px;">收合</button>
    </div>
    <div id="yeeCheatBody" class="yee-grid">
      <button id="yeeAddMoney">+2000 錢</button>
      <button id="yeeAddRes">+50 人力/研</button>
      <button id="yeeAddTrust">+200 信任</button>
      <button id="yeeGiveNuke">+1 核彈</button>
      <button id="yeeCapAll">升旗全滿</button>
      <button id="yeeGodBtn">無敵：關</button>
    </div>
  </aside>

  <!-- 結算視窗 -->
  <div id="resultModal" class="modal-bg">
    <div class="modal-box">
      <h2 id="resultTitle">對局結束</h2>
      <p id="resultBody" class="muted" style="font-size: 14px; margin: 10px 0 20px;"></p>
      <button id="replayBtn" class="btn-blue">再來一局（完整重置）</button>
      <button id="backLobbyFromResultBtn" style="margin-left: 8px;">返回大廳</button>
    </div>
  </div>

  <script>
    (function () {
      const ROLE_NAMES = ["指揮官", "上路將軍", "中路將軍", "下路將軍"];
      const ORDER_LABELS = {
        attack: "進攻",
        defend: "防守",
        support: "支援",
        retreat: "回家"
      };

      const state = {
        view: "home",
        playerName: "統帥_01",
        rooms: [
          {
            id: 101,
            name: "三線演習房 #101",
            blue: [null, null, null, null],
            red: [null, null, null, null],
            specs: Array(20).fill(null)
          }
        ],
        activeRoomId: 101,
        lobbyChat: [{ who: "系統", text: "歡迎來到作戰大廳，可建立或加入 4V4 房間。" }],
        roomChat: [{ who: "系統", text: "隊伍上方可按「+ AI」補位，共有 20 個觀戰席。" }],
        battleChat: [{ who: "系統", text: "戰場已開啟，可在此與隊友通訊。" }],

        // 每一把重置的彩蛋狀態
        yeeCount: 0,
        gojoCount: 0,
        gojoUnlocked: false,
        gojoCooldown: 0,
        yeeUnlocked: false,
        yeeCollapsed: false,
        godMode: false
      };

      let match = null;
      let selectedSquad = 0;
      let mapClickAction = null; // "support" | "nuke" | null
      let prevTime = performance.now();

      function switchView(name) {
        state.view = name;
        document.querySelectorAll(".view").forEach((v) => v.classList.remove("is-active"));
        const el = document.getElementById("view-" + name);
        if (el) el.classList.add("is-active");
        if (name === "lobby") drawLobby();
        if (name === "room") drawRoom();
      }

      // 每一把完整重置所有數值、戰局與彩蛋
      function resetEverythingForNewMatch() {
        match = null;
        selectedSquad = 0;
        mapClickAction = null;

        state.yeeCount = 0;
        state.gojoCount = 0;
        state.gojoUnlocked = false;
        state.gojoCooldown = 0;
        state.yeeUnlocked = false;
        state.yeeCollapsed = false;
        state.godMode = false;

        document.getElementById("yeeCheatPanel").style.display = "none";
        document.getElementById("yeeCheatBody").style.display = "grid";
        document.getElementById("toggleYeeCollapseBtn").textContent = "收合";
        document.getElementById("yeeGodBtn").textContent = "無敵：關";
        document.getElementById("gojoBox").style.display = "none";
        document.getElementById("resultModal").classList.remove("open");
      }

      function createMatchState(team, roleIdx, isSpec) {
        return {
          time: 0,
          over: false,
          team,
          roleIdx,
          isSpec,
          money: 500,
          manpower: 25,
          research: 10,
          trust: 100,
          mpBonusPerMin: 0,
          resBonusPerMin: 0,
          prodLv: 1,
          tech: { infantry: 0, archer: 0, heavy: 0 },
          unlocks: { artillery: false, cavalry: false, medic: false },
          nukeUnlocked: false,
          nukeStock: 0,
          nukeZones: [],
          nodes: [
            { id: "b_base", name: "藍方本營", lane: "mid", owner: "blue", base: true, x: 95, y: 205, hp: 1200, maxHp: 1200, cap: 0, capTeam: null },
            { id: "b_top", name: "藍方上營", lane: "top", owner: "blue", base: false, x: 255, y: 80, hp: 450, maxHp: 450, cap: 0, capTeam: null },
            { id: "n_top", name: "中立上營", lane: "top", owner: "neutral", base: false, x: 420, y: 80, hp: 350, maxHp: 350, cap: 0, capTeam: null },
            { id: "r_top", name: "紅方上營", lane: "top", owner: "red", base: false, x: 585, y: 80, hp: 450, maxHp: 450, cap: 0, capTeam: null },

            { id: "b_mid", name: "藍方中營", lane: "mid", owner: "blue", base: false, x: 265, y: 205, hp: 450, maxHp: 450, cap: 0, capTeam: null },
            { id: "n_mid", name: "中立中營", lane: "mid", owner: "neutral", base: false, x: 420, y: 205, hp: 350, maxHp: 350, cap: 0, capTeam: null },
            { id: "r_mid", name: "紅方中營", lane: "mid", owner: "red", base: false, x: 575, y: 205, hp: 450, maxHp: 450, cap: 0, capTeam: null },

            { id: "b_bot", name: "藍方下營", lane: "bot", owner: "blue", base: false, x: 255, y: 330, hp: 450, maxHp: 450, cap: 0, capTeam: null },
            { id: "n_bot", name: "中立下營", lane: "bot", owner: "neutral", base: false, x: 420, y: 330, hp: 350, maxHp: 350, cap: 0, capTeam: null },
            { id: "r_bot", name: "紅方下營", lane: "bot", owner: "red", base: false, x: 585, y: 330, hp: 450, maxHp: 450, cap: 0, capTeam: null },
            { id: "r_base", name: "紅方本營", lane: "mid", owner: "red", base: true, x: 745, y: 205, hp: 1200, maxHp: 1200, cap: 0, capTeam: null }
          ],
          squads: [
            { id: 0, name: "第一分隊", lane: "top", order: "attack", tx: 420, ty: 80 },
            { id: 1, name: "第二分隊", lane: "mid", order: "attack", tx: 420, ty: 205 },
            { id: 2, name: "第三分隊", lane: "bot", order: "attack", tx: 420, ty: 330 },
            { id: 3, name: "第四分隊", lane: "mid", order: "defend", tx: 265, ty: 205 }
          ],
          units: [],
          spawnTimer: 0,
          aiTimer: 0
        };
      }

      // 聊天室彩蛋過濾（僅本地生效、無閃爍）
      function filterChatWithEggs(raw) {
        let out = raw;
        let yeeHit = 0;
        let gojoHit = 0;

        out = out.replace(/yee/gi, () => {
          yeeHit += 1;
          return "###";
        });

        out = out.replace(/gojo/gi, () => {
          gojoHit += 1;
          return "####";
        });

        if (yeeHit > 0) {
          state.yeeCount += yeeHit;
          if (state.yeeCount >= 5 && !state.yeeUnlocked) {
            state.yeeUnlocked = true;
            document.getElementById("yeeCheatPanel").style.display = "block";
          }
        }

        if (gojoHit > 0) {
          state.gojoCount += gojoHit;
          if (state.gojoCount >= 5 && !state.gojoUnlocked) {
            state.gojoUnlocked = true;
            state.gojoCooldown = 0;
            if (match && !match.over && !match.isSpec) {
              document.getElementById("gojoBox").style.display = "block";
              summonGojo();
            }
          }
        }

        return out;
      }

      function pushChatLine(boxId, who, text) {
        const box = document.getElementById(boxId);
        if (!box) return;
        const row = document.createElement("div");
        row.className = "chat-msg";
        const w = document.createElement("span");
        w.className = "chat-who";
        w.textContent = who + ":";
        row.appendChild(w);
        row.appendChild(document.createTextNode(" " + text));
        box.appendChild(row);
        box.scrollTop = box.scrollHeight;
      }

      function getRoom() {
        return state.rooms.find((r) => r.id === state.activeRoomId) || state.rooms[0];
      }

      function clearSelfFromRoom(room) {
        for (let i = 0; i < 4; i++) {
          if (room.blue[i] && room.blue[i].self) room.blue[i] = null;
          if (room.red[i] && room.red[i].self) room.red[i] = null;
        }
        for (let i = 0; i < room.specs.length; i++) {
          if (room.specs[i] && room.specs[i].self) room.specs[i] = null;
        }
      }

      function autoSeatSelf(room) {
        clearSelfFromRoom(room);
        const bIdx = room.blue.findIndex((x) => x === null);
        if (bIdx !== -1) {
          room.blue[bIdx] = { name: state.playerName, ai: false, self: true };
          return;
        }
        const rIdx = room.red.findIndex((x) => x === null);
        if (rIdx !== -1) {
          room.red[rIdx] = { name: state.playerName, ai: false, self: true };
          return;
        }
        const sIdx = room.specs.findIndex((x) => x === null);
        if (sIdx !== -1) {
          room.specs[sIdx] = { name: state.playerName, ai: false, self: true };
        }
      }

      function drawLobby() {
        document.getElementById("playerTag").textContent = "玩家：" + state.playerName;
        const list = document.getElementById("roomListBox");
        list.innerHTML = "";
        state.rooms.forEach((r) => {
          const b = r.blue.filter(Boolean).length;
          const rd = r.red.filter(Boolean).length;
          const sp = r.specs.filter(Boolean).length;
          const card = document.createElement("div");
          card.className = "room-card";
          card.innerHTML = `
            <div>
              <strong>${r.name}</strong>
              <div class="muted">藍方 ${b}/4・紅方 ${rd}/4・觀戰 ${sp}/20</div>
            </div>
          `;
          const joinBtn = document.createElement("button");
          joinBtn.className = "btn-blue";
          joinBtn.textContent = "加入房間";
          joinBtn.addEventListener("click", () => {
            state.activeRoomId = r.id;
            autoSeatSelf(r);
            switchView("room");
          });
          card.appendChild(joinBtn);
          list.appendChild(card);
        });

        const log = document.getElementById("lobbyChatMsgs");
        log.innerHTML = "";
        state.lobbyChat.forEach((m) => pushChatLine("lobbyChatMsgs", m.who, m.text));
      }

      function drawTeam(boxId, room, team) {
        const box = document.getElementById(boxId);
        box.innerHTML = "";
        const slots = room[team];
        for (let i = 0; i < 4; i++) {
          const occ = slots[i];
          const item = document.createElement("div");
          item.className = "slot-item";

          const left = document.createElement("div");
          left.innerHTML = `<span class="role-badge">${ROLE_NAMES[i]}</span><span>${
            occ ? occ.name + (occ.ai ? " [AI]" : "") : "空位（未佔用由指揮官接管）"
          }</span>`;

          const right = document.createElement("div");
          if (!occ) {
            const btn = document.createElement("button");
            btn.textContent = "入座";
            btn.style.padding = "4px 8px";
            btn.style.fontSize = "12px";
            btn.addEventListener("click", () => {
              clearSelfFromRoom(room);
              slots[i] = { name: state.playerName, ai: false, self: true };
              drawRoom();
            });
            right.appendChild(btn);
          } else if (occ.ai) {
            const del = document.createElement("button");
            del.textContent = "移除";
            del.style.padding = "4px 8px";
            del.style.fontSize = "12px";
            del.addEventListener("click", () => {
              slots[i] = null;
              drawRoom();
            });
            right.appendChild(del);
          }

          item.appendChild(left);
          item.appendChild(right);
          box.appendChild(item);
        }
      }

      function drawRoom() {
        const room = getRoom();
        document.getElementById("roomNameTitle").textContent = room.name;
        const b = room.blue.filter(Boolean).length;
        const rd = room.red.filter(Boolean).length;
        const sp = room.specs.filter(Boolean).length;
        document.getElementById("roomSummaryTag").textContent = `對戰 ${b + rd}/8・觀戰 ${sp}/20`;
        document.getElementById("specHeader").textContent = `觀戰席（${sp} / 20）`;

        drawTeam("blueSlotsBox", room, "blue");
        drawTeam("redSlotsBox", room, "red");

        const specBox = document.getElementById("specSlotsBox");
        specBox.innerHTML = "";
        for (let i = 0; i < 20; i++) {
          const occ = room.specs[i];
          const cell = document.createElement("div");
          cell.className = "spec-slot";
          cell.innerHTML = `<span>${occ ? occ.name : `觀戰 #${i + 1}`}</span>`;
          if (!occ) {
            const bBtn = document.createElement("button");
            bBtn.textContent = "坐下";
            bBtn.style.padding = "3px 7px";
            bBtn.style.fontSize = "11px";
            bBtn.addEventListener("click", () => {
              clearSelfFromRoom(room);
              room.specs[i] = { name: state.playerName, ai: false, self: true };
              drawRoom();
            });
            cell.appendChild(bBtn);
          }
          specBox.appendChild(cell);
        }

        const rLog = document.getElementById("roomChatMsgs");
        rLog.innerHTML = "";
        state.roomChat.forEach((m) => pushChatLine("roomChatMsgs", m.who, m.text));
      }

      function startBattleFromRoom(room) {
        resetEverythingForNewMatch();

        let pTeam = "blue";
        let pRole = 0;
        let isSpec = false;

        for (let i = 0; i < 4; i++) {
          if (room.blue[i] && room.blue[i].self) {
            pTeam = "blue";
            pRole = i;
          }
          if (room.red[i] && room.red[i].self) {
            pTeam = "red";
            pRole = i;
          }
        }
        if (room.specs.some((s) => s && s.self)) {
          isSpec = true;
        }

        match = createMatchState(pTeam, pRole, isSpec);

        ["top", "mid", "bot"].forEach((lane, idx) => {
          createUnit("blue", "infantry", lane, idx);
          createUnit("blue", "archer", lane, idx);
          createUnit("red", "infantry", lane, idx);
          createUnit("red", "archer", lane, idx);
        });

        document.getElementById("battleIdentity").textContent = isSpec
          ? "身分：觀戰席"
          : `身分：${pTeam === "blue" ? "藍方" : "紅方"} ${ROLE_NAMES[pRole]}`;

        const bLog = document.getElementById("battleChatMsgs");
        bLog.innerHTML = "";
        state.battleChat = [{ who: "系統", text: "新對局已開始，所有狀態與彩蛋計數已重置。" }];
        state.battleChat.forEach((m) => pushChatLine("battleChatMsgs", m.who, m.text));

        switchView("battle");
        drawSquadCards();
        drawControlPanes();
      }

      function quickStart4v4() {
        const room = getRoom();
        room.blue = [
          { name: state.playerName, ai: false, self: true },
          { name: "AI_上路將軍", ai: true, self: false },
          { name: "AI_中路將軍", ai: true, self: false },
          { name: "AI_下路將軍", ai: true, self: false }
        ];
        room.red = [
          { name: "AI_敵指揮官", ai: true, self: false },
          { name: "AI_敵上將軍", ai: true, self: false },
          { name: "AI_敵中將軍", ai: true, self: false },
          { name: "AI_敵下將軍", ai: true, self: false }
        ];
        startBattleFromRoom(room);
      }

      function getUnitSpec(type) {
        const iLv = match.tech.infantry;
        const aLv = match.tech.archer;
        const hLv = match.tech.heavy;
        if (type === "infantry") return { hp: 100 + iLv * 25, atk: 10 + iLv * 3, spd: 54 + iLv * 6, rng: 24, r: 7, bounty: 10 };
        if (type === "archer") return { hp: 50 + aLv * 12, atk: 15 + aLv * 5, spd: 48, rng: 90 + aLv * 14, r: 7, bounty: 12 };
        if (type === "heavy") return { hp: 150 + hLv * 45, atk: 5 + hLv * 2, spd: 38, rng: 26, r: 9, bounty: 18 };
        if (type === "artillery") return { hp: 80, atk: 38, spd: 34, rng: 140, r: 9, bounty: 28 };
        if (type === "cavalry") return { hp: 135, atk: 18, spd: 86, rng: 28, r: 8, bounty: 24 };
        if (type === "medic") return { hp: 90, atk: 5, spd: 50, rng: 80, r: 7, bounty: 20 };
        return { hp: 2500, atk: 95, spd: 96, rng: 115, r: 12, bounty: 0, gojo: true };
      }

      function getBaseNode(team) {
        return match.nodes.find((n) => n.base && n.owner === team) ||
          match.nodes.find((n) => n.id === (team === "blue" ? "b_base" : "r_base"));
      }

      function createUnit(team, type, lane, squadId = 0) {
        if (!match) return;
        const base = getBaseNode(team);
        const spec = getUnitSpec(type);
        const sq = match.squads[squadId] || match.squads[0];
        match.units.push({
          team,
          type,
          lane,
          squadId,
          x: base.x + (Math.random() - 0.5) * 18,
          y: base.y + (Math.random() - 0.5) * 24,
          hp: spec.hp,
          maxHp: spec.hp,
          atk: spec.atk,
          spd: spec.spd,
          rng: spec.rng,
          r: spec.r,
          bounty: spec.bounty,
          isGojo: Boolean(spec.gojo),
          cd: 0,
          tx: sq.tx,
          ty: sq.ty
        });
      }

      function summonGojo() {
        if (!match || match.over || match.isSpec) return;
        if (!state.gojoUnlocked || state.gojoCooldown > 0) return;
        const lane = document.getElementById("cmdLaneSelect").value || "mid";
        createUnit(match.team, "gojo", lane, selectedSquad);
        state.gojoCooldown = 300; // 5 分鐘冷卻
        refreshGojoWidget();
      }

      function refreshGojoWidget() {
        const box = document.getElementById("gojoBox");
        if (!state.gojoUnlocked || !match || match.isSpec) {
          box.style.display = "none";
          return;
        }
        box.style.display = "block";
        const btn = document.getElementById("summonGojoBtn");
        const lbl = document.getElementById("gojoCdLabel");
        if (state.gojoCooldown > 0) {
          const m = String(Math.floor(state.gojoCooldown / 60)).padStart(2, "0");
          const s = String(Math.floor(state.gojoCooldown % 60)).padStart(2, "0");
          lbl.textContent = `冷卻 ${m}:${s}`;
          btn.disabled = true;
        } else {
          lbl.textContent = "可召喚";
          btn.disabled = false;
        }
      }

      function drawSquadCards() {
        if (!match) return;
        const box = document.getElementById("squadCardsBox");
        box.innerHTML = "";
        match.squads.forEach((sq) => {
          const div = document.createElement("div");
          div.className = "squad-card" + (sq.id === selectedSquad ? " selected" : "");
          const laneText = sq.lane === "top" ? "上路" : sq.lane === "bot" ? "下路" : "中路";
          div.innerHTML = `
            <div class="ctrl-top">
              <strong>${sq.name}</strong>
              <span class="tag">${ORDER_LABELS[sq.order]}・${laneText}</span>
            </div>
          `;
          div.addEventListener("click", () => {
            selectedSquad = sq.id;
            document.getElementById("cmdLaneSelect").value = sq.lane;
            drawSquadCards();
          });
          box.appendChild(div);
        });
      }

      function applyOrder(order, pt = null) {
        if (!match || match.over || match.isSpec) return;
        const sq = match.squads[selectedSquad];
        const lane = document.getElementById("cmdLaneSelect").value || "mid";
        sq.lane = lane;
        sq.order = order;

        const home = getBaseNode(match.team);
        const neutralLane = match.nodes.find((n) => n.lane === lane && n.owner === "neutral");
        const enemyLane = match.nodes.find((n) => n.lane === lane && n.owner !== match.team && !n.base) ||
          getBaseNode(match.team === "blue" ? "red" : "blue");
        const ownLane = match.nodes.find((n) => n.lane === lane && n.owner === match.team && !n.base) || home;

        if (order === "attack") {
          sq.tx = enemyLane.x;
          sq.ty = enemyLane.y;
        } else if (order === "defend") {
          sq.tx = ownLane.x;
          sq.ty = ownLane.y;
        } else if (order === "retreat") {
          sq.tx = home.x;
          sq.ty = home.y;
        } else if (order === "support") {
          if (pt) {
            sq.tx = pt.x;
            sq.ty = pt.y;
          } else if (neutralLane) {
            sq.tx = neutralLane.x;
            sq.ty = neutralLane.y;
          }
        }

        match.units.forEach((u) => {
          if (u.team === match.team && u.squadId === selectedSquad) {
            u.lane = lane;
            u.tx = sq.tx;
            u.ty = sq.ty;
          }
        });
        drawSquadCards();
      }

      function drawControlPanes() {
        if (!match) return;
        const pProd = document.getElementById("subPaneProd");
        const pTrust = document.getElementById("subPaneTrust");
        const pRd = document.getElementById("subPaneRd");

        const prodMoney = 350 * match.prodLv;
        const prodMp = 12 * match.prodLv;

        const unitCatalog = [
          { k: "infantry", name: "步兵", m: 60, mp: 2, ok: true },
          { k: "archer", name: "射手", m: 80, mp: 2, ok: true },
          { k: "heavy", name: "重裝士兵", m: 110, mp: 3, ok: true },
          { k: "artillery", name: "攻城砲兵", m: 220, mp: 5, ok: match.unlocks.artillery },
          { k: "cavalry", name: "衝鋒騎兵", m: 180, mp: 4, ok: match.unlocks.cavalry },
          { k: "medic", name: "戰地醫護兵", m: 150, mp: 3, ok: match.unlocks.medic }
        ];

        pProd.innerHTML = `
          <div class="ctrl-card">
            <div class="ctrl-top">
              <strong>產兵效率 Lv.${match.prodLv}</strong>
              <button id="upgradeProdBtn" ${match.isSpec ? "disabled" : ""}>升級（${prodMoney}錢/${prodMp}人）</button>
            </div>
            <div class="muted">每分鐘基準產兵：各 ${2 * match.prodLv} 隻（小營 50% 減半）</div>
          </div>
        `;

        unitCatalog.forEach((item) => {
          const c = document.createElement("div");
          c.className = "ctrl-card";
          c.innerHTML = `
            <div class="ctrl-top">
              <strong>${item.name}</strong>
              <span class="muted">${item.ok ? `${item.m}錢 / ${item.mp}人` : "未解鎖"}</span>
            </div>
          `;
          const btn = document.createElement("button");
          btn.textContent = "徵召至本營";
          btn.disabled = match.isSpec || !item.ok;
          btn.addEventListener("click", () => {
            if (match.money >= item.m && match.manpower >= item.mp) {
              match.money -= item.m;
              match.manpower -= item.mp;
              const lane = document.getElementById("cmdLaneSelect").value || "mid";
              createUnit(match.team, item.k, lane, selectedSquad);
            }
          });
          c.appendChild(btn);
          pProd.appendChild(c);
        });

        const upg = document.getElementById("upgradeProdBtn");
        if (upg) {
          upg.addEventListener("click", () => {
            if (match.money >= prodMoney && match.manpower >= prodMp) {
              match.money -= prodMoney;
              match.manpower -= prodMp;
              match.prodLv += 1;
              drawControlPanes();
            }
          });
        }

        pTrust.innerHTML = `
          <div class="ctrl-card">
            <strong>信任點轉換</strong>
            <div class="muted">初始 100，每分鐘 +30、擊殺敵兵 +1。10 點轉 +1 人力/分；15 點轉 +1 研究/分。</div>
          </div>
          <div class="ctrl-card">
            <div class="ctrl-top">
              <strong>每分鐘 +1 人力</strong>
              <button id="toMpBtn" ${match.isSpec ? "disabled" : ""}>轉換（10 信任）</button>
            </div>
            <div class="muted">已轉換：+${match.mpBonusPerMin} / 分</div>
          </div>
          <div class="ctrl-card">
            <div class="ctrl-top">
              <strong>每分鐘 +1 研究點</strong>
              <button id="toResBtn" ${match.isSpec ? "disabled" : ""}>轉換（15 信任）</button>
            </div>
            <div class="muted">已轉換：+${match.resBonusPerMin} / 分</div>
          </div>
        `;

        document.getElementById("toMpBtn").addEventListener("click", () => {
          if (match.trust >= 10) {
            match.trust -= 10;
            match.mpBonusPerMin += 1;
            drawControlPanes();
          }
        });
        document.getElementById("toResBtn").addEventListener("click", () => {
          if (match.trust >= 15) {
            match.trust -= 15;
            match.resBonusPerMin += 1;
            drawControlPanes();
          }
        });

        pRd.innerHTML = "";
        [
          ["infantry", "步兵強化", match.tech.infantry],
          ["archer", "射手強化", match.tech.archer],
          ["heavy", "重裝士兵強化", match.tech.heavy]
        ].forEach(([k, label, lv]) => {
          const next = lv + 1;
          const mCost = 250 * next;
          const mpCost = 8 * next;
          const rCost = 10 * next;
          const row = document.createElement("div");
          row.className = "ctrl-card";
          row.innerHTML = `
            <div class="ctrl-top">
              <strong>${label}（Lv.${lv}/3）</strong>
              <span class="muted">${lv >= 3 ? "滿級" : `${mCost}錢/${mpCost}人/${rCost}研`}</span>
            </div>
          `;
          if (lv < 3) {
            const b = document.createElement("button");
            b.textContent = "研發";
            b.disabled = match.isSpec;
            b.addEventListener("click", () => {
              if (match.money >= mCost && match.manpower >= mpCost && match.research >= rCost) {
                match.money -= mCost;
                match.manpower -= mpCost;
                match.research -= rCost;
                match.tech[k] += 1;
                drawControlPanes();
              }
            });
            row.appendChild(b);
          }
          pRd.appendChild(row);
        });

        [
          ["artillery", "解鎖攻城砲兵", match.unlocks.artillery],
          ["cavalry", "解鎖衝鋒騎兵", match.unlocks.cavalry],
          ["medic", "解鎖戰地醫護兵", match.unlocks.medic]
        ].forEach(([k, label, ok]) => {
          const row = document.createElement("div");
          row.className = "ctrl-card";
          row.innerHTML = `
            <div class="ctrl-top">
              <strong>${label}</strong>
              <span class="muted">${ok ? "已解鎖" : "500錢/15人/15研"}</span>
            </div>
          `;
          if (!ok) {
            const b = document.createElement("button");
            b.textContent = "研發解鎖";
            b.disabled = match.isSpec;
            b.addEventListener("click", () => {
              if (match.money >= 500 && match.manpower >= 15 && match.research >= 15) {
                match.money -= 500;
                match.manpower -= 15;
                match.research -= 15;
                match.unlocks[k] = true;
                drawControlPanes();
              }
            });
            row.appendChild(b);
          }
          pRd.appendChild(row);
        });

        // 戰術核彈
        const nukeCard = document.createElement("div");
        nukeCard.className = "ctrl-card";
        nukeCard.innerHTML = `
          <div class="ctrl-top">
            <strong>戰術核彈</strong>
            <span class="muted">庫存：${match.nukeStock}</span>
          </div>
          <div class="muted" style="margin-bottom: 6px;">解鎖：1000錢/30人/30研・裝填：800錢/20研</div>
        `;
        const nRow = document.createElement("div");
        nRow.style.display = "flex";
        nRow.style.gap = "6px";

        if (!match.nukeUnlocked) {
          const uBtn = document.createElement("button");
          uBtn.textContent = "解鎖核彈";
          uBtn.disabled = match.isSpec;
          uBtn.addEventListener("click", () => {
            if (match.money >= 1000 && match.manpower >= 30 && match.research >= 30) {
              match.money -= 1000;
              match.manpower -= 30;
              match.research -= 30;
              match.nukeUnlocked = true;
              drawControlPanes();
            }
          });
          nRow.appendChild(uBtn);
        } else {
          const bBtn = document.createElement("button");
          bBtn.textContent = "裝填核彈";
          bBtn.disabled = match.isSpec;
          bBtn.addEventListener("click", () => {
            if (match.money >= 800 && match.research >= 20) {
              match.money -= 800;
              match.research -= 20;
              match.nukeStock += 1;
              drawControlPanes();
            }
          });

          const fBtn = document.createElement("button");
          fBtn.className = "btn-red";
          fBtn.textContent = "投擲核彈";
          fBtn.disabled = match.isSpec || match.nukeStock <= 0;
          fBtn.addEventListener("click", () => {
            mapClickAction = "nuke";
            document.getElementById("battleHintTag").textContent = "請在地圖點選戰術核彈落點";
          });

          nRow.appendChild(bBtn);
          nRow.appendChild(fBtn);
        }
        nukeCard.appendChild(nRow);
        pRd.appendChild(nukeCard);
      }

      function triggerNuke(x, y) {
        if (!match || match.nukeStock <= 0) return;
        match.nukeStock -= 1;
        const r = 105;
        match.units.forEach((u) => {
          if (u.team !== match.team && Math.hypot(u.x - x, u.y - y) <= r) {
            u.hp -= 450;
          }
        });
        match.nodes.forEach((n) => {
          if (n.owner !== match.team && Math.hypot(n.x - x, n.y - y) <= r) {
            n.hp = Math.max(n.base ? 150 : 0, n.hp - 320);
            n.cap = 0;
            n.capTeam = null;
          }
        });
        match.nukeZones.push({ x, y, r, ttl: 6 });
        drawControlPanes();
      }

      function stepBattle(dt) {
        if (!match || match.over) return;
        match.time += dt;

        if (state.gojoCooldown > 0) {
          state.gojoCooldown = Math.max(0, state.gojoCooldown - dt);
          refreshGojoWidget();
        }

        let incomePerMin = 0;
        match.nodes.forEach((n) => {
          if (n.base && n.owner === match.team) incomePerMin += 120;
          else if (!n.base && n.owner === match.team) incomePerMin += 60;
          else if (!n.base && n.owner === "neutral") incomePerMin += 90;
        });

        match.money += (incomePerMin / 60) * dt;
        match.trust += (30 / 60) * dt;
        match.manpower += (match.mpBonusPerMin / 60) * dt;
        match.research += (match.resBonusPerMin / 60) * dt;

        match.spawnTimer += dt;
        const interval = 60 / (2 * match.prodLv);
        while (match.spawnTimer >= interval) {
          match.spawnTimer -= interval;
          const lanes = ["top", "mid", "bot"];
          const lane = lanes[Math.floor(Math.random() * lanes.length)];
          ["infantry", "archer", "heavy"].forEach((t) => {
            createUnit("blue", t, lane, Math.floor(Math.random() * 4));
            createUnit("red", t, lane, Math.floor(Math.random() * 4));
          });
        }

        match.aiTimer += dt;
        if (match.aiTimer >= 4) {
          match.aiTimer = 0;
          const enemyTeam = match.team === "blue" ? "red" : "blue";
          const targetBase = getBaseNode(match.team);
          match.units.forEach((u) => {
            if (u.team === enemyTeam) {
              u.tx = targetBase.x;
              u.ty = targetBase.y;
            }
          });
        }

        for (let i = match.units.length - 1; i >= 0; i--) {
          const u = match.units[i];
          if (u.hp <= 0) {
            if (u.team !== match.team && !match.isSpec) {
              match.trust += 1;
              match.money += u.bounty || 10;
            }
            match.units.splice(i, 1);
            continue;
          }

          if (u.cd > 0) u.cd -= dt;

          let enemy = null;
          let eDist = Infinity;
          for (let j = 0; j < match.units.length; j++) {
            const o = match.units[j];
            if (o.team !== u.team && o.hp > 0) {
              const d = Math.hypot(o.x - u.x, o.y - u.y);
              if (d < eDist) {
                eDist = d;
                enemy = o;
              }
            }
          }

          let node = null;
          let nDist = Infinity;
          for (let k = 0; k < match.nodes.length; k++) {
            const nd = match.nodes[k];
            if (nd.owner !== u.team && nd.hp > 0) {
              const d = Math.hypot(nd.x - u.x, nd.y - u.y);
              if (d < nDist) {
                nDist = d;
                node = nd;
              }
            }
          }

          if (enemy && eDist <= u.rng + 40) {
            if (eDist > u.rng) {
              const a = Math.atan2(enemy.y - u.y, enemy.x - u.x);
              u.x += Math.cos(a) * u.spd * dt;
              u.y += Math.sin(a) * u.spd * dt;
            } else if (u.cd <= 0) {
              if (!(state.godMode && enemy.team === match.team)) {
                enemy.hp -= u.atk;
              }
              u.cd = 1.0;
            }
          } else if (node && nDist <= u.rng + 36) {
            if (nDist > u.rng + 14) {
              const a = Math.atan2(node.y - u.y, node.x - u.x);
              u.x += Math.cos(a) * u.spd * dt;
              u.y += Math.sin(a) * u.spd * dt;
            } else if (u.cd <= 0) {
              node.hp -= u.atk * 0.8;
              if (node.hp <= 0 && !node.base) {
                node.hp = 350;
                node.owner = "neutral";
                node.cap = 0;
                node.capTeam = null;
              }
              u.cd = 1.1;
            }
          } else {
            const dx = u.tx - u.x;
            const dy = u.ty - u.y;
            const dist = Math.hypot(dx, dy);
            if (dist > 5) {
              u.x += (dx / dist) * u.spd * dt;
              u.y += (dy / dist) * u.spd * dt;
            }
          }
        }

        // 20 秒升旗佔領（圈內每多 5 人加速 10%）
        match.nodes.forEach((nd) => {
          if (nd.base) return;
          const bCount = match.units.filter((u) => u.team === "blue" && Math.hypot(u.x - nd.x, u.y - nd.y) <= 40).length;
          const rCount = match.units.filter((u) => u.team === "red" && Math.hypot(u.x - nd.x, u.y - nd.y) <= 40).length;

          if (bCount > 0 && rCount === 0 && nd.owner !== "blue") {
            const mult = 1 + Math.floor(Math.max(0, bCount - 1) / 5) * 0.1;
            nd.capTeam = "blue";
            nd.cap += (dt / 20) * mult;
            if (nd.cap >= 1) {
              nd.owner = "blue";
              nd.hp = 450;
              nd.cap = 0;
              nd.capTeam = null;
            }
          } else if (rCount > 0 && bCount === 0 && nd.owner !== "red") {
            const mult = 1 + Math.floor(Math.max(0, rCount - 1) / 5) * 0.1;
            nd.capTeam = "red";
            nd.cap += (dt / 20) * mult;
            if (nd.cap >= 1) {
              nd.owner = "red";
              nd.hp = 450;
              nd.cap = 0;
              nd.capTeam = null;
            }
          } else if (bCount === 0 && rCount === 0 && nd.cap > 0) {
            nd.cap = Math.max(0, nd.cap - dt * 0.05);
          }
        });

        for (let i = match.nukeZones.length - 1; i >= 0; i--) {
          match.nukeZones[i].ttl -= dt;
          if (match.nukeZones[i].ttl <= 0) match.nukeZones.splice(i, 1);
        }

        const bBase = match.nodes.find((n) => n.id === "b_base");
        const rBase = match.nodes.find((n) => n.id === "r_base");
        if (rBase.hp <= 0 || bBase.hp <= 0) {
          match.over = true;
          const winner = rBase.hp <= 0 ? "藍方" : "紅方";
          document.getElementById("resultTitle").textContent = `${winner}摧毀敵營・獲勝`;
          document.getElementById("resultBody").textContent =
            `對局時長 ${Math.floor(match.time)} 秒。下一局將完整重置所有數值與彩蛋。`;
          document.getElementById("resultModal").classList.add("open");
        }

        const mm = String(Math.floor(match.time / 60)).padStart(2, "0");
        const ss = String(Math.floor(match.time % 60)).padStart(2, "0");
        document.getElementById("battleTimerTag").textContent = `${mm}:${ss}`;
        document.getElementById("resStripBox").innerHTML = `
          <div class="res-chip">錢：<strong>${Math.floor(match.money)}</strong>（+${incomePerMin}/分）</div>
          <div class="res-chip">人力：<strong>${Math.floor(match.manpower)}</strong>（+${match.mpBonusPerMin}/分）</div>
          <div class="res-chip">研究點：<strong>${Math.floor(match.research)}</strong>（+${match.resBonusPerMin}/分）</div>
          <div class="res-chip">信任點：<strong>${Math.floor(match.trust)}</strong></div>
        `;
      }

      function drawBattleMap() {
        const canvas = document.getElementById("battleMap");
        if (!canvas || !match) return;
        const ctx = canvas.getContext("2d");
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        ctx.strokeStyle = "#293950";
        ctx.lineWidth = 4;
        [
          [95, 205, 255, 80, 420, 80, 585, 80, 745, 205],
          [95, 205, 265, 205, 420, 205, 575, 205, 745, 205],
          [95, 205, 255, 330, 420, 330, 585, 330, 745, 205]
        ].forEach((p) => {
          ctx.beginPath();
          ctx.moveTo(p[0], p[1]);
          ctx.lineTo(p[2], p[3]);
          ctx.lineTo(p[4], p[5]);
          ctx.lineTo(p[6], p[7]);
          ctx.lineTo(p[8], p[9]);
          ctx.stroke();
        });

        match.nukeZones.forEach((z) => {
          ctx.fillStyle = "rgba(239, 68, 68, 0.14)";
          ctx.beginPath();
          ctx.arc(z.x, z.y, z.r, 0, Math.PI * 2);
          ctx.fill();
        });

        match.nodes.forEach((nd) => {
          const col = nd.owner === "blue" ? "#3b82f6" : nd.owner === "red" ? "#ef4444" : "#94a3b8";
          const rad = nd.base ? 22 : 14;

          ctx.strokeStyle = "rgba(148, 163, 184, 0.24)";
          ctx.lineWidth = 2;
          ctx.beginPath();
          ctx.arc(nd.x, nd.y, 40, 0, Math.PI * 2);
          ctx.stroke();

          ctx.fillStyle = col;
          ctx.beginPath();
          ctx.arc(nd.x, nd.y, rad, 0, Math.PI * 2);
          ctx.fill();

          if (nd.cap > 0 && nd.capTeam) {
            ctx.strokeStyle = nd.capTeam === "blue" ? "#93c5fd" : "#fca5a5";
            ctx.lineWidth = 4;
            ctx.beginPath();
            ctx.arc(nd.x, nd.y, rad + 6, -Math.PI / 2, -Math.PI / 2 + Math.PI * 2 * nd.cap);
            ctx.stroke();
          }

          ctx.fillStyle = "#e8eff8";
          ctx.font = "12px sans-serif";
          ctx.textAlign = "center";
          ctx.fillText(`${nd.name} (${Math.max(0, Math.floor(nd.hp))})`, nd.x, nd.y - rad - 7);
        });

        match.units.forEach((u) => {
          ctx.fillStyle = u.isGojo ? "#c084fc" : u.team === "blue" ? "#60a5fa" : "#f87171";
          ctx.beginPath();
          ctx.arc(u.x, u.y, u.r, 0, Math.PI * 2);
          ctx.fill();
        });
      }

      function loop(now) {
        const dt = Math.min(0.1, (now - prevTime) / 1000);
        prevTime = now;
        if (state.view === "battle" && match) {
          stepBattle(dt);
          drawBattleMap();
        }
        requestAnimationFrame(loop);
      }

      // 導覽與房間事件
      document.getElementById("goLobbyBtn").addEventListener("click", () => switchView("lobby"));
      document.getElementById("quickBattleBtn").addEventListener("click", quickStart4v4);
      document.getElementById("backHomeBtn").addEventListener("click", () => switchView("home"));

      document.getElementById("createRoomBtn").addEventListener("click", () => {
        const id = state.rooms.length + 101;
        const room = {
          id,
          name: `三線戰場 #${id}`,
          blue: [null, null, null, null],
          red: [null, null, null, null],
          specs: Array(20).fill(null)
        };
        autoSeatSelf(room);
        state.rooms.push(room);
        state.activeRoomId = id;
        switchView("room");
      });

      document.getElementById("leaveRoomBtn").addEventListener("click", () => {
        clearSelfFromRoom(getRoom());
        switchView("lobby");
      });

      document.getElementById("addBlueAiBtn").addEventListener("click", () => {
        const room = getRoom();
        const idx = room.blue.findIndex((x) => x === null);
        if (idx !== -1) {
          room.blue[idx] = { name: "AI_" + ROLE_NAMES[idx], ai: true, self: false };
          drawRoom();
        }
      });

      document.getElementById("addRedAiBtn").addEventListener("click", () => {
        const room = getRoom();
        const idx = room.red.findIndex((x) => x === null);
        if (idx !== -1) {
          room.red[idx] = { name: "AI_" + ROLE_NAMES[idx], ai: true, self: false };
          drawRoom();
        }
      });

      document.getElementById("joinSpecBtn").addEventListener("click", () => {
        const room = getRoom();
        const idx = room.specs.findIndex((x) => x === null);
        if (idx !== -1) {
          clearSelfFromRoom(room);
          room.specs[idx] = { name: state.playerName, ai: false, self: true };
          drawRoom();
        }
      });

      document.getElementById("startMatchBtn").addEventListener("click", () => {
        startBattleFromRoom(getRoom());
      });

      // 三處聊天室表單
      function bindChatForm(formId, inputId, storeArr, boxId) {
        document.getElementById(formId).addEventListener("submit", (e) => {
          e.preventDefault();
          const inp = document.getElementById(inputId);
          const raw = inp.value.trim();
          if (!raw) return;
          const clean = filterChatWithEggs(raw);
          storeArr.push({ who: state.playerName, text: clean });
          pushChatLine(boxId, state.playerName, clean);
          inp.value = "";
        });
      }

      bindChatForm("lobbyChatForm", "lobbyChatInput", state.lobbyChat, "lobbyChatMsgs");
      bindChatForm("roomChatForm", "roomChatInput", state.roomChat, "roomChatMsgs");
      bindChatForm("battleChatForm", "battleChatInput", state.battleChat, "battleChatMsgs");

      // 戰場指令與地圖點擊
      document.getElementById("exitBattleBtn").addEventListener("click", () => {
        resetEverythingForNewMatch();
        switchView("room");
      });

      document.getElementById("orderAttackBtn").addEventListener("click", () => applyOrder("attack"));
      document.getElementById("orderDefendBtn").addEventListener("click", () => applyOrder("defend"));
      document.getElementById("orderSupportBtn").addEventListener("click", () => {
        mapClickAction = "support";
        document.getElementById("battleHintTag").textContent = "請在地圖點選支援目標位置";
      });
      document.getElementById("orderRetreatBtn").addEventListener("click", () => applyOrder("retreat"));

      document.getElementById("battleMap").addEventListener("click", (e) => {
        if (!match || match.over || !mapClickAction) return;
        const rect = e.target.getBoundingClientRect();
        const x = ((e.clientX - rect.left) / rect.width) * e.target.width;
        const y = ((e.clientY - rect.top) / rect.height) * e.target.height;

        if (mapClickAction === "support") {
          applyOrder("support", { x, y });
        } else if (mapClickAction === "nuke") {
          triggerNuke(x, y);
        }
        mapClickAction = null;
        document.getElementById("battleHintTag").textContent = "選擇分隊與路線，或點選四向指令";
      });

      document.getElementById("summonGojoBtn").addEventListener("click", summonGojo);

      // 右側分頁
      [
        ["subTabProd", "subPaneProd"],
        ["subTabTrust", "subPaneTrust"],
        ["subTabRd", "subPaneRd"]
      ].forEach(([bId, pId]) => {
        document.getElementById(bId).addEventListener("click", () => {
          ["subTabProd", "subTabTrust", "subTabRd"].forEach((x) => document.getElementById(x).classList.remove("active"));
          ["subPaneProd", "subPaneTrust", "subPaneRd"].forEach((x) => document.getElementById(x).classList.remove("active"));
          document.getElementById(bId).classList.add("active");
          document.getElementById(pId).classList.add("active");
        });
      });

      // YEE 外掛面板操作
      document.getElementById("toggleYeeCollapseBtn").addEventListener("click", () => {
        state.yeeCollapsed = !state.yeeCollapsed;
        document.getElementById("yeeCheatBody").style.display = state.yeeCollapsed ? "none" : "grid";
        document.getElementById("toggleYeeCollapseBtn").textContent = state.yeeCollapsed ? "展開" : "收合";
      });

      document.getElementById("yeeAddMoney").addEventListener("click", () => {
        if (match) match.money += 2000;
      });
      document.getElementById("yeeAddRes").addEventListener("click", () => {
        if (match) {
          match.manpower += 50;
          match.research += 50;
          drawControlPanes();
        }
      });
      document.getElementById("yeeAddTrust").addEventListener("click", () => {
        if (match) {
          match.trust += 200;
          drawControlPanes();
        }
      });
      document.getElementById("yeeGiveNuke").addEventListener("click", () => {
        if (match) {
          match.nukeUnlocked = true;
          match.nukeStock += 1;
          drawControlPanes();
        }
      });
      document.getElementById("yeeCapAll").addEventListener("click", () => {
        if (match && !match.isSpec) {
          match.nodes.forEach((nd) => {
            if (!nd.base && nd.owner !== match.team) {
              nd.owner = match.team;
              nd.hp = 450;
              nd.cap = 0;
              nd.capTeam = null;
            }
          });
        }
      });
      document.getElementById("yeeGodBtn").addEventListener("click", () => {
        state.godMode = !state.godMode;
        document.getElementById("yeeGodBtn").textContent = state.godMode ? "無敵：開" : "無敵：關";
      });

      document.getElementById("replayBtn").addEventListener("click", () => {
        startBattleFromRoom(getRoom());
      });
      document.getElementById("backLobbyFromResultBtn").addEventListener("click", () => {
        resetEverythingForNewMatch();
        switchView("lobby");
      });

      requestAnimationFrame(loop);
    })();
  </script>
</body>
</html>
