# lucasonhub.github.io
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Jogo da Desigualdade Social — Rawls</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Sora:wght@400;500;600;700&family=Source+Serif+4:ital,wght@0,400;1,400&display=swap" rel="stylesheet">
<style>
/* ============================================================
   RESET & TOKENS
============================================================ */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --font-head: 'Sora', sans-serif;
  --font-body: 'Sora', sans-serif;
  --font-serif: 'Source Serif 4', serif;

  --c-bg:       #0f1117;
  --c-surface:  #181c26;
  --c-card:     #1e2333;
  --c-border:   rgba(255,255,255,0.08);
  --c-border2:  rgba(255,255,255,0.14);

  --c-text:     #e8eaf0;
  --c-muted:    #8a90a2;
  --c-hint:     #555c70;

  --c-amber:    #f5a623;
  --c-amber-dk: #c47f0a;
  --c-amber-bg: rgba(245,166,35,0.12);
  --c-teal:     #2ec9a0;
  --c-teal-dk:  #0f8f6e;
  --c-teal-bg:  rgba(46,201,160,0.12);
  --c-red:      #f05252;
  --c-red-bg:   rgba(240,82,82,0.12);
  --c-blue:     #5b9cf6;
  --c-blue-bg:  rgba(91,156,246,0.12);
  --c-purple:   #a78bfa;
  --c-purple-bg:rgba(167,139,250,0.12);

  --radius-sm:  8px;
  --radius-md:  12px;
  --radius-lg:  18px;
  --radius-xl:  24px;

  --transition: all 0.28s cubic-bezier(0.4,0,0.2,1);
}

html { -webkit-text-size-adjust: 100%; }
body {
  font-family: var(--font-body);
  background: var(--c-bg);
  color: var(--c-text);
  min-height: 100vh;
  overflow-x: hidden;
}

/* ============================================================
   LAYOUT SHELL
============================================================ */
.app {
  max-width: 540px;
  margin: 0 auto;
  padding: 0 0 100px;
  min-height: 100vh;
}

/* ============================================================
   HEADER
============================================================ */
.header {
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(15,17,23,0.92);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--c-border);
  padding: 14px 20px 10px;
}
.header-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
}
.header-title {
  font-size: 13px;
  font-weight: 600;
  letter-spacing: 0.04em;
  color: var(--c-muted);
  text-transform: uppercase;
}
.score-pill {
  display: flex;
  align-items: center;
  gap: 6px;
  background: var(--c-amber-bg);
  border: 1px solid rgba(245,166,35,0.3);
  border-radius: 20px;
  padding: 4px 12px;
  font-size: 13px;
  font-weight: 600;
  color: var(--c-amber);
  transition: var(--transition);
}
.score-pill .score-val {
  font-size: 16px;
  font-weight: 700;
}

/* Progress bar */
.progress-wrap {
  display: flex;
  gap: 4px;
  align-items: center;
}
.progress-seg {
  flex: 1;
  height: 3px;
  border-radius: 2px;
  background: var(--c-border2);
  transition: background 0.5s ease;
  overflow: hidden;
}
.progress-seg .fill {
  height: 100%;
  width: 0;
  border-radius: 2px;
  background: var(--c-amber);
  transition: width 0.6s cubic-bezier(0.4,0,0.2,1);
}
.progress-seg.done .fill { width: 100%; background: var(--c-teal); }
.progress-seg.active .fill { width: 60%; background: var(--c-amber); }
.progress-pct {
  font-size: 11px;
  color: var(--c-muted);
  white-space: nowrap;
  min-width: 28px;
  text-align: right;
}

/* ============================================================
   PANELS
============================================================ */
.panel {
  display: none;
  padding: 24px 20px 0;
  animation: fadeSlide 0.35s cubic-bezier(0.4,0,0.2,1);
}
.panel.active { display: block; }

