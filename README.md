<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>三線統帥：4V4 戰略紀元</title>
  <style>
    :root {
      --bg: #0f141c;
      --panel: #171f2b;
      --panel-2: #1e2838;
      --border: #2c3a4f;
      --text: #e6edf5;
      --muted: #93a4bc;
      --blue: #3b82f6;
      --blue-soft: rgba(59, 130, 246, 0.16);
      --red: #ef4444;
      --red-soft: rgba(239, 68, 68, 0.16);
      --gold: #eab308;
      --green: #22c55e;
      --purple: #a855f7;
    }

    * {
      box-sizing: border-box;
      user-select: none;
    }

    body {
      margin: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Noto Sans TC", sans-serif;
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      overflow-x: hidden;
    }

    button, input, select {
      font-family: inherit;
    }

    button {
      cursor: pointer;
      border: 1px solid var(--border);
      background: var(--panel-2);
      color: var(--text);
      border-radius: 8px;
      padding: 8px 14px;
      font-size: 14px;
      transition: background 0.18s ease, border-color 0.18s ease, opacity 0.18s ease;
    }

    button:hover:not(:disabled) {
      background: #28364b;
      border-color: #455b7a;
    }

    button:disabled {
      opacity: 0.45;
      cursor: not-allowed;
    }

    button.primary {
      background: var(--blue);
      border-color: #60a5fa;
      color: #fff;
      font-weight: 600;
    }

    button.primary:hover:not(:disabled) {
      background: #2563eb;
    }

    button.danger {
      background: rgba(239, 68, 68, 0.2);
      border-color: rgba(239, 68, 68, 0.55);
      color: #fca5a5;
    }

    button.success {
      background: rgba(34, 197, 94, 0.2);
      border-color: rgba(34, 197, 94, 0.55);
      color: #86efac;
    }

    .screen {
      display: none;
      min-height: 100vh;
      padding: 20px;
    }

    .screen.active {
      display: block;
    }

    /* 主畫面 */
    .title-wrap {
      max-width: 780px;
      margin: 48px auto;
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 36px;
      text-align: center;
    }

    .title-wrap h1 {
      margin: 0 0 10px;
      font-size: 32px;
      letter-spacing: 2px;
    }

    .title-wrap p {
      color: var(--muted);
      margin: 0 0 28px;
      line-height: 1.6;
    }

    .menu-actions {
      display: flex;
      gap: 14px;
      justify-content: center;
      flex-wrap: wrap;
    }

    .menu-actions button {
      padding: 13px 24px;
      font-size: 16px;
    }

    /* 共用頂部列 */
    .topbar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      flex-wrap: wrap;
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 12px 18px;
      margin-bottom: 16px;
    }

    .topbar-left, .topbar-right {
      display: flex;
      align-items: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    .badge {
      background: var(--panel-2);
      border: 1px solid var(--border);
      border-radius: 999px;
      padding: 4px 12px;
      font-size: 13px;
      color: var(--muted);
    }

    /* 大廳與聊天室 */
    .lobby-grid {
      display: grid;
      grid-template-columns: 1.25fr 1fr;
      gap: 16px;
      max-width: 1280px;
      margin: 0 auto;
    }

    @media (max-width: 900px) {
      .lobby-grid {
        grid-template-columns: 1fr;
      }
    }

    .card {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 16px;
    }

    .card h2, .card h3 {
      margin: 0 0 12px;
      font-size: 17px;
    }

    .room-list {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .room-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      background: var(--panel-2);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 12px 14px;
    }

    .chat-box {
      display: flex;
      flex-direction: column;
      height: 360px;
    }

    .chat-log {
      flex: 1;
      overflow-y: auto;
      background: var(--bg);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 10px;
      margin-bottom: 10px;
      font-size: 13px;
      line-height: 1.5;
    }

    .chat-line {
      margin-bottom: 6px;
      word-break: break-word;
    }

    .chat-sender {
      color: #93c5fd;
      font-weight: 600;
      margin-right: 6px;
    }

    .chat-form {
      display: flex;
      gap: 8px;
    }

    .chat-form input {
      flex: 1;
      background: var(--bg);
      border: 1px solid var(--border);
      color: var(--text);
      border-radius: 8px;
      padding: 9px 12px;
    }

    /* 4V4 + 20 觀戰席房間 */
    .room-wrap {
      max-width: 1320px;
      margin: 0 auto;
    }

    .teams-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
      margin-bottom: 16px;
    }

    @media (max-width: 820px) {
      .teams-grid {
        grid-template-columns: 1fr;
      }
    }

    .team-card.blue {
      border-color: rgba(59, 130, 246, 0.5);
      background: linear-gradient(180deg, var(--blue-soft), var(--panel) 45%);
    }

    .team-card.red {
      border-color: rgba(239, 68, 68, 0.5);
      background: linear-gradient(180deg, var(--red-soft), var(--panel) 45%);
    }

    .team-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
      gap: 8px;
    }

    .slots {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .slot {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: rgba(15, 20, 28, 0.72);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 9px 12px;
      font-size: 14px;
    }

    .slot-role {
      font-size: 12px;
      color: var(--gold);
      font-weight: 600;
      margin-right: 8px;
    }

    .spec-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(145px, 1fr));
      gap: 8px;
      margin-top: 10px;
    }

    .spec-cell {
      background: var(--panel-2);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 8px 10px;
      font-size: 12px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    /* 戰場畫面 */
    .battle-layout {
      max-width: 1440px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: 1fr 340px;
      gap: 14px;
    }

    @media (max-width: 1080px) {
      .battle-layout {
        grid-template-columns: 1fr;
      }
    }

    .res-bar {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-bottom: 12px;
    }

    .res-pill {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 7px 12px;
      font-size: 13px;
    }

    canvas#mapCanvas {
      width: 100%;
      height: 460px;
      display: block;
      background: #121a24;
      border: 1px solid var(--border);
      border-radius: 12px;
    }

    .cmd-bar {
      margin-top: 12px;
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 12px;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
    }

    .cmd-group {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      align-items: center;
    }

    .side-tabs {
      display: flex;
      gap: 6px;
      margin-bottom: 10px;
    }

    .side-tabs button {
      flex: 1;
      padding: 7px 8px;
      font-size: 13px;
    }

    .side-tabs button.active-tab {
      background: var(--blue);
      border-color: #60a5fa;
      color: #fff;
    }

    .tab-panel {
      display: none;
      max-height: 540px;
      overflow-y: auto;
    }

    .tab-panel.active-panel {
      display: block;
    }

    .list-row {
      background: var(--panel-2);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 9px 10px;
      margin-bottom: 8px;
      font-size: 13px;
    }

    .list-row-top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 8px;
      margin-bottom: 4px;
    }

    .muted {
      color: var(--muted);
      font-size: 12px;
    }

    /* 五條悟戰場召喚卡（僅在 Gojo 5 次解鎖後顯示） */
    #gojoSummonBox {
      display: none;
      margin-top: 10px;
      background: rgba(168, 85, 247, 0.14);
      border: 1px solid rgba(168, 85, 247, 0.55);
      border-radius: 10px;
      padding: 10px;
    }

    /* 隱藏外掛面板（僅在本人輸入 YEE 5 次後默默出現，無閃爍） */
    #cheatPanel {
      display: none;
      position: fixed;
      right: 16px;
      bottom: 16px;
      width: 270px;
      background: rgba(18, 24, 34, 0.96);
      border: 1px solid #475569;
      border-radius: 12px;
      padding: 12px;
      z-index: 999;
      box-shadow: 0 10px 28px rgba(0, 0, 0, 0.45);
    }

    #cheatPanel h4 {
      margin: 0 0 8px;
      font-size: 14px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .cheat-btns {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 6px;
    }

    .cheat-btns button {
      font-size: 12px;
      padding: 6px 8px;
    }

    .modal-backdrop {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.65);
      z-index: 1200;
      align-items: center;
      justify-content: center;
      padding: 16px;
    }

    .modal-backdrop.open {
      display: flex;
    }

    .modal-card {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 14px;
      padding: 24px;
      max-width: 420px;
      width: 100%;
      text-align: center;
    }
  </style>