@keyframes fadeSlide {
  from { opacity: 0; transform: translateY(14px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* ============================================================
   TYPOGRAPHY
============================================================ */
.phase-label {
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--c-amber);
  margin-bottom: 6px;
}
.phase-title {
  font-size: 22px;
  font-weight: 700;
  line-height: 1.25;
  color: var(--c-text);
  margin-bottom: 8px;
}
.phase-sub {
  font-size: 14px;
  line-height: 1.6;
  color: var(--c-muted);
  margin-bottom: 20px;
}

/* ============================================================
   PLAYER CARDS
============================================================ */
.player-list { display: flex; flex-direction: column; gap: 10px; margin-bottom: 20px; }
.player-card {
cursor: pointer;
touch-action: manipulation;
  background: var(--c-card);
  border: 1.5px solid var(--c-border);
  border-radius: var(--radius-lg);
  padding: 16px;
  cursor: pointer;
  transition: var(--transition);
  -webkit-tap-highlight-color: transparent;
}
.player-card:hover { border-color: var(--c-border2); background: #222840; }
.player-card.selected {
  border-color: var(--c-amber);
  background: rgba(245,166,35,0.07);
  box-shadow: 0 0 0 3px rgba(245,166,35,0.15);
}
.player-card-head {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 10px;
}
.player-avatar {
  width: 44px; height: 44px;
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 20px;
  flex-shrink: 0;
}
.player-info { flex: 1; }
.player-name { font-size: 16px; font-weight: 700; margin-bottom: 2px; }
.player-label { font-size: 12px; color: var(--c-muted); }
.selected-check {
  width: 24px; height: 24px;
  border-radius: 50%;
  background: var(--c-amber);
  display: flex; align-items: center; justify-content: center;
  font-size: 13px;
  flex-shrink: 0;
  opacity: 0;
  transform: scale(0.5);
  transition: var(--transition);
}
.player-card.selected .selected-check { opacity: 1; transform: scale(1); }
.player-desc { font-size: 12px; color: var(--c-muted); line-height: 1.5; margin-bottom: 12px; }

.stat-row { display: flex; align-items: center; gap: 8px; margin: 5px 0; }
.stat-label { font-size: 11px; color: var(--c-hint); width: 82px; flex-shrink: 0; }
.stat-track { flex: 1; height: 4px; background: rgba(255,255,255,0.08); border-radius: 2px; overflow: hidden; }
.stat-fill { height: 100%; border-radius: 2px; transition: width 0.8s cubic-bezier(0.4,0,0.2,1); }
.stat-pct { font-size: 11px; color: var(--c-hint); width: 28px; text-align: right; }

/* ============================================================
   RAWLS BOX
============================================================ */
.rawls-box {
  border-left: 3px solid var(--c-amber);
  background: var(--c-amber-bg);
  border-radius: 0 var(--radius-md) var(--radius-md) 0;
  padding: 14px 16px;
  margin: 20px 0;
}
.rawls-tag {
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--c-amber);
  margin-bottom: 6px;
}
.rawls-text {
  font-family: var(--font-serif);
  font-size: 14px;
  line-height: 1.65;
  color: var(--c-text);
}

/* ============================================================
   BUTTONS
============================================================ */
.btn-row {
  display: flex;
  gap: 10px;
  margin-top: 24px;
  flex-wrap: wrap;
}
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  padding: 14px 20px;
  border-radius: var(--radius-md);
  border: 1.5px solid var(--c-border2);
  background: var(--c-card);
  color: var(--c-text);
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: var(--transition);
  -webkit-tap-highlight-color: transparent;
  min-height: 52px;
  white-space: nowrap;
}
.btn:hover { background: #252a3a; border-color: var(--c-border2); }
.btn:active { transform: scale(0.97); }
.btn-primary {
  background: var(--c-amber);
  border-color: var(--c-amber);
  color: #1a1000;
  flex: 1;
}
.btn-primary:hover { background: #f7b84a; }
.btn-primary:disabled {
  background: var(--c-hint);
  border-color: var(--c-hint);
  color: var(--c-bg);
  cursor: not-allowed;
  opacity: 0.6;
}
.btn-ghost { background: transparent; color: var(--c-muted); border-color: transparent; }
.btn-ghost:hover { background: var(--c-card); }
.btn-teal { background: var(--c-teal); border-color: var(--c-teal); color: #021a12; flex: 1; }
.btn-teal:hover { background: #48d9b4; }

/* ============================================================
   EVENT CARD
============================================================ */
.event-card {
  background: var(--c-card);
  border: 1px solid var(--c-border2);
  border-radius: var(--radius-lg);
  padding: 20px;
  margin-bottom: 16px;
}
.event-tag {
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--c-muted);
  margin-bottom: 8px;
}
.event-title {
  font-size: 17px;
  font-weight: 700;
  line-height: 1.3;
  margin-bottom: 8px;
  color: var(--c-text);
}
.event-desc {
  font-size: 13px;
  color: var(--c-muted);
  line-height: 1.6;
}

/* Impact block */
.impact-block {
  background: var(--c-surface);
  border-radius: var(--radius-lg);
  padding: 20px;
  margin-bottom: 16px;
  border: 1px solid var(--c-border);
}
.impact-header {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 12px;
}
.impact-icon {
  width: 40px; height: 40px;
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 20px;
  flex-shrink: 0;
}
.impact-name { font-size: 15px; font-weight: 700; }
.impact-tag  { font-size: 12px; color: var(--c-muted); }
.impact-narrative {
  font-family: var(--font-serif);
  font-size: 15px;
  line-height: 1.7;
  color: var(--c-text);
  margin-bottom: 14px;
}
.impact-delta-row {
  display: flex;
  align-items: center;
  gap: 10px;
}
.impact-delta {
  font-size: 22px;
  font-weight: 700;
}
.impact-badge {
  display: inline-flex;
  align-items: center;
  gap: 5px;
  padding: 5px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
}

/* ============================================================
   MOBILITY PANEL — individual
============================================================ */
.mobility-outcome {
  background: var(--c-card);
  border: 1px solid var(--c-border);
  border-radius: var(--radius-md);
  padding: 16px;
  margin-bottom: 10px;
}
.mobility-label { font-size: 13px; font-weight: 600; margin-bottom: 10px; color: var(--c-text); }
.mobility-track { height: 10px; background: rgba(255,255,255,0.06); border-radius: 5px; overflow: hidden; margin-bottom: 6px; }
.mobility-fill { height: 100%; border-radius: 5px; transition: width 1s cubic-bezier(0.4,0,0.2,1); }
.mobility-meta { display: flex; justify-content: space-between; font-size: 12px; color: var(--c-muted); }

/* ============================================================
   VEIL INTERMEDIATE SCREEN
============================================================ */
.veil-screen {
  text-align: center;
  padding: 30px 0;
}
.veil-icon {
  font-size: 56px;
  margin-bottom: 16px;
  display: block;
}
.veil-title {
  font-size: 22px;
  font-weight: 700;
  line-height: 1.3;
  margin-bottom: 14px;
}
.veil-text {
  font-family: var(--font-serif);
  font-size: 15px;
  line-height: 1.7;
  color: var(--c-muted);
  margin-bottom: 20px;
}

/* ============================================================
   POLICY CARDS
============================================================ */
.policy-card {
  background: var(--c-card);
  border: 1.5px solid var(--c-border);
  border-radius: var(--radius-lg);
  padding: 18px;
  margin-bottom: 12px;
  transition: var(--transition);
}
.policy-card:hover { border-color: var(--c-border2); }
.policy-num {
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--c-teal);
  margin-bottom: 6px;
}
.policy-title { font-size: 15px; font-weight: 700; margin-bottom: 6px; }
.policy-desc { font-size: 13px; color: var(--c-muted); line-height: 1.55; margin-bottom: 10px; }
.policy-rawls {
  font-family: var(--font-serif);
  font-size: 13px;
  line-height: 1.6;
  color: var(--c-teal);
  font-style: italic;
  padding: 8px 12px;
  background: var(--c-teal-bg);
  border-radius: var(--radius-sm);
  margin-bottom: 10px;
}

/* ============================================================
   CONCLUSION
============================================================ */
.principle-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 16px; }
.principle-card {
  background: var(--c-card);
  border: 1px solid var(--c-border);
  border-radius: var(--radius-md);
  padding: 14px;
}
.principle-num { font-size: 10px; font-weight: 700; letter-spacing: 0.08em; text-transform: uppercase; margin-bottom: 6px; }
.principle-name { font-size: 13px; font-weight: 700; margin-bottom: 5px; }
.principle-desc { font-size: 11px; color: var(--c-muted); line-height: 1.5; }
.conclusion-personal {
  background: var(--c-surface);
  border-radius: var(--radius-lg);
  padding: 20px;
  margin-bottom: 16px;
  border: 1px solid var(--c-border);
}
.conclusion-avatar {
  font-size: 36px;
  margin-bottom: 10px;
  display: block;
}
.conclusion-text {
  font-family: var(--font-serif);
  font-size: 15px;
  line-height: 1.7;
  color: var(--c-text);
}
.final-score-block {
  text-align: center;
  background: var(--c-amber-bg);
  border: 1px solid rgba(245,166,35,0.3);
  border-radius: var(--radius-lg);
  padding: 20px;
  margin-bottom: 16px;
}
.final-score-label { font-size: 12px; color: var(--c-amber); font-weight: 600; margin-bottom: 4px; }
.final-score-val { font-size: 42px; font-weight: 700; color: var(--c-amber); line-height: 1; margin-bottom: 6px; }
.final-score-sub { font-size: 12px; color: var(--c-muted); }
.ref-block {
  font-size: 12px;
  color: var(--c-hint);
  line-height: 1.5;
  padding: 12px 14px;
  border: 1px solid var(--c-border);
  border-radius: var(--radius-md);
  margin-bottom: 16px;
}

/* ============================================================
   UTILITY
============================================================ */
.divider { height: 1px; background: var(--c-border); margin: 20px 0; }
.spacer-sm { height: 8px; }
.spacer-md { height: 16px; }

/* Pulse animation for score update */
@keyframes scorePulse {
  0%   { transform: scale(1); }
  40%  { transform: scale(1.18); }
  100% { transform: scale(1); }
}
.pulse { animation: scorePulse 0.4s cubic-bezier(0.4,0,0.2,1); }

/* Screen reader only */
.sr-only {
  position: absolute; width: 1px; height: 1px;
  overflow: hidden; clip: rect(0,0,0,0); white-space: nowrap;
}
</style>
</head>
<body>

<div class="app" id="app">

  <!-- HEADER -->
  <header class="header">
    <div class="header-top">
      <span class="header-title">Desigualdade Social · Rawls</span>
      <div class="score-pill" id="score-pill" style="display:none">
        <span>Pontuação</span>
        <span class="score-val" id="score-display">0</span>
      </div>
    </div>
    <div class="progress-wrap">
      <div class="progress-seg" id="seg-0"><div class="fill"></div></div>
      <div class="progress-seg" id="seg-1"><div class="fill"></div></div>
      <div class="progress-seg" id="seg-2"><div class="fill"></div></div>
      <div class="progress-seg" id="seg-3"><div class="fill"></div></div>
      <div class="progress-seg" id="seg-4"><div class="fill"></div></div>
      <span class="progress-pct" id="progress-pct">0%</span>
    </div>
  </header>

  <!-- ===================================================
       FASE 0 — SELEÇÃO DE PERFIL
  =================================================== -->
  <section class="panel active" id="panel-0" aria-label="Seleção de perfil">
    <p class="phase-label">Fase 1 de 5</p>
    <h1 class="phase-title">Escolha seu perfil social</h1>
    <p class="phase-sub">Quatro estudantes assumem condições socioeconômicas distintas no Brasil contemporâneo. Esta é a realidade antes do véu da ignorância.</p>

    <div class="player-list" id="player-list" role="radiogroup" aria-label="Perfis disponíveis"></div>

    <div class="rawls-box">
      <div class="rawls-tag">Conexão Rawlsiana</div>
      <p class="rawls-text">Esta é a posição pré-original: as desigualdades são visíveis antes de qualquer deliberação. Renda, escolaridade, moradia e oportunidades são distribuídas assimetricamente desde o nascimento.</p>
    </div>

    <div class="btn-row">
      <button class="btn btn-primary" id="btn-0-next" onclick="goTo(1)" disabled>
        Avançar →
      </button>
    </div>
  </section>

  <!-- ===================================================
       FASE 1 — EVENTO ESTRUTURAL
  =================================================== -->
  <section class="panel" id="panel-1" aria-label="Evento estrutural">
    <p class="phase-label">Fase 2 de 5</p>
    <h1 class="phase-title">Evento estrutural</h1>
    <p class="phase-sub">Um acontecimento político-econômico afeta toda a sociedade. Descubra como ele impacta sua trajetória específica.</p>

    <div id="event-display"></div>

    <div class="rawls-box">
      <div class="rawls-tag">Princípio da Diferença</div>
      <p class="rawls-text">Para Rawls, desigualdades só se justificam se beneficiam os membros menos favorecidos. Este evento aprofunda ou atenua as disparidades existentes?</p>
    </div>

    <div class="btn-row">
      <button class="btn btn-ghost" onclick="goTo(0)">← Voltar</button>
      <button class="btn btn-primary" onclick="goTo(2)">Ver minha trajetória →</button>
    </div>
  </section>

  <!-- ===================================================
       FASE 2 — MOBILIDADE SOCIAL
  =================================================== -->
  <section class="panel" id="panel-2" aria-label="Mobilidade social">
    <p class="phase-label">Fase 3 de 5</p>
    <h1 class="phase-title">Sua mobilidade social</h1>
    <p class="phase-sub">Veja as probabilidades reais de acesso a direitos fundamentais com base no seu perfil e no evento ocorrido.</p>

    <div id="mobility-display"></div>

    <div class="rawls-box">
      <div class="rawls-tag">Posição Original</div>
      <p class="rawls-text">Se você não soubesse em qual perfil nasceria, qual estrutura social escolheria? Esta é a questão central de <em>Uma Teoria da Justiça</em> (Rawls, 1971).</p>
    </div>

    <div class="btn-row">
      <button class="btn btn-ghost" onclick="goTo(1)">← Voltar</button>
      <button class="btn btn-primary" onclick="goTo(3)">Cruzar o véu →</button>
    </div>
  </section>

  <!-- ===================================================
       FASE 3 — VÉU DA IGNORÂNCIA (tela intermediária + políticas)
  =================================================== -->
  <section class="panel" id="panel-3" aria-label="Véu da ignorância">
    <div id="veil-display"></div>
  </section>

  <!-- ===================================================
       FASE 4 — CONCLUSÃO
  =================================================== -->
  <section class="panel" id="panel-4" aria-label="Conclusão">
    <p class="phase-label">Conclusão</p>
    <h1 class="phase-title">Justiça como equidade</h1>
    <p class="phase-sub">A dinâmica demonstrou os princípios rawlsianos em operação no contexto da desigualdade brasileira.</p>

    <div id="conclusion-display"></div>

    <div class="btn-row">
      <button class="btn btn-ghost" onclick="goTo(3)">← Voltar</button>
      <button class="btn btn-teal" onclick="resetGame()">↺ Jogar novamente</button>
    </div>
    <div class="btn-row" style="margin-top:8px">
      <button class="btn" onclick="goTo(0)" style="flex:1">Escolher outro perfil</button>
    </div>
  </section>

</div><!-- /app -->

<script>
/* ============================================================
   DATA
============================================================ */

const PLAYERS = [
  {
    id: 0,
    name: "Sofia",
    label: "Alta renda",
    emoji: "👩‍💼",
    cor: "#f5a623",
    renda: 90, escolaridade: 88, moradia: 92, oportunidades: 87,
    desc: "Escola particular, bairro nobre, família com pós-graduação. Rede de contatos consolidada desde a infância.",
    conclusao: "Você iniciou o jogo com amplo acesso a renda, educação e saúde. Mesmo diante de eventos negativos, sua posição social funcionou como uma rede de proteção invisível — o que Rawls chamaria de vantagem arbitrária do ponto de vista moral.",
    mobilidade: { universidade: 82, emprego: 78, saude: 90, univ_label: "Muito provável", emp_label: "Alta probabilidade", sau_label: "Plano privado premium" }
  },
  {
    id: 1,
    name: "Marcos",
    label: "Classe média",
    emoji: "👨‍🏫",
    cor: "#5b9cf6",
    renda: 55, escolaridade: 60, moradia: 58, oportunidades: 52,
    desc: "Escola pública de qualidade, apartamento alugado, pais com ensino médio. Acesso instável a bens e serviços.",
    conclusao: "Você vivenciou a instabilidade característica da classe média brasileira: próxima o suficiente da vulnerabilidade para sentir cada turbulência estrutural, mas sem a proteção da alta renda. Rawls diria que esta posição revela a insuficiência de oportunidades meramente formais.",
    mobilidade: { universidade: 45, emprego: 48, saude: 55, univ_label: "Possível, com esforço", emp_label: "Possível", sau_label: "SUS + plano básico" }
  },
  {
    id: 2,
    name: "Luana",
    label: "Baixa renda",
    emoji: "👩‍🏭",
    cor: "#f87171",
    renda: 28, escolaridade: 30, moradia: 25, oportunidades: 22,
    desc: "Escola pública periférica, casa em área de risco, sem rede de apoio. Enfrenta barreiras estruturais cotidianas.",
    conclusao: "Sua trajetória evidenciou como desigualdades de partida se reproduzem e aprofundam ao longo da vida. Pequenas mudanças estruturais tiveram consequências desproporcionais sobre suas oportunidades — exatamente o cenário que o princípio da diferença de Rawls busca remediar.",
    mobilidade: { universidade: 14, emprego: 16, saude: 22, univ_label: "Muito difícil", emp_label: "Trabalho informal", sau_label: "Dependente do SUS" }
  },
  {
    id: 3,
    name: "Pedro",
    label: "Extrema pobreza",
    emoji: "👦",
    cor: "#a78bfa",
    renda: 8, escolaridade: 12, moradia: 10, oportunidades: 6,
    desc: "Evasão escolar, habitação precária, trabalho informal desde os 14 anos. Ausência de redes de segurança.",
    conclusao: "Você iniciou em situação de extrema vulnerabilidade. Cada evento estrutural ampliou as distâncias já existentes. Para Rawls, nenhuma sociedade justa poderia permitir que fatores arbitrários como local de nascimento determinassem trajetórias tão radicalmente distintas.",
    mobilidade: { universidade: 4, emprego: 6, saude: 8, univ_label: "Improvável", emp_label: "Trabalho precário", sau_label: "Sem acesso regular" }
  }
];

const EVENTS = [
  {
    title: "Corte no financiamento da educação pública",
    desc: "O governo federal reduz em 20% o orçamento das universidades federais e escolas técnicas estaduais.",
    efeitos: [+2, -5, -18, -22],
    narrativas: [
      "Você estuda em escola particular e planeja universidade privada. O corte não afeta diretamente sua trajetória educacional.",
      "Você depende das escolas técnicas estaduais para qualificação profissional. O corte reduz as vagas e a qualidade dos cursos que você considerava.",
      "A escola pública onde você estuda já opera com recursos escassos. O corte agrava superlotação e falta de professores, comprometendo sua preparação para o ENEM.",
      "Você já havia abandonado a escola. O corte nas políticas de reinserção escolar elimina programas que poderiam trazê-lo de volta."
    ],
    badges: [
      { texto: "Impacto mínimo", tipo: "positivo" },
      { texto: "Perda moderada", tipo: "moderado" },
      { texto: "Perda severa", tipo: "grave" },
      { texto: "Exclusão aprofundada", tipo: "grave" }
    ]
  },
  {
    title: "Expansão do Programa Bolsa Família",
    desc: "Transferência de renda para famílias vulneráveis aumenta 40%, com condicionalidades de frequência escolar.",
    efeitos: [0, +3, +12, +20],
    narrativas: [
      "Sua família não se enquadra nos critérios de elegibilidade. O programa não afeta diretamente sua situação, embora você reconheça seus efeitos sociais mais amplos.",
      "Você não recebe o benefício, mas clientes do comércio local onde trabalha um familiar têm mais poder de compra. O impacto é indireto e positivo.",
      "Você passa a receber o benefício, o que alivia parte da pressão financeira e permite que você continue estudando sem precisar trabalhar em tempo integral.",
      "O benefício representa uma mudança concreta na sua vida: você consegue garantir alimentação regular e mantém seu filho menor na escola graças à condicionalidade."
    ],
    badges: [
      { texto: "Sem efeito direto", tipo: "moderado" },
      { texto: "Leve benefício indireto", tipo: "positivo" },
      { texto: "Benefício significativo", tipo: "positivo" },
      { texto: "Impacto transformador", tipo: "positivo" }
    ]
  },
  {
    title: "Privatização de hospitais públicos regionais",
    desc: "Três hospitais públicos da periferia são convertidos em unidades privadas. O atendimento universal passa a exigir plano de saúde.",
    efeitos: [+1, -8, -25, -30],
    narrativas: [
      "Sua família possui plano de saúde privado abrangente. Os hospitais privatizados passam a oferecer um serviço que você já utilizava em clínicas particulares. Impacto praticamente nulo.",
      "Você tinha um plano de saúde básico que cobria consultas simples. Com a privatização, o plano deixa de cobrir o hospital mais próximo, forçando deslocamentos longos ou gastos maiores.",
      "Você dependia inteiramente do hospital público do bairro para atendimento. Sem plano de saúde e sem condições de pagar, você passa a enfrentar filas enormes no único hospital público que sobrou na região.",
      "Sem qualquer cobertura de saúde, a privatização representa abandono institucional completo. O hospital mais próximo agora exige pagamento ou plano que você jamais poderá contratar."
    ],
    badges: [
      { texto: "Sem alteração", tipo: "positivo" },
      { texto: "Acesso comprometido", tipo: "moderado" },
      { texto: "Exclusão do sistema", tipo: "grave" },
      { texto: "Abandono institucional", tipo: "grave" }
    ]
  }
];

const POLICIES = [
  {
    title: "Cotas socioeconômicas nas universidades federais",
    desc: "Reserva de 50% das vagas para estudantes de escolas públicas, com recorte de renda familiar.",
    rawls: "Aplicação direta do princípio da diferença: maximiza oportunidades para os menos favorecidos sem eliminar o mérito como critério de seleção."
  },
  {
    title: "Imposto progressivo sobre grandes heranças",
    desc: "Tributação de 15–35% sobre heranças acima de R$ 1 milhão, com destinação ao fundo nacional de educação.",
    rawls: "Rawls não exige igualdade de resultados, mas exige igualdade equitativa de oportunidades. Heranças concentradas perpetuam vantagens arbitrárias."
  },
  {
    title: "Renda básica universal incondicional",
    desc: "Transferência mensal de R$ 600 a todos os cidadãos, financiada por imposto sobre riqueza financeira.",
    rawls: "Princípio maximin: maximize a situação do grupo em pior posição. Seria esta política escolhida sob o véu da ignorância por alguém racional?"
  }
];

/* ============================================================
   STATE
============================================================ */
let selectedPlayer = null;
let currentEvent   = null;
let playerScore    = 0;
let currentPhase   = 0;
let veilRevealed   = false; // controls two-step veil phase

/* ============================================================
   NAVIGATION
============================================================ */
function goTo(n) {
  // Validation
  if (n === 1 && selectedPlayer === null) {
    shakeBtn('btn-0-next');
    return;
  }

  // Hide all panels
  document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));

  // Show target
  document.getElementById('panel-' + n).classList.add('active');
  currentPhase = n;

  // Update progress
  updateProgress(n);

  // Scroll to top
  window.scrollTo({ top: 0, behavior: 'smooth' });

  // Build content
  if (n === 1) buildEvent();
  if (n === 2) buildMobility();
  if (n === 3) { veilRevealed = false; buildVeil(); }
  if (n === 4) buildConclusion();
}

/* ============================================================
   PROGRESS BAR
============================================================ */
const PCT = [0, 20, 40, 60, 80, 100];

function updateProgress(phase) {
  for (let i = 0; i < 5; i++) {
    const seg = document.getElementById('seg-' + i);
    seg.classList.remove('done', 'active');
    const fill = seg.querySelector('.fill');
    fill.style.width = '0';
    if (i < phase) { seg.classList.add('done'); fill.style.width = '100%'; }
    else if (i === phase) { seg.classList.add('active'); fill.style.width = '60%'; }
  }
  document.getElementById('progress-pct').textContent = PCT[phase] + '%';
}

/* ============================================================
   SCORE
============================================================ */
function setScore(val) {
  playerScore = Math.max(0, Math.min(100, val));
  const disp = document.getElementById('score-display');
  disp.textContent = playerScore;
  disp.classList.remove('pulse');
  void disp.offsetWidth; // reflow
  disp.classList.add('pulse');
}

function showScore(show) {
  document.getElementById('score-pill').style.display = show ? 'flex' : 'none';
}

/* ============================================================
   SHAKE ANIMATION
============================================================ */
function shakeBtn(id) {
  const btn = document.getElementById(id);
  btn.style.animation = 'none';
  void btn.offsetWidth;
  btn.style.animation = 'shake 0.4s ease';
}