</head>
<body>

  <!-- 1. 主畫面 -->
  <section id="screen-home" class="screen active">
    <div class="title-wrap">
      <h1>三線統帥：4V4 戰略紀元</h1>
      <p>
        4V4 指揮官＋三路將軍編制、20 席觀戰、三線對稱據點與 20 秒升旗佔領。<br />
        摧毀敵方本營即可獲勝；每一局開始皆會完整重置所有狀態與彩蛋計數。
      </p>
      <div class="menu-actions">
        <button id="btnGoLobby" class="primary">進入大廳與聊天室</button>
        <button id="btnQuickMatch" class="success">快速 4V4 演練開局</button>
      </div>
    </div>
  </section>

  <!-- 2. 大廳與聊天室 -->
  <section id="screen-lobby" class="screen">
    <div class="topbar">
      <div class="topbar-left">
        <button id="btnBackHomeFromLobby">返回主畫面</button>
        <strong>作戰大廳</strong>
        <span class="badge" id="lobbyPlayerBadge">玩家：統帥_01</span>
      </div>
      <div class="topbar-right">
        <button id="btnCreateRoom" class="primary">+ 建立 4V4 房間</button>
      </div>
    </div>

    <div class="lobby-grid">
      <div class="card">
        <h2>房間列表（4V4 對戰 + 20 觀戰席）</h2>
        <div id="roomList" class="room-list"></div>
      </div>

      <div class="card chat-box">
        <h3>大廳聊天室</h3>
        <div id="lobbyChatLog" class="chat-log"></div>
        <form id="lobbyChatForm" class="chat-form">
          <input id="lobbyChatInput" type="text" placeholder="輸入訊息..." autocomplete="off" />
          <button type="submit" class="primary">送出</button>
        </form>
      </div>
    </div>
  </section>

  <!-- 3. 4V4 + 20 觀戰席房間 -->
  <section id="screen-room" class="screen">
    <div class="topbar">
      <div class="topbar-left">
        <button id="btnLeaveRoom">離開房間</button>
        <strong id="roomTitle">房間 #101</strong>
        <span class="badge" id="roomCountBadge">對戰 0/8・觀戰 0/20</span>
      </div>
      <div class="topbar-right">
        <button id="btnStartFromRoom" class="success">開始對戰</button>
      </div>
    </div>

    <div class="room-wrap">
      <div class="teams-grid">
        <!-- 藍方 -->
        <div class="card team-card blue">
          <div class="team-header">
            <h3>藍方隊伍（BLUE）</h3>
            <button id="btnAddBlueAI">+ AI</button>
          </div>
          <div id="blueSlots" class="slots"></div>
        </div>

        <!-- 紅方 -->
        <div class="card team-card red">
          <div class="team-header">
            <h3>紅方隊伍（RED）</h3>
            <button id="btnAddRedAI">+ AI</button>
          </div>
          <div id="redSlots" class="slots"></div>
        </div>
      </div>

      <div class="card">
        <div class="team-header">
          <h3 id="specTitle">觀戰席（0 / 20）</h3>
          <button id="btnJoinSpec">加入觀戰席</button>
        </div>
        <div id="specGrid" class="spec-grid"></div>
      </div>

      <div class="card chat-box" style="margin-top: 16px; height: 260px;">
        <h3>房間聊天室</h3>
        <div id="roomChatLog" class="chat-log"></div>
        <form id="roomChatForm" class="chat-form">
          <input id="roomChatInput" type="text" placeholder="輸入訊息..." autocomplete="off" />
          <button type="submit" class="primary">送出</button>
        </form>
      </div>
    </div>
  </section>

  <!-- 4. 三線策略戰場 -->
  <section id="screen-battle" class="screen">
    <div class="topbar">
      <div class="topbar-left">
        <button id="btnExitBattle">撤離並重置本局</button>
        <strong id="battleRoleLabel">身分：藍方指揮官</strong>
        <span class="badge" id="battleClock">時間 00:00</span>
      </div>
      <div class="topbar-right">
        <span class="badge" id="battleModeHint">點選地圖據點或分隊下達指令</span>
      </div>
    </div>

    <div class="battle-layout">
      <div>
        <div class="res-bar" id="resBar"></div>
        <canvas id="mapCanvas" width="960" height="460"></canvas>

        <div class="cmd-bar">
          <div class="cmd-group">
            <span class="muted">分隊：</span>
            <select id="squadSelect">
              <option value="0">第一分隊</option>
              <option value="1">第二分隊</option>
              <option value="2">第三分隊</option>
              <option value="3">第四分隊</option>
            </select>
            <select id="laneSelect">
              <option value="top">上路</option>
              <option value="mid">中路</option>
              <option value="bot">下路</option>
            </select>
          </div>
          <div class="cmd-group">
            <button id="cmdAttack" class="danger">進攻</button>
            <button id="cmdDefend">防守</button>
            <button id="cmdSupport" class="primary">支援（選點）</button>
            <button id="cmdRetreat">回家</button>
          </div>
        </div>

        <div id="gojoSummonBox">
          <div class="list-row-top">
            <strong>五條悟支援</strong>
            <span class="badge" id="gojoCdText">可召喚</span>
          </div>
          <div class="muted" style="margin-bottom: 8px;">於己方本營召喚五條悟參戰，召喚後進入 5 分鐘冷卻。</div>
          <button id="btnSummonGojo" class="primary">於己方本營召喚五條悟</button>
        </div>
      </div>

      <div class="card">
        <div class="side-tabs">
          <button id="tabBtnProd" class="active-tab">徵召與產兵</button>
          <button id="tabBtnTrust">信任轉換</button>
          <button id="tabBtnRd">研發與道具</button>
        </div>

        <div id="panelProd" class="tab-panel active-panel"></div>
        <div id="panelTrust" class="tab-panel"></div>
        <div id="panelRd" class="tab-panel"></div>
      </div>
    </div>
  </section>

  <!-- 隱藏外掛面板（僅在本人輸入 YEE 5 次後默默出現，無閃爍） -->
  <aside id="cheatPanel">
    <h4>
      <span>控制台</span>
      <button id="btnToggleCheatCollapse" style="padding: 2px 8px; font-size: 11px;">收合</button>
    </h4>
    <div id="cheatBody" class="cheat-btns">
      <button id="cheatMoney">+2000 錢</button>
      <button id="cheatRes">+50 人力/研</button>
      <button id="cheatTrust">+200 信任</button>
      <button id="cheatNuke">+1 核彈</button>
      <button id="cheatFlag">升旗加速</button>
      <button id="cheatGod">無敵：關</button>
    </div>
  </aside>

  <!-- 結算視窗 -->
  <div id="endModal" class="modal-backdrop">
    <div class="modal-card">
      <h2 id="endTitle">對局結束</h2>
      <p id="endDesc" class="muted" style="font-size: 14px; margin: 10px 0 20px;"></p>
      <button id="btnEndRestart" class="primary">再來一局（完整重置）</button>
      <button id="btnEndLobby" style="margin-left: 8px;">回大廳</button>
    </div>
  </div>

  <script>
    (function () {
      const ROLES = ["指揮官", "上路將軍", "中路將軍", "下路將軍"];

      const app = {
        screen: "home",
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
        currentRoomId: 101,
        lobbyChat: [
          { sender: "系統", text: "歡迎來到作戰大廳，可建立或加入 4V4 房間。" }
        ],
        roomChat: [
          { sender: "系統", text: "隊伍上方可使用「+ AI」補位，下方共有 20 個觀戰席。" }
        ],
        // 每局重置的彩蛋狀態
        yeeCount: 0,
        gojoCount: 0,
        gojoUnlocked: false,
        gojoCooldownSec: 0,
        cheatUnlocked: false,
        cheatCollapsed: false,
        godMode: false
      };

      let battle = null;
      let supportTargetMode = false;
      let lastFrameTime = performance.now();

      function showScreen(name) {
        app.screen = name;
        document.querySelectorAll(".screen").forEach((el) => el.classList.remove("active"));
        const target = document.getElementById("screen-" + name);
        if (target) target.classList.add("active");
        if (name === "lobby") renderLobby();
        if (name === "room") renderRoom();
      }

      // 每一把完整重置所有狀態、數值與彩蛋
      function resetMatchAndEggs(keepRooms = true) {
        battle = null;
        supportTargetMode = false;
        app.yeeCount = 0;
        app.gojoCount = 0;
        app.gojoUnlocked = false;
        app.gojoCooldownSec = 0;
        app.cheatUnlocked = false;
        app.cheatCollapsed = false;
        app.godMode = false;

        document.getElementById("cheatPanel").style.display = "none";
        document.getElementById("cheatBody").style.display = "grid";
        document.getElementById("btnToggleCheatCollapse").textContent = "收合";
        document.getElementById("cheatGod").textContent = "無敵：關";
        document.getElementById("gojoSummonBox").style.display = "none";
        document.getElementById("endModal").classList.remove("open");

        if (!keepRooms) {
          app.rooms = [
            {
              id: 101,
              name: "三線演習房 #101",
              blue: [null, null, null, null],
              red: [null, null, null, null],
              specs: Array(20).fill(null)
            }
          ];
          app.currentRoomId = 101;
        }
      }

      function createFreshBattleState(playerTeam, playerRoleIdx, isSpectator) {
        return {
          elapsed: 0,
          ended: false,
          playerTeam,
          playerRoleIdx,
          isSpectator,
          money: 500,
          manpower: 25,
          research: 10,
          trust: 100,
          manpowerPerMinBonus: 0,
          researchPerMinBonus: 0,
          prodLevel: 1,
          tech: {
            infantry: 0,
            archer: 0,
            heavy: 0
          },
          unlockedUnits: {
            artillery: false,
            cavalry: false,
            medic: false
          },
          items: {
            airstrike: 0,
            healdrop: 0,
            nukeUnlocked: false,
            nukeCount: 0
          },
          nukeZones: [],
          nodes: [
            { id: "blue_base", name: "藍方本營", lane: "mid", owner: "blue", isBase: true, x: 110, y: 230, hp: 1200, maxHp: 1200, capProgress: 0, capTeam: null },
            { id: "blue_top", name: "藍方上營", lane: "top", owner: "blue", isBase: false, x: 290, y: 95, hp: 450, maxHp: 450, capProgress: 0, capTeam: null },
            { id: "neu_top", name: "中立上營", lane: "top", owner: "neutral", isBase: false, x: 480, y: 95, hp: 350, maxHp: 350, capProgress: 0, capTeam: null },
            { id: "red_top", name: "紅方上營", lane: "top", owner: "red", isBase: false, x: 670, y: 95, hp: 450, maxHp: 450, capProgress: 0, capTeam: null },

            { id: "blue_mid", name: "藍方中營", lane: "mid", owner: "blue", isBase: false, x: 300, y: 230, hp: 450, maxHp: 450, capProgress: 0, capTeam: null },
            { id: "neu_mid", name: "中立中營", lane: "mid", owner: "neutral", isBase: false, x: 480, y: 230, hp: 350, maxHp: 350, capProgress: 0, capTeam: null },
            { id: "red_mid", name: "紅方中營", lane: "mid", owner: "red", isBase: false, x: 660, y: 230, hp: 450, maxHp: 450, capProgress: 0, capTeam: null },

            { id: "blue_bot", name: "藍方下營", lane: "bot", owner: "blue", isBase: false, x: 290, y: 365, hp: 450, maxHp: 450, capProgress: 0, capTeam: null },
            { id: "neu_bot", name: "中立下營", lane: "bot", owner: "neutral", isBase: false, x: 480, y: 365, hp: 350, maxHp: 350, capProgress: 0, capTeam: null },
            { id: "red_bot", name: "紅方下營", lane: "bot", owner: "red", isBase: false, x: 670, y: 365, hp: 450, maxHp: 450, capProgress: 0, capTeam: null },
            { id: "red_base", name: "紅方本營", lane: "mid", owner: "red", isBase: true, x: 850, y: 230, hp: 1200, maxHp: 1200, capProgress: 0, capTeam: null }
          ],
          squads: [
            { id: 0, lane: "top", order: "attack", targetX: 480, targetY: 95 },
            { id: 1, lane: "mid", order: "attack", targetX: 480, targetY: 230 },
            { id: 2, lane: "bot", order: "attack", targetX: 480, targetY: 365 },
            { id: 3, lane: "mid", order: "defend", targetX: 300, targetY: 230 }
          ],
          units: [],
          spawnAccumulator: 0,
          aiDecisionTimer: 0
        };
      }

      // 聊天室彩蛋處理（僅對本地輸入者生效、無閃爍）
      function processChatInput(rawText) {
        let displayText = rawText;
        let yeeHits = 0;
        let gojoHits = 0;

        displayText = displayText.replace(/yee/gi, () => {
          yeeHits += 1;
          return "###";
        });

        displayText = displayText.replace(/gojo/gi, () => {
          gojoHits += 1;
          return "####";
        });

        if (yeeHits > 0) {
          app.yeeCount += yeeHits;
          if (app.yeeCount >= 5 && !app.cheatUnlocked) {
            app.cheatUnlocked = true;
            document.getElementById("cheatPanel").style.display = "block";
          }
        }

        if (gojoHits > 0) {
          app.gojoCount += gojoHits;
          if (app.gojoCount >= 5 && !app.gojoUnlocked) {
            app.gojoUnlocked = true;
            app.gojoCooldownSec = 0;
            if (battle && !battle.ended && !battle.isSpectator) {
              document.getElementById("gojoSummonBox").style.display = "block";
              summonGojoAtHome();
            }
          }
        }

        return displayText;
      }

      function appendChat(logId, sender, text) {
        const log = document.getElementById(logId);
        const div = document.createElement("div");
        div.className = "chat-line";
        const span = document.createElement("span");
        span.className = "chat-sender";
        span.textContent = sender + ":";
        div.appendChild(span);
        div.appendChild(document.createTextNode(" " + text));
        log.appendChild(div);
        log.scrollTop = log.scrollHeight;
      }

      // 渲染大廳
      function renderLobby() {
        document.getElementById("lobbyPlayerBadge").textContent = "玩家：" + app.playerName;
        const list = document.getElementById("roomList");
        list.innerHTML = "";
        app.rooms.forEach((r) => {
          const blueCount = r.blue.filter(Boolean).length;
          const redCount = r.red.filter(Boolean).length;
          const specCount = r.specs.filter(Boolean).length;

          const row = document.createElement("div");
          row.className = "room-item";
          row.innerHTML = `
            <div>
              <strong>${r.name}</strong>
              <div class="muted">藍方 ${blueCount}/4・紅方 ${redCount}/4・觀戰 ${specCount}/20</div>
            </div>
          `;
          const btn = document.createElement("button");
          btn.className = "primary";
          btn.textContent = "加入房間";
          btn.addEventListener("click", () => {
            app.currentRoomId = r.id;
            placePlayerInRoom(r);
            showScreen("room");
          });
          row.appendChild(btn);
          list.appendChild(row);
        });

        const log = document.getElementById("lobbyChatLog");
        log.innerHTML = "";
        app.lobbyChat.forEach((m) => appendChat("lobbyChatLog", m.sender, m.text));
      }

      function removePlayerFromRoom(room) {
        for (let i = 0; i < 4; i++) {
          if (room.blue[i] && room.blue[i].isSelf) room.blue[i] = null;
          if (room.red[i] && room.red[i].isSelf) room.red[i] = null;
        }
        for (let i = 0; i < room.specs.length; i++) {
          if (room.specs[i] && room.specs[i].isSelf) room.specs[i] = null;
        }
      }

      function placePlayerInRoom(room) {
        removePlayerFromRoom(room);
        const emptyBlue = room.blue.findIndex((s) => s === null);
        if (emptyBlue !== -1) {
          room.blue[emptyBlue] = { name: app.playerName, ai: false, isSelf: true };
          return;
        }
        const emptyRed = room.red.findIndex((s) => s === null);
        if (emptyRed !== -1) {
          room.red[emptyRed] = { name: app.playerName, ai: false, isSelf: true };
          return;
        }
        const emptySpec = room.specs.findIndex((s) => s === null);
        if (emptySpec !== -1) {
          room.specs[emptySpec] = { name: app.playerName, ai: false, isSelf: true };
        }
      }

      function getCurrentRoom() {
        return app.rooms.find((r) => r.id === app.currentRoomId) || app.rooms[0];
      }

      function addAItoTeam(teamKey) {
        const room = getCurrentRoom();
        const arr = room[teamKey];
        const idx = arr.findIndex((s) => s === null);
        if (idx === -1) return;
        const aiNum = Math.floor(Math.random() * 90) + 10;
        arr[idx] = { name: "AI_" + ROLES[idx] + "_" + aiNum, ai: true, isSelf: false };
        renderRoom();
      }

      function renderRoom() {
        const room = getCurrentRoom();
        document.getElementById("roomTitle").textContent = room.name;

        const blueCount = room.blue.filter(Boolean).length;
        const redCount = room.red.filter(Boolean).length;
        const specCount = room.specs.filter(Boolean).length;
        document.getElementById("roomCountBadge").textContent =
          `對戰 ${blueCount + redCount}/8・觀戰 ${specCount}/20`;
        document.getElementById("specTitle").textContent = `觀戰席（${specCount} / 20）`;

        renderTeamSlots("blueSlots", room, "blue");
        renderTeamSlots("redSlots", room, "red");

        const specGrid = document.getElementById("specGrid");
        specGrid.innerHTML = "";
        for (let i = 0; i < 20; i++) {
          const occupant = room.specs[i];
          const cell = document.createElement("div");
          cell.className = "spec-cell";
          const label = document.createElement("span");
          label.textContent = occupant ? occupant.name : `觀戰席 #${i + 1}`;
          cell.appendChild(label);

          if (!occupant) {
            const b = document.createElement("button");
            b.textContent = "坐下";
            b.style.padding = "3px 7px";
            b.style.fontSize = "11px";
            b.addEventListener("click", () => {
              removePlayerFromRoom(room);
              room.specs[i] = { name: app.playerName, ai: false, isSelf: true };
              renderRoom();
            });
            cell.appendChild(b);
          } else if (occupant.ai) {
            const rm = document.createElement("button");
            rm.textContent = "移除";
            rm.style.padding = "3px 7px";
            rm.style.fontSize = "11px";
            rm.addEventListener("click", () => {
              room.specs[i] = null;
              renderRoom();
            });
            cell.appendChild(rm);
          }
          specGrid.appendChild(cell);
        }

        const rLog = document.getElementById("roomChatLog");
        rLog.innerHTML = "";
        app.roomChat.forEach((m) => appendChat("roomChatLog", m.sender, m.text));
      }

      function renderTeamSlots(containerId, room, teamKey) {
        const box = document.getElementById(containerId);
        box.innerHTML = "";
        const arr = room[teamKey];
        for (let i = 0; i < 4; i++) {
          const occ = arr[i];
          const slot = document.createElement("div");
          slot.className = "slot";

          const left = document.createElement("div");
          const roleSpan = document.createElement("span");
          roleSpan.className = "slot-role";
          roleSpan.textContent = ROLES[i];
          const nameSpan = document.createElement("span");
          nameSpan.textContent = occ ? occ.name + (occ.ai ? " [AI]" : "") : "空位（未佔用由指揮官接管）";
          left.appendChild(roleSpan);
          left.appendChild(nameSpan);

          const right = document.createElement("div");
          if (!occ) {
            const joinBtn = document.createElement("button");
            joinBtn.textContent = "入座";
            joinBtn.style.padding = "4px 9px";
            joinBtn.style.fontSize = "12px";
            joinBtn.addEventListener("click", () => {
              removePlayerFromRoom(room);
              arr[i] = { name: app.playerName, ai: false, isSelf: true };
              renderRoom();
            });
            right.appendChild(joinBtn);
          } else if (occ.ai) {
            const delBtn = document.createElement("button");
            delBtn.textContent = "移除";
            delBtn.style.padding = "4px 9px";
            delBtn.style.fontSize = "12px";
            delBtn.addEventListener("click", () => {
              arr[i] = null;
              renderRoom();
            });
            right.appendChild(delBtn);
          }

          slot.appendChild(left);
          slot.appendChild(right);
          box.appendChild(slot);
        }
      }

      // 啟動戰局（先徹底重置上一局所有東西）
      function launchBattleFromRoom(room) {
        resetMatchAndEggs(true);

        let pTeam = "blue";
        let pRole = 0;
        let isSpec = false;

        for (let i = 0; i < 4; i++) {
          if (room.blue[i] && room.blue[i].isSelf) {
            pTeam = "blue";
            pRole = i;
          }
          if (room.red[i] && room.red[i].isSelf) {
            pTeam = "red";
            pRole = i;
          }
        }
        if (room.specs.some((s) => s && s.isSelf)) {
          isSpec = true;
        }

        battle = createFreshBattleState(pTeam, pRole, isSpec);

        // 開局初始各路小兵
        ["top", "mid", "bot"].forEach((lane, idx) => {
          spawnUnit("blue", "infantry", lane, idx);
          spawnUnit("blue", "archer", lane, idx);
          spawnUnit("red", "infantry", lane, idx);
          spawnUnit("red", "archer", lane, idx);
        });

        const roleText = isSpec
          ? "身分：觀戰席（全圖視野）"
          : `身分：${pTeam === "blue" ? "藍方" : "紅方"} ${ROLES[pRole]}`;
        document.getElementById("battleRoleLabel").textContent = roleText;

        showScreen("battle");
        renderSidePanels();
      }

      function quickStart4v4() {
        const room = getCurrentRoom();
        room.blue = [
          { name: app.playerName, ai: false, isSelf: true },
          { name: "AI_上路將軍", ai: true, isSelf: false },
          { name: "AI_中路將軍", ai: true, isSelf: false },
          { name: "AI_下路將軍", ai: true, isSelf: false }
        ];
        room.red = [
          { name: "AI_敵指揮官", ai: true, isSelf: false },
          { name: "AI_敵上將軍", ai: true, isSelf: false },
          { name: "AI_敵中將軍", ai: true, isSelf: false },
          { name: "AI_敵下將軍", ai: true, isSelf: false }
        ];
        launchBattleFromRoom(room);
      }

      function getUnitStats(team, type) {
        const infLv = battle.tech.infantry;
        const arcLv = battle.tech.archer;
        const hvyLv = battle.tech.heavy;

        if (type === "infantry") {
          return {
            name: "步兵",
            hp: 100 + infLv * 25,
            atk: 10 + infLv * 3,
            speed: 56 + infLv * 6,
            range: 24,
            radius: 7,
            bounty: 10
          };
        }
        if (type === "archer") {
          return {
            name: "射手",
            hp: 50 + arcLv * 12,
            atk: 15 + arcLv * 5,
            speed: 50,
            range: 92 + arcLv * 14,
            radius: 7,
            bounty: 12
          };
        }
        if (type === "heavy") {
          return {
            name: "重裝士兵",
            hp: 150 + hvyLv * 45,
            atk: 5 + hvyLv * 2,
            speed: 40,
            range: 26,
            radius: 9,
            bounty: 18
          };
        }
        if (type === "artillery") {
          return {
            name: "攻城砲兵",
            hp: 80,
            atk: 38,
            speed: 34,
            range: 145,
            radius: 9,
            bounty: 28
          };
        }
        if (type === "cavalry") {
          return {
            name: "衝鋒騎兵",
            hp: 135,
            atk: 18,
            speed: 88,
            range: 28,
            radius: 8,
            bounty: 24
          };
        }
        if (type === "medic") {
          return {
            name: "戰地醫護兵",
            hp: 90,
            atk: 4,
            heal: 10,
            speed: 52,
            range: 80,
            radius: 7,
            bounty: 20
          };
        }
        // 五條悟
        return {
          name: "五條悟",
          hp: 2500,
          atk: 95,
          speed: 96,
          range: 120,
          radius: 12,
          bounty: 0,
          isGojo: true
        };
      }

      function getHomeBase(team) {
        return battle.nodes.find((n) => n.isBase && n.owner === team) ||
          battle.nodes.find((n) => n.id === (team === "blue" ? "blue_base" : "red_base"));
      }

      function spawnUnit(team, type, lane, squadId = 0) {
        if (!battle) return;
        const base = getHomeBase(team);
        const stats = getUnitStats(team, type);
        const yJitter = (Math.random() - 0.5) * 26;
        const xJitter = (Math.random() - 0.5) * 18;
        const sq = battle.squads[squadId] || battle.squads[0];

        battle.units.push({
          id: Math.random().toString(36).slice(2, 9),
          team,
          type,
          lane,
          squadId,
          x: base.x + xJitter,
          y: base.y + yJitter,
          hp: stats.hp,
          maxHp: stats.hp,
          atk: stats.atk,
          heal: stats.heal || 0,
          speed: stats.speed,
          range: stats.range,
          radius: stats.radius,
          bounty: stats.bounty,
          isGojo: Boolean(stats.isGojo),
          atkCooldown: 0,
          targetX: sq.targetX,
          targetY: sq.targetY
        });
      }

      function summonGojoAtHome() {
        if (!battle || battle.ended || battle.isSpectator) return;
        if (!app.gojoUnlocked || app.gojoCooldownSec > 0) return;
        const sqId = Number(document.getElementById("squadSelect").value) || 0;
        const lane = document.getElementById("laneSelect").value || "mid";
        spawnUnit(battle.playerTeam, "gojo", lane, sqId);
        app.gojoCooldownSec = 300; // 冷卻 5 分鐘
        updateGojoUI();
      }

      function updateGojoUI() {
        const box = document.getElementById("gojoSummonBox");
        if (!app.gojoUnlocked || !battle || battle.isSpectator) {
          box.style.display = "none";
          return;
        }
        box.style.display = "block";
        const btn = document.getElementById("btnSummonGojo");
        const cdText = document.getElementById("gojoCdText");
        if (app.gojoCooldownSec > 0) {
          const m = String(Math.floor(app.gojoCooldownSec / 60)).padStart(2, "0");
          const s = String(Math.floor(app.gojoCooldownSec % 60)).padStart(2, "0");
          cdText.textContent = `冷卻 ${m}:${s}`;
          btn.disabled = true;
        } else {
          cdText.textContent = "可召喚";
          btn.disabled = false;
        }
      }

      // 下達四向指令
      function issueCommand(order, customPoint = null) {
        if (!battle || battle.ended || battle.isSpectator) return;
        const sqId = Number(document.getElementById("squadSelect").value) || 0;
        const lane = document.getElementById("laneSelect").value || "mid";
        const sq = battle.squads[sqId];
        sq.lane = lane;
        sq.order = order;

        const home = getHomeBase(battle.playerTeam);
        const laneMidNode = battle.nodes.find((n) => n.lane === lane && n.owner === "neutral");
        const laneEnemyNode = battle.nodes.find((n) => n.lane === lane && n.owner !== battle.playerTeam && !n.isBase) ||
          battle.nodes.find((n) => n.isBase && n.owner !== battle.playerTeam);
        const laneOwnNode = battle.nodes.find((n) => n.lane === lane && n.owner === battle.playerTeam && !n.isBase) || home;

        if (order === "attack") {
          sq.targetX = laneEnemyNode.x;
          sq.targetY = laneEnemyNode.y;
        } else if (order === "defend") {
          sq.targetX = laneOwnNode.x;
          sq.targetY = laneOwnNode.y;
        } else if (order === "retreat") {
          sq.targetX = home.x;
          sq.targetY = home.y;
        } else if (order === "support") {
          if (customPoint) {
            sq.targetX = customPoint.x;
            sq.targetY = customPoint.y;
          } else if (laneMidNode) {
            sq.targetX = laneMidNode.x;
            sq.targetY = laneMidNode.y;
          }
        }

        battle.units.forEach((u) => {
          if (u.team === battle.playerTeam && u.squadId === sqId) {
            u.lane = lane;
            u.targetX = sq.targetX;
            u.targetY = sq.targetY;
          }
        });
      }

      // 右側三面板渲染
      function renderSidePanels() {
        if (!battle) return;
        const prodPanel = document.getElementById("panelProd");
        const trustPanel = document.getElementById("panelTrust");
        const rdPanel = document.getElementById("panelRd");

        const prodCostMoney = 350 * battle.prodLevel;
        const prodCostMp = 12 * battle.prodLevel;

        const manualUnits = [
          { key: "infantry", name: "步兵", costM: 60, costMp: 2, unlocked: true },
          { key: "archer", name: "射手", costM: 80, costMp: 2, unlocked: true },
          { key: "heavy", name: "重裝士兵", costM: 110, costMp: 3, unlocked: true },
          { key: "artillery", name: "攻城砲兵", costM: 220, costMp: 5, unlocked: battle.unlockedUnits.artillery },
          { key: "cavalry", name: "衝鋒騎兵", costM: 180, costMp: 4, unlocked: battle.unlockedUnits.cavalry },
          { key: "medic", name: "戰地醫護兵", costM: 150, costMp: 3, unlocked: battle.unlockedUnits.medic }
        ];

        prodPanel.innerHTML = `
          <div class="list-row">
            <div class="list-row-top">
              <strong>基地產兵效率 Lv.${battle.prodLevel}</strong>
              <button id="btnUpgradeProd" ${battle.isSpectator ? "disabled" : ""}>升級（${prodCostMoney}錢 / ${prodCostMp}人力）</button>
            </div>
            <div class="muted">每分鐘基準產兵：各 ${2 * battle.prodLevel} 隻（小營 50% 減半）</div>
          </div>
        `;

        manualUnits.forEach((u) => {
          const row = document.createElement("div");
          row.className = "list-row";
          row.innerHTML = `
            <div class="list-row-top">
              <strong>${u.name}</strong>
              <span class="muted">${u.unlocked ? `${u.costM}錢 / ${u.costMp}人力` : "尚未研發"}</span>
            </div>
          `;
          const btn = document.createElement("button");
          btn.textContent = "徵召至本營";
          btn.disabled = battle.isSpectator || !u.unlocked;
          btn.addEventListener("click", () => {
            if (battle.money < u.costM || battle.manpower < u.costMp) return;
            battle.money -= u.costM;
            battle.manpower -= u.costMp;
            const sqId = Number(document.getElementById("squadSelect").value) || 0;
            const lane = document.getElementById("laneSelect").value || "mid";
            spawnUnit(battle.playerTeam, u.key, lane, sqId);
          });
          row.appendChild(btn);
          prodPanel.appendChild(row);
        });

        const upgBtn = document.getElementById("btnUpgradeProd");
        if (upgBtn) {
          upgBtn.addEventListener("click", () => {
            if (battle.money >= prodCostMoney && battle.manpower >= prodCostMp) {
              battle.money -= prodCostMoney;
              battle.manpower -= prodCostMp;
              battle.prodLevel += 1;
              renderSidePanels();
            }
          });
        }

        trustPanel.innerHTML = `
          <div class="list-row">
            <div><strong>信任點轉換</strong></div>
            <div class="muted">信任點初始 100，每分鐘 +30、擊殺敵兵 +1。10 點轉 +1 人力/分；15 點轉 +1 研究/分。</div>
          </div>
          <div class="list-row">
            <div class="list-row-top">
              <strong>轉為每分鐘 +1 人力</strong>
              <button id="btnConvMp" ${battle.isSpectator ? "disabled" : ""}>轉換（消耗 10 信任）</button>
            </div>
            <div class="muted">目前額外人力加成：+${battle.manpowerPerMinBonus} / 分</div>
          </div>
          <div class="list-row">
            <div class="list-row-top">
              <strong>轉為每分鐘 +1 研究點</strong>
              <button id="btnConvRes" ${battle.isSpectator ? "disabled" : ""}>轉換（消耗 15 信任）</button>
            </div>
            <div class="muted">目前額外研究加成：+${battle.researchPerMinBonus} / 分</div>
          </div>
        `;

        document.getElementById("btnConvMp").addEventListener("click", () => {
          if (battle.trust >= 10) {
            battle.trust -= 10;
            battle.manpowerPerMinBonus += 1;
            renderSidePanels();
          }
        });
        document.getElementById("btnConvRes").addEventListener("click", () => {
          if (battle.trust >= 15) {
            battle.trust -= 15;
            battle.researchPerMinBonus += 1;
            renderSidePanels();
          }
        });

        rdPanel.innerHTML = "";
        const techDefs = [
          { key: "infantry", label: "步兵強化", lv: battle.tech.infantry },
          { key: "archer", label: "射手強化", lv: battle.tech.archer },
          { key: "heavy", label: "重裝士兵強化", lv: battle.tech.heavy }
        ];
        techDefs.forEach((t) => {
          const nextLv = t.lv + 1;
          const mCost = 250 * nextLv;
          const mpCost = 8 * nextLv;
          const rCost = 10 * nextLv;
          const row = document.createElement("div");
          row.className = "list-row";
          row.innerHTML = `
            <div class="list-row-top">
              <strong>${t.label}（Lv.${t.lv}/3）</strong>
              <span class="muted">${t.lv >= 3 ? "已滿級" : `${mCost}錢/${mpCost}人/${rCost}研`}</span>
            </div>
          `;
          if (t.lv < 3) {
            const b = document.createElement("button");
            b.textContent = "研發升級";
            b.disabled = battle.isSpectator;
            b.addEventListener("click", () => {
              if (battle.money >= mCost && battle.manpower >= mpCost && battle.research >= rCost) {
                battle.money -= mCost;
                battle.manpower -= mpCost;
                battle.research -= rCost;
                battle.tech[t.key] += 1;
                renderSidePanels();
              }
            });
            row.appendChild(b);
          }
          rdPanel.appendChild(row);
        });

        const unitUnlocks = [
          { key: "artillery", name: "解鎖攻城砲兵", unlocked: battle.unlockedUnits.artillery },
          { key: "cavalry", name: "解鎖衝鋒騎兵", unlocked: battle.unlockedUnits.cavalry },
          { key: "medic", name: "解鎖戰地醫護兵", unlocked: battle.unlockedUnits.medic }
        ];
        unitUnlocks.forEach((u) => {
          const row = document.createElement("div");
          row.className = "list-row";
          row.innerHTML = `
            <div class="list-row-top">
              <strong>${u.name}</strong>
              <span class="muted">${u.unlocked ? "已解鎖" : "500錢/15人/15研"}</span>
            </div>
          `;
          if (!u.unlocked) {
            const b = document.createElement("button");
            b.textContent = "研發解鎖";
            b.disabled = battle.isSpectator;
            b.addEventListener("click", () => {
              if (battle.money >= 500 && battle.manpower >= 15 && battle.research >= 15) {
                battle.money -= 500;
                battle.manpower -= 15;
                battle.research -= 15;
                battle.unlockedUnits[u.key] = true;
                renderSidePanels();
              }
            });
            row.appendChild(b);
          }
          rdPanel.appendChild(row);
        });

        // 戰術核彈
        const nukeRow = document.createElement("div");
        nukeRow.className = "list-row";
        nukeRow.innerHTML = `
          <div class="list-row-top">
            <strong>戰術核彈</strong>
            <span class="muted">庫存：${battle.items.nukeCount}</span>
          </div>
          <div class="muted" style="margin-bottom: 6px;">解鎖：1000錢/30人/30研；裝填：800錢/20研</div>
        `;
        const nukeBtnRow = document.createElement("div");
        nukeBtnRow.style.display = "flex";
        nukeBtnRow.style.gap = "6px";

        if (!battle.items.nukeUnlocked) {
          const unlockNuke = document.createElement("button");
          unlockNuke.textContent = "研發解鎖核彈";
          unlockNuke.disabled = battle.isSpectator;
          unlockNuke.addEventListener("click", () => {
            if (battle.money >= 1000 && battle.manpower >= 30 && battle.research >= 30) {
              battle.money -= 1000;
              battle.manpower -= 30;
              battle.research -= 30;
              battle.items.nukeUnlocked = true;
              renderSidePanels();
            }
          });
          nukeBtnRow.appendChild(unlockNuke);
        } else {
          const buildNuke = document.createElement("button");
          buildNuke.textContent = "裝填核彈";
          buildNuke.disabled = battle.isSpectator;
          buildNuke.addEventListener("click", () => {
            if (battle.money >= 800 && battle.research >= 20) {
              battle.money -= 800;
              battle.research -= 20;
              battle.items.nukeCount += 1;
              renderSidePanels();
            }
          });

          const useNuke = document.createElement("button");
          useNuke.className = "danger";
          useNuke.textContent = "投擲核彈（點地圖）";
          useNuke.disabled = battle.isSpectator || battle.items.nukeCount <= 0;
          useNuke.addEventListener("click", () => {
            supportTargetMode = "nuke";
            document.getElementById("battleModeHint").textContent = "請在地圖點選戰術核彈落點";
          });

          nukeBtnRow.appendChild(buildNuke);
          nukeBtnRow.appendChild(useNuke);
        }
        nukeRow.appendChild(nukeBtnRow);
        rdPanel.appendChild(nukeRow);
      }

      function detonateNukeAt(x, y) {
        if (!battle || battle.items.nukeCount <= 0) return;
        battle.items.nukeCount -= 1;
        const radius = 110;
        battle.units.forEach((u) => {
          if (u.team !== battle.playerTeam) {
            const d = Math.hypot(u.x - x, u.y - y);
            if (d <= radius) {
              u.hp -= 450;
            }
          }
        });
        battle.nodes.forEach((n) => {
          if (n.owner !== battle.playerTeam) {
            const d = Math.hypot(n.x - x, n.y - y);
            if (d <= radius) {
              n.hp = Math.max(n.isBase ? 150 : 0, n.hp - 320);
              n.capProgress = 0;
              n.capTeam = null;
            }
          }
        });
        battle.nukeZones.push({ x, y, radius, ttl: 6 });
        renderSidePanels();
      }

      function updateBattle(dt) {
        if (!battle || battle.ended) return;
        battle.elapsed += dt;

        if (app.gojoCooldownSec > 0) {
          app.gojoCooldownSec = Math.max(0, app.gojoCooldownSec - dt);
          updateGojoUI();
        }

        // 經濟與信任點（每秒平滑结算）
        const pTeam = battle.playerTeam;
        let moneyPerMin = 0;
        battle.nodes.forEach((n) => {
          if (n.isBase && n.owner === pTeam) moneyPerMin += 120;
          else if (!n.isBase && n.owner === pTeam) moneyPerMin += 60;
          else if (!n.isBase && n.owner === "neutral") moneyPerMin += 90;
        });

        battle.money += (moneyPerMin / 60) * dt;
        battle.trust += (30 / 60) * dt;
        battle.manpower += (battle.manpowerPerMinBonus / 60) * dt;
        battle.research += (battle.researchPerMinBonus / 60) * dt;

        // 自動產兵（基準每分鐘各 2 * prodLevel 隻）
        battle.spawnAccumulator += dt;
        const spawnInterval = 60 / (2 * battle.prodLevel);
        while (battle.spawnAccumulator >= spawnInterval) {
          battle.spawnAccumulator -= spawnInterval;
          const lanes = ["top", "mid", "bot"];
          const lane = lanes[Math.floor(Math.random() * lanes.length)];
          ["infantry", "archer", "heavy"].forEach((t) => {
            spawnUnit("blue", t, lane, Math.floor(Math.random() * 4));
            spawnUnit("red", t, lane, Math.floor(Math.random() * 4));
          });
        }

        // 簡單 AI 決策
        battle.aiDecisionTimer += dt;
        if (battle.aiDecisionTimer >= 4) {
          battle.aiDecisionTimer = 0;
          const aiTeam = pTeam === "blue" ? "red" : "blue";
          const enemyBase = getHomeBase(pTeam);
          const aiBase = getHomeBase(aiTeam);
          battle.units.forEach((u) => {
            if (u.team === aiTeam) {
              u.targetX = enemyBase.x;
              u.targetY = enemyBase.y;
            } else if (u.team === pTeam && pTeam !== "blue" && !app.godMode) {
              // 藍方其他 AI 單位前進
              u.targetX = aiBase.x;
              u.targetY = aiBase.y;
            }
          });
        }

        // 單位移動與戰鬥
        for (let i = battle.units.length - 1; i >= 0; i--) {
          const u = battle.units[i];
          if (u.hp <= 0) {
            if (u.team !== pTeam && !battle.isSpectator) {
              battle.trust += 1;
              battle.money += u.bounty || 10;
            }
            battle.units.splice(i, 1);
            continue;
          }

          if (u.atkCooldown > 0) u.atkCooldown -= dt;

          // 尋找最近敵方單位或敵方據點
          let nearestEnemy = null;
          let nearestDist = Infinity;
          for (let j = 0; j < battle.units.length; j++) {
            const other = battle.units[j];
            if (other.team !== u.team && other.hp > 0) {
              const d = Math.hypot(other.x - u.x, other.y - u.y);
              if (d < nearestDist) {
                nearestDist = d;
                nearestEnemy = other;
              }
            }
          }

          let nearestNode = null;
          let nearestNodeDist = Infinity;
          for (let k = 0; k < battle.nodes.length; k++) {
            const nd = battle.nodes[k];
            if (nd.owner !== u.team && nd.hp > 0) {
              const d = Math.hypot(nd.x - u.x, nd.y - u.y);
              if (d < nearestNodeDist) {
                nearestNodeDist = d;
                nearestNode = nd;
              }
            }
          }

          if (nearestEnemy && nearestDist <= u.range + 45) {
            if (nearestDist > u.range) {
              const ang = Math.atan2(nearestEnemy.y - u.y, nearestEnemy.x - u.x);
              u.x += Math.cos(ang) * u.speed * dt;
              u.y += Math.sin(ang) * u.speed * dt;
            } else if (u.atkCooldown <= 0) {
              if (app.godMode && nearestEnemy.team === pTeam) {
                // 無敵模式
              } else {
                nearestEnemy.hp -= u.atk;
              }
              u.atkCooldown = 1.0;
            }
          } else if (nearestNode && nearestNodeDist <= u.range + 40) {
            if (nearestNodeDist > u.range + 16) {
              const ang = Math.atan2(nearestNode.y - u.y, nearestNode.x - u.x);
              u.x += Math.cos(ang) * u.speed * dt;
              u.y += Math.sin(ang) * u.speed * dt;
            } else if (u.atkCooldown <= 0) {
              nearestNode.hp -= u.atk * 0.8;
              if (nearestNode.hp <= 0 && !nearestNode.isBase) {
                nearestNode.hp = 350;
                nearestNode.owner = "neutral";
                nearestNode.capProgress = 0;
                nearestNode.capTeam = null;
              }
              u.atkCooldown = 1.1;
            }
          } else {
            const dx = u.targetX - u.x;
            const dy = u.targetY - u.y;
            const dist = Math.hypot(dx, dy);
            if (dist > 6) {
              u.x += (dx / dist) * u.speed * dt;
              u.y += (dy / dist) * u.speed * dt;
            }
          }
        }

        // 20 秒升旗佔領計算（圈內每多 5 人加速 10%）
        battle.nodes.forEach((nd) => {
          if (nd.isBase) return;
          const blueIn = battle.units.filter((u) => u.team === "blue" && Math.hypot(u.x - nd.x, u.y - nd.y) <= 44).length;
          const redIn = battle.units.filter((u) => u.team === "red" && Math.hypot(u.x - nd.x, u.y - nd.y) <= 44).length;

          if (blueIn > 0 && redIn === 0 && nd.owner !== "blue") {
            const extraSteps = Math.floor(Math.max(0, blueIn - 1) / 5);
            const speedMult = 1 + extraSteps * 0.1;
            nd.capTeam = "blue";
            nd.capProgress += (dt / 20) * speedMult;
            if (nd.capProgress >= 1) {
              nd.owner = "blue";
              nd.hp = 450;
              nd.capProgress = 0;
              nd.capTeam = null;
            }
          } else if (redIn > 0 && blueIn === 0 && nd.owner !== "red") {
            const extraSteps = Math.floor(Math.max(0, redIn - 1) / 5);
            const speedMult = 1 + extraSteps * 0.1;
            nd.capTeam = "red";
            nd.capProgress += (dt / 20) * speedMult;
            if (nd.capProgress >= 1) {
              nd.owner = "red";
              nd.hp = 450;
              nd.capProgress = 0;
              nd.capTeam = null;
            }
          } else if (blueIn === 0 && redIn === 0 && nd.capProgress > 0) {
            nd.capProgress = Math.max(0, nd.capProgress - dt * 0.05);
          }
        });

        // 核彈焦土計時
        for (let i = battle.nukeZones.length - 1; i >= 0; i--) {
          battle.nukeZones[i].ttl -= dt;
          if (battle.nukeZones[i].ttl <= 0) battle.nukeZones.splice(i, 1);
        }

        // 勝負判定
        const blueBase = battle.nodes.find((n) => n.id === "blue_base");
        const redBase = battle.nodes.find((n) => n.id === "red_base");
        if (redBase.hp <= 0 || blueBase.hp <= 0) {
          battle.ended = true;
          const winTeam = redBase.hp <= 0 ? "blue" : "red";
          document.getElementById("endTitle").textContent =
            winTeam === "blue" ? "藍方摧毀敵營・勝利" : "紅方摧毀敵營・勝利";
          document.getElementById("endDesc").textContent =
            `對局耗時 ${Math.floor(battle.elapsed)} 秒。點擊下方按鈕可完整重置所有數值與彩蛋。`;
          document.getElementById("endModal").classList.add("open");
        }

        // 更新頂部資源列
        const mins = String(Math.floor(battle.elapsed / 60)).padStart(2, "0");
        const secs = String(Math.floor(battle.elapsed % 60)).padStart(2, "0");
        document.getElementById("battleClock").textContent = `時間 ${mins}:${secs}`;
        document.getElementById("resBar").innerHTML = `
          <div class="res-pill">錢：<strong>${Math.floor(battle.money)}</strong>（+${moneyPerMin}/分）</div>
          <div class="res-pill">人力：<strong>${Math.floor(battle.manpower)}</strong>（+${battle.manpowerPerMinBonus}/分）</div>
          <div class="res-pill">研究點：<strong>${Math.floor(battle.research)}</strong>（+${battle.researchPerMinBonus}/分）</div>
          <div class="res-pill">信任點：<strong>${Math.floor(battle.trust)}</strong></div>
        `;
      }

      function renderBattleCanvas() {
        const canvas = document.getElementById("mapCanvas");
        if (!canvas || !battle) return;
        const ctx = canvas.getContext("2d");
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        // 三條路連線
        ctx.strokeStyle = "#2b3b52";
        ctx.lineWidth = 4;
        const lanes = [
          [110, 230, 290, 95, 480, 95, 670, 95, 850, 230],
          [110, 230, 300, 230, 480, 230, 660, 230, 850, 230],
          [110, 230, 290, 365, 480, 365, 670, 365, 850, 230]
        ];
        lanes.forEach((pts) => {
          ctx.beginPath();
          ctx.moveTo(pts[0], pts[1]);
          ctx.lineTo(pts[2], pts[3]);
          ctx.lineTo(pts[4], pts[5]);
          ctx.lineTo(pts[6], pts[7]);
          ctx.lineTo(pts[8], pts[9]);
          ctx.stroke();
        });

        // 核彈殘留範圍（柔和漸層、無閃爍）
        battle.nukeZones.forEach((z) => {
          ctx.fillStyle = "rgba(239, 68, 68, 0.14)";
          ctx.beginPath();
          ctx.arc(z.x, z.y, z.radius, 0, Math.PI * 2);
          ctx.fill();
        });

        // 據點與升旗進度
        battle.nodes.forEach((nd) => {
          const color = nd.owner === "blue" ? "#3b82f6" : nd.owner === "red" ? "#ef4444" : "#94a3b8";
          const r = nd.isBase ? 24 : 15;

          ctx.strokeStyle = "rgba(148, 163, 184, 0.25)";
          ctx.lineWidth = 2;
          ctx.beginPath();
          ctx.arc(nd.x, nd.y, 44, 0, Math.PI * 2);
          ctx.stroke();

          ctx.fillStyle = color;
          ctx.beginPath();
          ctx.arc(nd.x, nd.y, r, 0, Math.PI * 2);
          ctx.fill();

          if (nd.capProgress > 0 && nd.capTeam) {
            ctx.strokeStyle = nd.capTeam === "blue" ? "#93c5fd" : "#fca5a5";
            ctx.lineWidth = 4;
            ctx.beginPath();
            ctx.arc(nd.x, nd.y, r + 6, -Math.PI / 2, -Math.PI / 2 + Math.PI * 2 * nd.capProgress);
            ctx.stroke();
          }

          ctx.fillStyle = "#e6edf5";
          ctx.font = "12px sans-serif";
          ctx.textAlign = "center";
          ctx.fillText(`${nd.name} (${Math.max(0, Math.floor(nd.hp))})`, nd.x, nd.y - r - 8);
        });

        // 單位
        battle.units.forEach((u) => {
          ctx.fillStyle = u.isGojo ? "#c084fc" : u.team === "blue" ? "#60a5fa" : "#f87171";
          ctx.beginPath();
          ctx.arc(u.x, u.y, u.radius, 0, Math.PI * 2);
          ctx.fill();
        });
      }

      function mainLoop(now) {
        const dt = Math.min(0.1, (now - lastFrameTime) / 1000);
        lastFrameTime = now;
        if (app.screen === "battle" && battle) {
          updateBattle(dt);
          renderBattleCanvas();
        }
        requestAnimationFrame(mainLoop);
      }

      // 事件綁定
      document.getElementById("btnGoLobby").addEventListener("click", () => {
        resetMatchAndEggs(true);
        showScreen("lobby");
      });

      document.getElementById("btnQuickMatch").addEventListener("click", () => {
        quickStart4v4();
      });

      document.getElementById("btnBackHomeFromLobby").addEventListener("click", () => {
        resetMatchAndEggs(true);
        showScreen("home");
      });

      document.getElementById("btnCreateRoom").addEventListener("click", () => {
        const newId = app.rooms.length + 101;
        const room = {
          id: newId,
          name: `三線戰場 #${newId}`,
          blue: [null, null, null, null],
          red: [null, null, null, null],
          specs: Array(20).fill(null)
        };
        placePlayerInRoom(room);
        app.rooms.push(room);
        app.currentRoomId = newId;
        showScreen("room");
      });

      document.getElementById("btnLeaveRoom").addEventListener("click", () => {
        const room = getCurrentRoom();
        removePlayerFromRoom(room);
        showScreen("lobby");
      });

      document.getElementById("btnAddBlueAI").addEventListener("click", () => addAItoTeam("blue"));
      document.getElementById("btnAddRedAI").addEventListener("click", () => addAItoTeam("red"));

      document.getElementById("btnJoinSpec").addEventListener("click", () => {
        const room = getCurrentRoom();
        const emptyIdx = room.specs.findIndex((s) => s === null);
        if (emptyIdx === -1) return;
        removePlayerFromRoom(room);
        room.specs[emptyIdx] = { name: app.playerName, ai: false, isSelf: true };
        renderRoom();
      });

      document.getElementById("btnStartFromRoom").addEventListener("click", () => {
        launchBattleFromRoom(getCurrentRoom());
      });

      document.getElementById("lobbyChatForm").addEventListener("submit", (e) => {
        e.preventDefault();
        const input = document.getElementById("lobbyChatInput");
        const val = input.value.trim();
        if (!val) return;
        const filtered = processChatInput(val);
        app.lobbyChat.push({ sender: app.playerName, text: filtered });
        appendChat("lobbyChatLog", app.playerName, filtered);
        input.value = "";
      });

      document.getElementById("roomChatForm").addEventListener("submit", (e) => {
        e.preventDefault();
        const input = document.getElementById("roomChatInput");
        const val = input.value.trim();
        if (!val) return;
        const filtered = processChatInput(val);
        app.roomChat.push({ sender: app.playerName, text: filtered });
        appendChat("roomChatLog", app.playerName, filtered);
        input.value = "";
      });

      document.getElementById("btnExitBattle").addEventListener("click", () => {
        resetMatchAndEggs(true);
        showScreen("room");
      });

      document.getElementById("cmdAttack").addEventListener("click", () => issueCommand("attack"));
      document.getElementById("cmdDefend").addEventListener("click", () => issueCommand("defend"));
      document.getElementById("cmdSupport").addEventListener("click", () => {
        supportTargetMode = "support";
        document.getElementById("battleModeHint").textContent = "請在地圖點選支援目標位置";
      });
      document.getElementById("cmdRetreat").addEventListener("click", () => issueCommand("retreat"));

      document.getElementById("mapCanvas").addEventListener("click", (e) => {
        if (!battle || battle.ended || !supportTargetMode) return;
        const rect = e.target.getBoundingClientRect();
        const scaleX = e.target.width / rect.width;
        const scaleY = e.target.height / rect.height;
        const cx = (e.clientX - rect.left) * scaleX;
        const cy = (e.clientY - rect.top) * scaleY;

        if (supportTargetMode === "support") {
          issueCommand("support", { x: cx, y: cy });
        } else if (supportTargetMode === "nuke") {
          detonateNukeAt(cx, cy);
        }
        supportTargetMode = false;
        document.getElementById("battleModeHint").textContent = "點選地圖據點或分隊下達指令";
      });

      document.getElementById("btnSummonGojo").addEventListener("click", summonGojoAtHome);

      // 右側分頁切換
      const tabMap = [
        ["tabBtnProd", "panelProd"],
        ["tabBtnTrust", "panelTrust"],
        ["tabBtnRd", "panelRd"]
      ];
      tabMap.forEach(([btnId, panelId]) => {
        document.getElementById(btnId).addEventListener("click", () => {
          tabMap.forEach(([b, p]) => {
            document.getElementById(b).classList.remove("active-tab");
            document.getElementById(p).classList.remove("active-panel");
          });
          document.getElementById(btnId).classList.add("active-tab");
          document.getElementById(panelId).classList.add("active-panel");
        });
      });

      // 外掛面板按鈕
      document.getElementById("btnToggleCheatCollapse").addEventListener("click", () => {
        app.cheatCollapsed = !app.cheatCollapsed;
        document.getElementById("cheatBody").style.display = app.cheatCollapsed ? "none" : "grid";
        document.getElementById("btnToggleCheatCollapse").textContent = app.cheatCollapsed ? "展開" : "收合";
      });

      document.getElementById("cheatMoney").addEventListener("click", () => {
        if (battle) battle.money += 2000;
      });
      document.getElementById("cheatRes").addEventListener("click", () => {
        if (battle) {
          battle.manpower += 50;
          battle.research += 50;
          renderSidePanels();
        }
      });
      document.getElementById("cheatTrust").addEventListener("click", () => {
        if (battle) {
          battle.trust += 200;
          renderSidePanels();
        }
      });
      document.getElementById("cheatNuke").addEventListener("click", () => {
        if (battle) {
          battle.items.nukeUnlocked = true;
          battle.items.nukeCount += 1;
          renderSidePanels();
        }
      });
      document.getElementById("cheatFlag").addEventListener("click", () => {
        if (battle && !battle.isSpectator) {
          battle.nodes.forEach((nd) => {
            if (!nd.isBase && nd.owner !== battle.playerTeam) {
              nd.owner = battle.playerTeam;
              nd.hp = 450;
              nd.capProgress = 0;
              nd.capTeam = null;
            }
          });
        }
      });
      document.getElementById("cheatGod").addEventListener("click", () => {
        app.godMode = !app.godMode;
        document.getElementById("cheatGod").textContent = app.godMode ? "無敵：開" : "無敵：關";
      });

      // 結算視窗
      document.getElementById("btnEndRestart").addEventListener("click", () => {
        launchBattleFromRoom(getCurrentRoom());
      });
      document.getElementById("btnEndLobby").addEventListener("click", () => {
        resetMatchAndEggs(true);
        showScreen("lobby");
      });

      requestAnimationFrame(mainLoop);
    })();
  </script>
</body>
</html>