/* ============================================================
   PHASE 0 — PLAYER SELECTION
============================================================ */
function buildPlayerList() {
  const list = document.getElementById('player-list');
  list.innerHTML = '';

  PLAYERS.forEach((p, i) => {
    const isSelected = selectedPlayer === i;
    const stats = [
      { label: 'Renda',        val: p.renda,        color: '#f5a623' },
      { label: 'Escolaridade', val: p.escolaridade,  color: '#2ec9a0' },
      { label: 'Moradia',      val: p.moradia,       color: '#5b9cf6' },
      { label: 'Oportunidades',val: p.oportunidades, color: '#a78bfa' }
    ];

    const card = document.createElement('div');
    card.className = 'player-card' + (isSelected ? ' selected' : '');
    card.setAttribute('role', 'radio');
    card.setAttribute('aria-checked', isSelected ? 'true' : 'false');
    card.setAttribute('tabindex', '0');
    card.addEventListener("click", () => selectPlayer(i));
card.addEventListener("touchstart", () => selectPlayer(i));
    card.onkeydown = (e) => { if (e.key === 'Enter' || e.key === ' ') selectPlayer(i); };

    card.innerHTML = `
      <div class="player-card-head">
        <div class="player-avatar" style="background:${p.cor}22">${p.emoji}</div>
        <div class="player-info">
          <div class="player-name" style="color:${p.cor}">${p.name}</div>
          <div class="player-label">${p.label}</div>
        </div>
        <div class="selected-check" aria-hidden="true">✓</div>
      </div>
      <div class="player-desc">${p.desc}</div>
      ${stats.map(s => `
        <div class="stat-row">
          <span class="stat-label">${s.label}</span>
          <div class="stat-track">
            <div class="stat-fill" style="width:${s.val}%;background:${s.color}"></div>
          </div>
          <span class="stat-pct">${s.val}</span>
        </div>`).join('')}
    `;
    list.appendChild(card);
  });
}

function selectPlayer(i) {
  selectedPlayer = i;
  const p = PLAYERS[i];
  playerScore = p.oportunidades;
  showScore(true);
  setScore(playerScore);
  buildPlayerList();
  document.getElementById('btn-0-next').disabled = false;
}

/* ============================================================
   PHASE 1 — EVENT
============================================================ */
function buildEvent() {
  // Pick random event (different from last if possible)
  currentEvent = EVENTS[Math.floor(Math.random() * EVENTS.length)];
  const ev = currentEvent;
  const p  = PLAYERS[selectedPlayer];
  const delta = ev.efeitos[selectedPlayer];
  const badge = ev.badges[selectedPlayer];
  const narrative = ev.narrativas[selectedPlayer];

  // Update score
  const newScore = playerScore + delta;
  setScore(newScore);
  playerScore = Math.max(0, Math.min(100, newScore));

  // Determine icon
  let icon, iconBg, deltaColor;
  if (delta > 0)       { icon = '🟢'; iconBg = 'var(--c-teal-bg)';   deltaColor = 'var(--c-teal)'; }
  else if (delta >= -8) { icon = '🟡'; iconBg = 'var(--c-amber-bg)'; deltaColor = 'var(--c-amber)'; }
  else                  { icon = '🔴'; iconBg = 'var(--c-red-bg)';   deltaColor = 'var(--c-red)'; }

  let badgeStyle;
  if (badge.tipo === 'positivo')  badgeStyle = `background:var(--c-teal-bg);color:var(--c-teal);border:1px solid rgba(46,201,160,0.3)`;
  else if (badge.tipo === 'moderado') badgeStyle = `background:var(--c-amber-bg);color:var(--c-amber);border:1px solid rgba(245,166,35,0.3)`;
  else badgeStyle = `background:var(--c-red-bg);color:var(--c-red);border:1px solid rgba(240,82,82,0.3)`;

  const sign = delta >= 0 ? '+' : '';

  document.getElementById('event-display').innerHTML = `
    <div class="event-card">
      <div class="event-tag">Evento sorteado</div>
      <div class="event-title">${ev.title}</div>
      <div class="event-desc">${ev.desc}</div>
    </div>

    <div class="impact-block">
      <div class="impact-header">
        <div class="impact-icon" style="background:${p.cor}22">${p.emoji}</div>
        <div>
          <div class="impact-name" style="color:${p.cor}">Você é ${p.name}</div>
          <div class="impact-tag">${p.label}</div>
        </div>
      </div>

      <div class="impact-narrative">${narrative}</div>

      <div class="impact-delta-row">
        <div class="impact-delta" style="color:${deltaColor}">${sign}${delta} pts</div>
        <div class="impact-badge" style="${badgeStyle}">${icon} ${badge.texto}</div>
      </div>
    </div>
  `;
}

/* ============================================================
   PHASE 2 — MOBILITY (individual)
============================================================ */
function buildMobility() {
  const p = PLAYERS[selectedPlayer];
  const ev = currentEvent;
  const delta = ev ? ev.efeitos[selectedPlayer] : 0;

  const outcomes = [
    { label: 'Acesso à universidade', base: p.mobilidade.universidade, qual: p.mobilidade.univ_label },
    { label: 'Emprego qualificado',   base: p.mobilidade.emprego,       qual: p.mobilidade.emp_label },
    { label: 'Saúde',                 base: p.mobilidade.saude,         qual: p.mobilidade.sau_label }
  ];

  const barColor = p.cor;

  document.getElementById('mobility-display').innerHTML = `
    <div class="impact-block" style="margin-bottom:16px">
      <div class="impact-header">
        <div class="impact-icon" style="background:${p.cor}22">${p.emoji}</div>
        <div>
          <div class="impact-name" style="color:${p.cor}">${p.name} — ${p.label}</div>
          <div class="impact-tag">Probabilidade de acesso a direitos fundamentais</div>
        </div>
      </div>
    </div>

    ${outcomes.map(o => {
      const adj = Math.max(2, Math.min(98, o.base + Math.round(delta * 0.25)));
      let barColor2 = barColor;
      if (adj < 20) barColor2 = 'var(--c-red)';
      else if (adj < 45) barColor2 = 'var(--c-amber)';
      return `
        <div class="mobility-outcome">
          <div class="mobility-label">${o.label}</div>
          <div class="mobility-track">
            <div class="mobility-fill" style="width:${adj}%;background:${barColor2}"></div>
          </div>
          <div class="mobility-meta">
            <span>${o.qual}</span>
            <span>${adj}%</span>
          </div>
        </div>`;
    }).join('')}
  `;
}

/* ============================================================
   PHASE 3 — VÉU DA IGNORÂNCIA (two-step)
============================================================ */
function buildVeil() {
  const div = document.getElementById('veil-display');

  if (!veilRevealed) {
    // Step 1: Intermediate screen
    div.innerHTML = `
      <div class="veil-screen">
        <span class="veil-icon">🎭</span>
        <h1 class="veil-title">Cruzando o Véu da Ignorância</h1>
        <p class="veil-text">
          Até agora você vivenciou a trajetória de um único indivíduo em uma estrutura social específica.
          <br><br>
          Agora imagine que você não sabe qual posição social ocupará ao nascer. Você pode ser Sofia, Marcos, Luana ou Pedro — com igual probabilidade.
          <br><br>
          Por trás desse véu, que estrutura social você escolheria?
        </p>
        <div class="rawls-box" style="text-align:left">
          <div class="rawls-tag">John Rawls — Uma Teoria da Justiça, 1971</div>
          <p class="rawls-text">Princípios de justiça são aqueles que pessoas racionais escolheriam em uma posição original de igualdade, sem conhecer seu lugar na sociedade, sua classe ou status social, ou a sua sorte na distribuição de ativos naturais.</p>
        </div>
        <div class="btn-row">
          <button class="btn btn-ghost" onclick="goTo(2)">← Voltar</button>
          <button class="btn btn-primary" onclick="revealPolicies()">Ver as políticas →</button>
        </div>
      </div>`;
  } else {
    // Step 2: Policies (identity hidden)
    div.innerHTML = `
      <p class="phase-label">Fase 4 de 5</p>
      <h1 class="phase-title">Escolha uma política pública</h1>
      <p class="phase-sub">Sem saber seu perfil, qual dessas políticas você escolheria para estruturar a sociedade?</p>

      ${POLICIES.map((pol, i) => `
        <div class="policy-card">
          <div class="policy-num">Política ${i + 1}</div>
          <div class="policy-title">${pol.title}</div>
          <div class="policy-desc">${pol.desc}</div>
          <div class="policy-rawls">${pol.rawls}</div>
          <button class="btn btn-ghost" style="font-size:13px;padding:10px 14px"
            onclick="window.sendPrompt && sendPrompt('Na dinâmica rawlsiana, a política \\'${pol.title}\\' seria escolhida sob o véu da ignorância? Explique com base nos dois princípios de Rawls.')">
            Analisar com Rawls ↗
          </button>
        </div>`).join('')}

      <div class="btn-row">
        <button class="btn btn-ghost" onclick="buildVeil(); veilRevealed=false;">← Voltar</button>
        <button class="btn btn-primary" onclick="goTo(4)">Ver conclusão →</button>
      </div>`;
  }
}

function revealPolicies() {
  veilRevealed = true;
  buildVeil();
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

/* ============================================================
   PHASE 4 — CONCLUSION
============================================================ */
function buildConclusion() {
  const p = PLAYERS[selectedPlayer];
  const finalScore = playerScore;

  let scoreComment;
  if (finalScore >= 80)     scoreComment = 'Pontuação alta — posição de privilégio estrutural';
  else if (finalScore >= 50) scoreComment = 'Pontuação média — vulnerabilidade moderada';
  else if (finalScore >= 25) scoreComment = 'Pontuação baixa — desvantagem estrutural significativa';
  else                       scoreComment = 'Pontuação muito baixa — exclusão estrutural severa';

  document.getElementById('conclusion-display').innerHTML = `
    <div class="conclusion-personal">
      <span class="conclusion-avatar">${p.emoji}</span>
      <div style="font-size:13px;font-weight:600;color:${p.cor};margin-bottom:8px">${p.name} — ${p.label}</div>
      <p class="conclusion-text">${p.conclusao}</p>
    </div>

    <div class="final-score-block">
      <div class="final-score-label">Pontuação final</div>
      <div class="final-score-val">${finalScore}</div>
      <div class="final-score-sub">${scoreComment}</div>
    </div>

    <div class="principle-grid">
      <div class="principle-card">
        <div class="principle-num" style="color:var(--c-amber)">Princípio 1</div>
        <div class="principle-name">Liberdades iguais</div>
        <div class="principle-desc">O mais amplo sistema de liberdades básicas compatível com liberdades semelhantes para todos.</div>
      </div>
      <div class="principle-card">
        <div class="principle-num" style="color:var(--c-teal)">Princípio 2</div>
        <div class="principle-name">Diferença</div>
        <div class="principle-desc">Desigualdades só se justificam se beneficiam os membros menos favorecidos da sociedade.</div>
      </div>
    </div>

    <div class="rawls-box">
      <div class="rawls-tag">Brasil e Rawls</div>
      <p class="rawls-text">A dinâmica evidencia que o Brasil ocupa uma posição contrafactual ao modelo rawlsiano: estruturas institucionais tendem a reproduzir — e frequentemente aprofundar — as desigualdades de partida, em vez de maximizar a posição dos menos favorecidos.</p>
    </div>

    <div class="ref-block">
      <strong>Referência central:</strong> Rawls, J. (1971). <em>A Theory of Justice</em>. Harvard University Press.<br>
      <strong>Leitura complementar:</strong> Sandel, M. (2009). <em>Justice: What's the Right Thing to Do?</em>. Farrar, Straus and Giroux.
    </div>

    <button class="btn" style="width:100%;margin-bottom:8px;font-size:13px"
      onclick="window.sendPrompt && sendPrompt('Quais são as principais críticas à teoria da justiça de Rawls por parte de autores como Nozick, Sen e Sandel?')">
      Explorar críticas à teoria de Rawls ↗
    </button>
  `;
}

/* ============================================================
   RESET
============================================================ */
function resetGame() {
  selectedPlayer = null;
  currentEvent   = null;
  playerScore    = 0;
  veilRevealed   = false;
  showScore(false);
  document.getElementById('btn-0-next').disabled = true;
  buildPlayerList();
  goTo(0);
}

/* ============================================================
   SHAKE KEYFRAME (injected)
============================================================ */
(function() {
  const style = document.createElement('style');
  style.textContent = `
    @keyframes shake {
      0%,100%{transform:translateX(0)}
      20%{transform:translateX(-6px)}
      40%{transform:translateX(6px)}
      60%{transform:translateX(-4px)}
      80%{transform:translateX(4px)}
    }
  `;
  document.head.appendChild(style);
})();

/* ============================================================
   INIT
============================================================ */
buildPlayerList();
updateProgress(0);
document.addEventListener("DOMContentLoaded", () => {
    buildPlayerList();
    updateProgress(0);
});
</script>
</body>
</html>
