<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>KosKeuangan Pro — Dashboard Keuangan Kos</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
:root {
  --green: #00C896;
  --green-dark: #009E78;
  --green-dim: #00C89622;
  --green-light: #E0FAF4;
  --red: #FF5252;
  --red-light: #FFF0F0;
  --red-dim: #FF525222;
  --amber: #FFB020;
  --amber-light: #FFF8E6;
  --blue: #4F8EF7;
  --blue-light: #EDF3FF;
  --purple: #9B6DFF;
  --purple-light: #F3EEFF;
  --bg: #0F1117;
  --bg2: #181C27;
  --bg3: #1E2335;
  --card: #1E2335;
  --card2: #252B3F;
  --border: #2E3650;
  --text: #F0F2FA;
  --text-muted: #8B92AA;
  --text-hint: #5A6180;
  --radius: 16px;
  --radius-sm: 10px;
  --radius-xs: 6px;
  --font: 'Plus Jakarta Sans', sans-serif;
  --mono: 'JetBrains Mono', monospace;
  --shadow: 0 4px 24px rgba(0,0,0,0.3);
  --shadow-sm: 0 2px 8px rgba(0,0,0,0.2);
}

* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: var(--font);
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  font-size: 14px;
  overflow-x: hidden;
}

/* ========= SCROLLBAR ========= */
::-webkit-scrollbar { width: 4px; height: 4px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }

/* ========= NAV ========= */
.bottom-nav {
  position: fixed;
  bottom: 0; left: 0; right: 0;
  background: var(--bg2);
  border-top: 1px solid var(--border);
  display: flex;
  z-index: 90;
  padding-bottom: env(safe-area-inset-bottom, 0);
}
.nav-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3px;
  padding: 10px 4px;
  cursor: pointer;
  border: none;
  background: transparent;
  color: var(--text-hint);
  font-family: var(--font);
  font-size: 10px;
  font-weight: 500;
  transition: color 0.2s;
  position: relative;
}
.nav-item.active { color: var(--green); }
.nav-item .nav-icon { font-size: 20px; line-height: 1; }
.nav-item.active::before {
  content: '';
  position: absolute;
  top: 0; left: 50%;
  transform: translateX(-50%);
  width: 32px; height: 2px;
  background: var(--green);
  border-radius: 0 0 4px 4px;
}

/* ========= PAGES ========= */
.page { display: none; padding-bottom: 90px; }
.page.active { display: block; }

/* ========= HEADER ========= */
.page-header {
  background: linear-gradient(180deg, var(--bg2) 0%, transparent 100%);
  padding: 20px 16px 16px;
  position: sticky;
  top: 0;
  z-index: 20;
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
}
.page-header-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.page-title {
  font-size: 20px;
  font-weight: 800;
  letter-spacing: -0.5px;
}
.page-title span { color: var(--green); }

/* ========= MONTH NAV ========= */
.month-nav {
  display: flex;
  align-items: center;
  gap: 10px;
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 50px;
  padding: 4px;
}
.month-nav-btn {
  width: 28px; height: 28px;
  border-radius: 50%;
  border: none;
  background: var(--card2);
  color: var(--text);
  cursor: pointer;
  font-size: 14px;
  display: flex; align-items: center; justify-content: center;
  transition: all 0.15s;
}
.month-nav-btn:hover { background: var(--border); }
.month-nav-label {
  font-size: 12px;
  font-weight: 600;
  color: var(--text);
  min-width: 70px;
  text-align: center;
  cursor: pointer;
}

/* ========= CARDS ========= */
.card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
}

/* ========= MAIN CONTENT ========= */
.main { padding: 0 16px; }

/* ========= BALANCE HERO ========= */
.balance-hero {
  background: linear-gradient(135deg, #1A2A3E 0%, #0D1F2D 100%);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 20px;
  margin: 0 16px 16px;
  position: relative;
  overflow: hidden;
}
.balance-hero::before {
  content: '';
  position: absolute;
  top: -40px; right: -40px;
  width: 150px; height: 150px;
  background: var(--green-dim);
  border-radius: 50%;
  filter: blur(40px);
}
.balance-label {
  font-size: 11px;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.8px;
  font-weight: 600;
  margin-bottom: 6px;
}
.balance-amount {
  font-size: 32px;
  font-weight: 800;
  letter-spacing: -1.5px;
  color: var(--text);
  font-family: var(--mono);
  margin-bottom: 16px;
}
.balance-amount.danger { color: var(--red); }
.balance-amount.warn { color: var(--amber); }
.balance-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}
.balance-item {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.07);
  border-radius: var(--radius-sm);
  padding: 10px 12px;
}
.balance-item-label { font-size: 10px; color: var(--text-muted); margin-bottom: 3px; }
.balance-item-val { font-size: 15px; font-weight: 700; font-family: var(--mono); }
.balance-item-val.inc { color: var(--green); }
.balance-item-val.exp { color: var(--red); }

/* ========= PROGRESS ========= */
.progress-wrap { margin-top: 14px; }
.progress-meta {
  display: flex;
  justify-content: space-between;
  font-size: 10px;
  color: var(--text-muted);
  margin-bottom: 5px;
}
.progress-bar {
  background: var(--bg3);
  border-radius: 4px;
  height: 6px;
  overflow: hidden;
}
.progress-fill {
  height: 100%;
  border-radius: 4px;
  background: var(--green);
  transition: width 0.6s cubic-bezier(.4,0,.2,1);
}
.progress-fill.warn { background: var(--amber); }
.progress-fill.danger { background: var(--red); }

/* ========= STAT GRID ========= */
.stat-grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 8px;
  margin: 0 16px 16px;
}
.stat-card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 12px 10px;
  text-align: center;
}
.stat-val { font-size: 14px; font-weight: 700; font-family: var(--mono); color: var(--text); }
.stat-label { font-size: 10px; color: var(--text-muted); margin-top: 3px; }

/* ========= ALERT BOX ========= */
.alert-box {
  display: none;
  margin: 0 16px 16px;
  padding: 12px 14px;
  border-radius: var(--radius-sm);
  align-items: flex-start;
  gap: 10px;
}
.alert-box.show { display: flex; }
.alert-box.warn { background: rgba(255,176,32,0.1); border: 1px solid rgba(255,176,32,0.3); }
.alert-box.danger { background: rgba(255,82,82,0.1); border: 1px solid rgba(255,82,82,0.3); }
.alert-box.info { background: rgba(79,142,247,0.1); border: 1px solid rgba(79,142,247,0.3); }
.alert-icon { font-size: 18px; flex-shrink: 0; }
.alert-title { font-size: 13px; font-weight: 700; margin-bottom: 2px; }
.alert-desc { font-size: 12px; color: var(--text-muted); line-height: 1.5; }

/* ========= SECTION HEADER ========= */
.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0 16px 10px;
}
.section-title { font-size: 14px; font-weight: 700; }
.section-action {
  font-size: 12px;
  color: var(--green);
  cursor: pointer;
  background: none;
  border: none;
  font-family: var(--font);
  font-weight: 600;
}

/* ========= CHART CONTAINER ========= */
.chart-card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  margin: 0 16px 16px;
  overflow: hidden;
}
.chart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 16px 10px;
}
.chart-title { font-size: 13px; font-weight: 700; }
.chart-toggle {
  display: flex;
  background: var(--bg3);
  border-radius: 6px;
  padding: 2px;
  gap: 2px;
}
.chart-toggle-btn {
  padding: 4px 10px;
  border-radius: 4px;
  border: none;
  font-family: var(--font);
  font-size: 11px;
  font-weight: 600;
  cursor: pointer;
  background: transparent;
  color: var(--text-muted);
  transition: all 0.15s;
}
.chart-toggle-btn.active { background: var(--card2); color: var(--text); }
.chart-wrap { padding: 0 12px 12px; height: 180px; position: relative; }
.chart-wrap-donut { padding: 8px 12px 12px; height: 220px; position: relative; }

/* ========= QUICK ADD ========= */
.quick-btns {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
  margin: 0 16px 16px;
}
.quick-btn {
  padding: 13px;
  border-radius: var(--radius-sm);
  border: 1.5px solid;
  font-size: 13px;
  font-weight: 700;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 7px;
  font-family: var(--font);
  transition: all 0.15s;
}
.quick-btn:active { transform: scale(0.97); }
.quick-btn.expense {
  background: rgba(255,82,82,0.08);
  border-color: rgba(255,82,82,0.4);
  color: var(--red);
}
.quick-btn.income {
  background: rgba(0,200,150,0.08);
  border-color: rgba(0,200,150,0.4);
  color: var(--green);
}

/* ========= TRANSACTION LIST ========= */
.tx-list { display: flex; flex-direction: column; gap: 6px; margin: 0 16px; }
.tx-item {
  background: var(--card);
  border-radius: var(--radius-sm);
  padding: 12px 13px;
  display: flex;
  align-items: center;
  gap: 11px;
  border: 1px solid var(--border);
  transition: all 0.15s;
}
.tx-item:hover { background: var(--card2); border-color: var(--text-hint); }
.tx-emoji {
  width: 36px; height: 36px;
  border-radius: 9px;
  display: flex; align-items: center; justify-content: center;
  font-size: 16px;
  flex-shrink: 0;
}
.tx-emoji.exp { background: var(--red-dim); }
.tx-emoji.inc { background: var(--green-dim); }
.tx-info { flex: 1; min-width: 0; }
.tx-name {
  font-size: 13px;
  font-weight: 600;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.tx-meta { font-size: 10px; color: var(--text-muted); margin-top: 2px; }
.tx-amount { font-size: 13px; font-weight: 700; font-family: var(--mono); flex-shrink: 0; }
.tx-amount.exp { color: var(--red); }
.tx-amount.inc { color: var(--green); }
.tx-del-btn {
  width: 28px; height: 28px;
  border-radius: 7px;
  background: rgba(255,82,82,0.1);
  border: none;
  color: var(--red);
  cursor: pointer;
  font-size: 14px;
  display: none;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  transition: background 0.15s;
}
.tx-item:hover .tx-del-btn { display: flex; }
.tx-del-btn:hover { background: rgba(255,82,82,0.2); }

.empty-state {
  text-align: center;
  padding: 36px 20px;
  color: var(--text-muted);
  font-size: 13px;
  background: var(--card);
  border-radius: var(--radius);
  border: 1px dashed var(--border);
}
.empty-state .emoji { font-size: 36px; margin-bottom: 10px; }

/* ========= FILTER BAR ========= */
.filter-bar {
  display: flex;
  gap: 8px;
  padding: 0 16px;
  margin-bottom: 12px;
  overflow-x: auto;
  scrollbar-width: none;
}
.filter-bar::-webkit-scrollbar { display: none; }
.filter-input {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  color: var(--text);
  font-family: var(--font);
  font-size: 12px;
  padding: 8px 12px;
  outline: none;
  transition: border-color 0.15s;
}
.filter-input:focus { border-color: var(--green); }
.filter-input.search { flex: 1; min-width: 120px; }
.filter-select {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  color: var(--text);
  font-family: var(--font);
  font-size: 12px;
  padding: 8px 10px;
  outline: none;
  cursor: pointer;
  white-space: nowrap;
}
.export-btn {
  background: var(--green-dim);
  border: 1px solid rgba(0,200,150,0.3);
  border-radius: var(--radius-sm);
  color: var(--green);
  font-family: var(--font);
  font-size: 12px;
  font-weight: 700;
  padding: 8px 12px;
  cursor: pointer;
  white-space: nowrap;
  flex-shrink: 0;
}

/* ========= BUDGET SECTION ========= */
.budget-list { margin: 0 16px; display: flex; flex-direction: column; gap: 8px; }
.budget-item {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 13px 14px;
}
.budget-item-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}
.budget-item-name { font-size: 13px; font-weight: 600; }
.budget-item-pct { font-size: 11px; font-weight: 700; font-family: var(--mono); }
.budget-item-pct.ok { color: var(--green); }
.budget-item-pct.warn { color: var(--amber); }
.budget-item-pct.over { color: var(--red); }
.budget-item-amounts {
  display: flex;
  justify-content: space-between;
  font-size: 10px;
  color: var(--text-muted);
  margin-top: 6px;
}

/* ========= GOALS ========= */
.goal-list { margin: 0 16px; display: flex; flex-direction: column; gap: 8px; }
.goal-item {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 14px;
}
.goal-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 8px; }
.goal-name { font-size: 13px; font-weight: 700; }
.goal-pct { font-size: 12px; font-weight: 700; color: var(--purple); font-family: var(--mono); }
.goal-amounts { display: flex; justify-content: space-between; font-size: 10px; color: var(--text-muted); margin-top: 6px; }
.goal-progress .progress-fill { background: var(--purple); }

/* ========= PREDICTION CARDS ========= */
.predict-grid { margin: 0 16px; display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; margin-bottom: 12px; }
.predict-month {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 12px 10px;
  text-align: center;
}
.predict-month-label { font-size: 10px; color: var(--text-muted); margin-bottom: 4px; }
.predict-month-val { font-size: 12px; font-weight: 700; font-family: var(--mono); color: var(--amber); }

/* ========= AI ANALYSIS ========= */
.ai-box {
  margin: 0 16px 16px;
  background: linear-gradient(135deg, rgba(79,142,247,0.08) 0%, rgba(155,109,255,0.08) 100%);
  border: 1px solid rgba(155,109,255,0.2);
  border-radius: var(--radius);
  padding: 14px 16px;
}
.ai-box-header {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 10px;
}
.ai-box-title { font-size: 13px; font-weight: 700; }
.ai-badge {
  background: linear-gradient(90deg, var(--blue), var(--purple));
  color: white;
  font-size: 9px;
  font-weight: 700;
  padding: 2px 7px;
  border-radius: 10px;
  letter-spacing: 0.5px;
}
.ai-box-content { font-size: 12px; color: var(--text-muted); line-height: 1.7; }
.ai-box-content b { color: var(--text); }
.ai-load-btn {
  width: 100%;
  margin-top: 10px;
  padding: 9px;
  background: linear-gradient(90deg, var(--blue), var(--purple));
  border: none;
  border-radius: var(--radius-xs);
  color: white;
  font-family: var(--font);
  font-size: 12px;
  font-weight: 700;
  cursor: pointer;
  transition: opacity 0.15s;
}
.ai-load-btn:hover { opacity: 0.85; }
.ai-load-btn:disabled { opacity: 0.5; cursor: not-allowed; }

/* ========= REPORT TABLE ========= */
.report-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 12px;
}
.report-table th {
  background: var(--bg3);
  color: var(--text-muted);
  font-weight: 600;
  padding: 9px 12px;
  text-align: left;
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}
.report-table td {
  padding: 10px 12px;
  border-bottom: 1px solid var(--border);
  color: var(--text);
}
.report-table tr:last-child td { border-bottom: none; }
.report-table tr:hover td { background: var(--bg3); }
.report-table .num { font-family: var(--mono); font-size: 11px; }
.report-table .inc { color: var(--green); }
.report-table .exp { color: var(--red); }
.report-table .pos { color: var(--green); }
.report-table .neg { color: var(--red); }

/* ========= SETTINGS ========= */
.settings-section {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  margin: 0 16px 16px;
  overflow: hidden;
}
.settings-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 16px;
  border-bottom: 1px solid var(--border);
  cursor: pointer;
  transition: background 0.15s;
}
.settings-item:last-child { border-bottom: none; }
.settings-item:hover { background: var(--card2); }
.settings-item-left { display: flex; align-items: center; gap: 10px; }
.settings-icon { font-size: 18px; }
.settings-item-label { font-size: 13px; font-weight: 600; }
.settings-item-desc { font-size: 11px; color: var(--text-muted); margin-top: 1px; }
.settings-arrow { color: var(--text-hint); font-size: 14px; }
.settings-value { font-size: 12px; color: var(--text-muted); }

/* ========= MODAL ========= */
.modal-overlay {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.6);
  z-index: 100;
  align-items: flex-end;
  justify-content: center;
}
.modal-overlay.open { display: flex; }
.modal {
  background: var(--bg2);
  border-radius: 24px 24px 0 0;
  border: 1px solid var(--border);
  border-bottom: none;
  padding: 20px 20px 32px;
  width: 100%;
  max-width: 480px;
  animation: slideUp 0.25s cubic-bezier(.4,0,.2,1);
  max-height: 90vh;
  overflow-y: auto;
}
@keyframes slideUp {
  from { transform: translateY(60px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}
.modal-handle {
  width: 36px; height: 4px;
  background: var(--border);
  border-radius: 2px;
  margin: 0 auto 18px;
}
.modal-title { font-size: 17px; font-weight: 800; margin-bottom: 16px; letter-spacing: -0.3px; }

.form-group { margin-bottom: 14px; }
.form-label { font-size: 11px; color: var(--text-muted); margin-bottom: 6px; display: block; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; }
.form-input, .form-select {
  width: 100%;
  padding: 11px 13px;
  background: var(--card);
  border: 1.5px solid var(--border);
  border-radius: var(--radius-sm);
  font-size: 14px;
  color: var(--text);
  outline: none;
  font-family: var(--font);
  transition: border-color 0.15s;
}
.form-input:focus, .form-select:focus { border-color: var(--green); }
.form-select option { background: var(--bg2); }

.type-toggle {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
  margin-bottom: 14px;
}
.type-btn {
  padding: 10px;
  border-radius: var(--radius-sm);
  border: 1.5px solid var(--border);
  font-size: 13px;
  font-weight: 700;
  cursor: pointer;
  background: var(--card);
  color: var(--text-muted);
  font-family: var(--font);
  transition: all 0.15s;
}
.type-btn.active.expense { background: rgba(255,82,82,0.1); border-color: var(--red); color: var(--red); }
.type-btn.active.income { background: rgba(0,200,150,0.1); border-color: var(--green); color: var(--green); }

.cat-chips { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 6px; }
.cat-chip {
  padding: 5px 12px;
  border-radius: 20px;
  font-size: 12px;
  cursor: pointer;
  border: 1.5px solid var(--border);
  background: var(--card);
  color: var(--text-muted);
  font-family: var(--font);
  font-weight: 500;
  transition: all 0.1s;
}
.cat-chip.selected.expense { background: var(--red-dim); border-color: var(--red); color: var(--red); }
.cat-chip.selected.income { background: var(--green-dim); border-color: var(--green); color: var(--green); }

.btn-save {
  width: 100%;
  padding: 13px;
  background: var(--green);
  color: var(--bg);
  border: none;
  border-radius: var(--radius-sm);
  font-size: 14px;
  font-weight: 800;
  cursor: pointer;
  margin-top: 4px;
  font-family: var(--font);
  transition: opacity 0.15s;
  letter-spacing: -0.2px;
}
.btn-save:hover { opacity: 0.9; }

/* ========= CENTER MODAL ========= */
.center-overlay {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.6);
  z-index: 200;
  align-items: center;
  justify-content: center;
  padding: 20px;
}
.center-overlay.open { display: flex; }
.center-modal {
  background: var(--bg2);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 22px;
  width: 100%;
  max-width: 340px;
}
.center-modal h3 { font-size: 16px; font-weight: 800; margin-bottom: 8px; }
.center-modal p { font-size: 12px; color: var(--text-muted); margin-bottom: 18px; line-height: 1.5; }
.modal-btn-row { display: flex; gap: 10px; }
.btn-cancel {
  flex: 1; padding: 10px;
  border: 1.5px solid var(--border);
  background: var(--card);
  border-radius: var(--radius-sm);
  font-size: 13px;
  cursor: pointer;
  font-family: var(--font);
  color: var(--text-muted);
  font-weight: 600;
}
.btn-danger {
  flex: 1; padding: 10px;
  border: none;
  background: var(--red);
  color: white;
  border-radius: var(--radius-sm);
  font-size: 13px;
  font-weight: 800;
  cursor: pointer;
  font-family: var(--font);
}

/* ========= FAB ========= */
.fab {
  position: fixed;
  bottom: 75px;
  right: 18px;
  width: 52px;
  height: 52px;
  background: var(--green);
  color: var(--bg);
  border: none;
  border-radius: 50%;
  font-size: 24px;
  cursor: pointer;
  box-shadow: 0 4px 20px rgba(0,200,150,0.35);
  display: flex; align-items: center; justify-content: center;
  z-index: 50;
  transition: transform 0.15s;
  font-weight: 700;
}
.fab:active { transform: scale(0.9); }

/* ========= CHIP TABS ========= */
.chip-tabs {
  display: flex;
  gap: 6px;
  overflow-x: auto;
  scrollbar-width: none;
  padding: 0 16px;
  margin-bottom: 12px;
}
.chip-tabs::-webkit-scrollbar { display: none; }
.chip-tab {
  padding: 6px 14px;
  border-radius: 20px;
  border: 1.5px solid var(--border);
  background: var(--card);
  color: var(--text-muted);
  font-family: var(--font);
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
  white-space: nowrap;
  transition: all 0.15s;
  flex-shrink: 0;
}
.chip-tab.active { background: var(--green-dim); border-color: var(--green); color: var(--green); }

/* ========= DONUT LEGEND ========= */
.donut-legend {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  padding: 0 12px 12px;
  justify-content: center;
}
.legend-item {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 10px;
  color: var(--text-muted);
}
.legend-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }

/* ========= BADGE ========= */
.badge {
  display: inline-block;
  padding: 2px 8px;
  border-radius: 10px;
  font-size: 10px;
  font-weight: 700;
}
.badge-green { background: var(--green-dim); color: var(--green); }
.badge-red { background: var(--red-dim); color: var(--red); }
.badge-amber { background: rgba(255,176,32,0.15); color: var(--amber); }

/* ========= PROFILE HEADER ========= */
.profile-header {
  display: flex;
  align-items: center;
  gap: 14px;
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  margin: 0 16px 16px;
  padding: 16px;
}
.profile-avatar {
  width: 50px; height: 50px;
  border-radius: 50%;
  background: linear-gradient(135deg, var(--green), var(--blue));
  display: flex; align-items: center; justify-content: center;
  font-size: 22px;
  flex-shrink: 0;
}
.profile-name { font-size: 16px; font-weight: 800; }
.profile-sub { font-size: 11px; color: var(--text-muted); margin-top: 2px; }

/* ========= SPACER ========= */
.spacer { height: 16px; }
.spacer-sm { height: 8px; }

@media (max-width: 360px) {
  .balance-amount { font-size: 26px; }
  .predict-grid { grid-template-columns: 1fr; }
}
</style>
</head>
<body>

<!-- ============ PAGE: DASHBOARD ============ -->
<div class="page active" id="page-dashboard">
  <div class="page-header">
    <div class="page-header-top">
      <div class="page-title">Kos<span>Keuangan</span> 💰</div>
      <div class="month-nav" id="monthNav">
        <button class="month-nav-btn" onclick="changeMonth(-1)">‹</button>
        <div class="month-nav-label" id="monthNavLabel">Apr 2025</div>
        <button class="month-nav-btn" onclick="changeMonth(1)">›</button>
      </div>
    </div>
  </div>

  <div class="balance-hero" id="balanceHero">
    <div class="balance-label">SISA SALDO BULAN INI</div>
    <div class="balance-amount" id="balanceDisplay">Rp 0</div>
    <div class="balance-row">
      <div class="balance-item">
        <div class="balance-item-label">📥 Pemasukan</div>
        <div class="balance-item-val inc" id="incomeDisplay">Rp 0</div>
      </div>
      <div class="balance-item">
        <div class="balance-item-label">📤 Pengeluaran</div>
        <div class="balance-item-val exp" id="expenseDisplay">Rp 0</div>
      </div>
    </div>
    <div class="progress-wrap">
      <div class="progress-meta">
        <span id="progressLabel">0% terpakai</span>
        <span id="progressDate"></span>
      </div>
      <div class="progress-bar">
        <div class="progress-fill" id="progressFill" style="width:0%"></div>
      </div>
    </div>
  </div>

  <div class="stat-grid" id="statGrid">
    <div class="stat-card">
      <div class="stat-val" id="statDaily">Rp 0</div>
      <div class="stat-label">Rata-rata/hari</div>
    </div>
    <div class="stat-card">
      <div class="stat-val" id="statTx">0</div>
      <div class="stat-label">Transaksi</div>
    </div>
    <div class="stat-card">
      <div class="stat-val" id="statDaysLeft">—</div>
      <div class="stat-label">Estimasi sisa</div>
    </div>
  </div>

  <div class="alert-box" id="alertBox">
    <div class="alert-icon" id="alertIcon">⚠️</div>
    <div>
      <div class="alert-title" id="alertTitle"></div>
      <div class="alert-desc" id="alertDesc"></div>
    </div>
  </div>

  <!-- Flow Chart -->
  <div class="chart-card">
    <div class="chart-header">
      <div class="chart-title">Arus Keuangan</div>
      <div class="chart-toggle">
        <button class="chart-toggle-btn active" id="btnLine" onclick="switchChart('line')">Line</button>
        <button class="chart-toggle-btn" id="btnBar" onclick="switchChart('bar')">Bar</button>
      </div>
    </div>
    <div class="chart-wrap">
      <canvas id="flowChart"></canvas>
    </div>
  </div>

  <!-- Donut Chart -->
  <div class="chart-card">
    <div class="chart-header">
      <div class="chart-title">Kategori Pengeluaran</div>
    </div>
    <div class="chart-wrap-donut">
      <canvas id="donutChart"></canvas>
    </div>
    <div class="donut-legend" id="donutLegend"></div>
  </div>

  <!-- Quick Add -->
  <div class="section-header">
    <div class="section-title">Tambah Transaksi</div>
  </div>
  <div class="quick-btns">
    <button class="quick-btn expense" onclick="openModal('expense')">➖ Pengeluaran</button>
    <button class="quick-btn income" onclick="openModal('income')">➕ Pemasukan</button>
  </div>

  <!-- Recent TX -->
  <div class="section-header">
    <div class="section-title">Transaksi Terbaru</div>
    <button class="section-action" onclick="switchPage('transactions')">Lihat semua →</button>
  </div>
  <div class="tx-list" id="recentTxList">
    <div class="empty-state"><div class="emoji">📋</div><div>Belum ada transaksi</div></div>
  </div>

  <div class="spacer"></div>
</div>

<!-- ============ PAGE: TRANSACTIONS ============ -->
<div class="page" id="page-transactions">
  <div class="page-header">
    <div class="page-header-top">
      <div class="page-title">Transaksi</div>
      <div class="month-nav">
        <button class="month-nav-btn" onclick="changeMonth(-1)">‹</button>
        <div class="month-nav-label" id="monthNavLabel2"></div>
        <button class="month-nav-btn" onclick="changeMonth(1)">›</button>
      </div>
    </div>
  </div>

  <div class="filter-bar">
    <input class="filter-input search" type="text" id="filterSearch" placeholder="🔍 Cari transaksi..." oninput="renderTxPage()" />
    <select class="filter-select" id="filterType" onchange="renderTxPage()">
      <option value="">Semua</option>
      <option value="expense">Pengeluaran</option>
      <option value="income">Pemasukan</option>
    </select>
    <select class="filter-select" id="filterCat" onchange="renderTxPage()">
      <option value="">Semua Kategori</option>
    </select>
    <button class="export-btn" onclick="exportCSV()">⬇ CSV</button>
  </div>

  <div class="tx-list" id="allTxList">
    <div class="empty-state"><div class="emoji">📋</div><div>Belum ada transaksi</div></div>
  </div>
  <div class="spacer"></div>
</div>

<!-- ============ PAGE: BUDGET & GOALS ============ -->
<div class="page" id="page-budget">
  <div class="page-header">
    <div class="page-header-top">
      <div class="page-title">Budget & Goals</div>
      <div class="month-nav">
        <button class="month-nav-btn" onclick="changeMonth(-1)">‹</button>
        <div class="month-nav-label" id="monthNavLabel3"></div>
        <button class="month-nav-btn" onclick="changeMonth(1)">›</button>
      </div>
    </div>
  </div>

  <!-- Budget per Kategori -->
  <div class="section-header">
    <div class="section-title">Budget Kategori</div>
    <button class="section-action" onclick="openBudgetModal()">+ Atur</button>
  </div>
  <div class="budget-list" id="budgetList">
    <div class="empty-state"><div class="emoji">🎯</div><div>Belum ada budget.<br/>Tap "+ Atur" untuk mulai.</div></div>
  </div>

  <div class="spacer"></div>

  <!-- Goals -->
  <div class="section-header">
    <div class="section-title">Target Tabungan</div>
    <button class="section-action" onclick="openGoalModal()">+ Tambah</button>
  </div>
  <div class="goal-list" id="goalList">
    <div class="empty-state"><div class="emoji">🏆</div><div>Belum ada target tabungan.</div></div>
  </div>

  <div class="spacer"></div>

  <!-- Prediction -->
  <div class="section-header">
    <div class="section-title">Prediksi 3 Bulan ke Depan</div>
  </div>
  <div class="predict-grid" id="predictGrid">
    <div class="predict-month"><div class="predict-month-label">—</div><div class="predict-month-val">Rp 0</div></div>
    <div class="predict-month"><div class="predict-month-label">—</div><div class="predict-month-val">Rp 0</div></div>
    <div class="predict-month"><div class="predict-month-label">—</div><div class="predict-month-val">Rp 0</div></div>
  </div>

  <div class="spacer-sm"></div>

  <!-- AI Analysis -->
  <div class="ai-box">
    <div class="ai-box-header">
      <div class="ai-box-title">Analisis AI</div>
      <div class="ai-badge">✨ AI</div>
    </div>
    <div class="ai-box-content" id="aiContent">Klik tombol di bawah untuk menganalisis pola keuangan kamu secara otomatis.</div>
    <button class="ai-load-btn" id="aiBtn" onclick="runAIAnalysis()">🤖 Analisis Sekarang</button>
  </div>

  <div class="spacer"></div>
</div>

<!-- ============ PAGE: REPORT ============ -->
<div class="page" id="page-report">
  <div class="page-header">
    <div class="page-header-top">
      <div class="page-title">Laporan</div>
    </div>
  </div>

  <div class="main">
    <div class="chip-tabs" id="reportTabs" style="padding:0; margin-bottom:14px;">
      <button class="chip-tab active" onclick="switchReportTab('monthly')">Bulanan</button>
      <button class="chip-tab" onclick="switchReportTab('category')">Per Kategori</button>
    </div>
  </div>

  <!-- Monthly Summary Table -->
  <div id="reportMonthly">
    <div class="card" style="margin: 0 16px 16px; overflow: auto;">
      <table class="report-table" id="monthlyTable">
        <thead>
          <tr>
            <th>Bulan</th>
            <th>Pemasukan</th>
            <th>Pengeluaran</th>
            <th>Saldo</th>
            <th>Tx</th>
          </tr>
        </thead>
        <tbody id="monthlyTableBody"></tbody>
      </table>
    </div>
  </div>

  <!-- Category Report -->
  <div id="reportCategory" style="display:none;">
    <div class="section-header">
      <div class="section-title">Breakdown Kategori</div>
      <div class="month-nav">
        <button class="month-nav-btn" onclick="changeMonth(-1)">‹</button>
        <div class="month-nav-label" id="monthNavLabel4"></div>
        <button class="month-nav-btn" onclick="changeMonth(1)">›</button>
      </div>
    </div>
    <div class="card" style="margin: 0 16px 16px; overflow: auto;">
      <table class="report-table" id="catTable">
        <thead>
          <tr>
            <th>Kategori</th>
            <th>Jumlah</th>
            <th>Total</th>
            <th>%</th>
          </tr>
        </thead>
        <tbody id="catTableBody"></tbody>
      </table>
    </div>
  </div>

  <div class="spacer"></div>
</div>

<!-- ============ PAGE: SETTINGS ============ -->
<div class="page" id="page-settings">
  <div class="page-header">
    <div class="page-header-top">
      <div class="page-title">Pengaturan</div>
    </div>
  </div>

  <div class="profile-header" id="profileHeader">
    <div class="profile-avatar" id="profileAvatar">👤</div>
    <div>
      <div class="profile-name" id="profileName">Pengguna</div>
      <div class="profile-sub" id="profileSub">Tap untuk edit profil</div>
    </div>
  </div>

  <div class="settings-section">
    <div class="settings-item" onclick="openProfileModal()">
      <div class="settings-item-left">
        <span class="settings-icon">✏️</span>
        <div>
          <div class="settings-item-label">Edit Profil</div>
          <div class="settings-item-desc">Nama & info pribadi</div>
        </div>
      </div>
      <span class="settings-arrow">›</span>
    </div>
    <div class="settings-item" onclick="openBudgetModal()">
      <div class="settings-item-left">
        <span class="settings-icon">🎯</span>
        <div>
          <div class="settings-item-label">Atur Budget</div>
          <div class="settings-item-desc">Budget per kategori</div>
        </div>
      </div>
      <span class="settings-arrow">›</span>
    </div>
    <div class="settings-item" onclick="openGoalModal()">
      <div class="settings-item-left">
        <span class="settings-icon">🏆</span>
        <div>
          <div class="settings-item-label">Target Tabungan</div>
          <div class="settings-item-desc">Tambah / kelola goals</div>
        </div>
      </div>
      <span class="settings-arrow">›</span>
    </div>
  </div>

  <div class="settings-section">
    <div class="settings-item" onclick="exportCSV()">
      <div class="settings-item-left">
        <span class="settings-icon">📊</span>
        <div>
          <div class="settings-item-label">Export Data CSV</div>
          <div class="settings-item-desc">Download semua transaksi</div>
        </div>
      </div>
      <span class="settings-arrow">›</span>
    </div>
    <div class="settings-item" onclick="confirmClearAll()">
      <div class="settings-item-left">
        <span class="settings-icon">🗑️</span>
        <div>
          <div class="settings-item-label" style="color:var(--red)">Hapus Semua Data</div>
          <div class="settings-item-desc">Reset aplikasi ke awal</div>
        </div>
      </div>
      <span class="settings-arrow" style="color:var(--red)">›</span>
    </div>
  </div>

  <div style="text-align:center; padding: 20px 16px; color: var(--text-hint); font-size:11px;">
    KosKeuangan Pro v2.0 · Data tersimpan lokal di browser Anda
  </div>
</div>

<!-- ============ BOTTOM NAV ============ -->
<nav class="bottom-nav">
  <button class="nav-item active" onclick="switchPage('dashboard')" id="nav-dashboard">
    <span class="nav-icon">🏠</span>
    <span>Dashboard</span>
  </button>
  <button class="nav-item" onclick="switchPage('transactions')" id="nav-transactions">
    <span class="nav-icon">📋</span>
    <span>Transaksi</span>
  </button>
  <button class="nav-item" onclick="switchPage('budget')" id="nav-budget">
    <span class="nav-icon">🎯</span>
    <span>Budget</span>
  </button>
  <button class="nav-item" onclick="switchPage('report')" id="nav-report">
    <span class="nav-icon">📈</span>
    <span>Laporan</span>
  </button>
  <button class="nav-item" onclick="switchPage('settings')" id="nav-settings">
    <span class="nav-icon">⚙️</span>
    <span>Pengaturan</span>
  </button>
</nav>

<!-- FAB -->
<button class="fab" onclick="openModal('expense')" title="Tambah Transaksi">+</button>

<!-- ============ MODAL: ADD TRANSACTION ============ -->
<div class="modal-overlay" id="modalOverlay" onclick="handleOverlayClick(event)">
  <div class="modal" id="modalBox">
    <div class="modal-handle"></div>
    <div class="modal-title" id="modalTitle">Tambah Pengeluaran</div>
    <div class="type-toggle">
      <button class="type-btn active expense" id="btnExpense" onclick="setType('expense')">➖ Pengeluaran</button>
      <button class="type-btn income" id="btnIncome" onclick="setType('income')">➕ Pemasukan</button>
    </div>
    <div class="form-group">
      <label class="form-label">Jumlah (Rp) *</label>
      <input class="form-input" type="number" id="inputAmount" placeholder="cth: 25000" inputmode="numeric" />
    </div>
    <div class="form-group">
      <label class="form-label">Keterangan *</label>
      <input class="form-input" type="text" id="inputDesc" placeholder="cth: Makan siang, Ojek..." maxlength="60" />
    </div>
    <div class="form-group">
      <label class="form-label">Kategori</label>
      <div class="cat-chips" id="catChips"></div>
    </div>
    <div class="form-group">
      <label class="form-label">Tanggal</label>
      <input class="form-input" type="date" id="inputDate" />
    </div>
    <button class="btn-save" onclick="saveTransaction()">Simpan Transaksi</button>
  </div>
</div>

<!-- ============ MODAL: DELETE ============ -->
<div class="center-overlay" id="deleteOverlay">
  <div class="center-modal">
    <h3>🗑️ Hapus Transaksi?</h3>
    <p id="deleteDesc">Transaksi ini akan dihapus permanen.</p>
    <div class="modal-btn-row">
      <button class="btn-cancel" onclick="closeDelete()">Batal</button>
      <button class="btn-danger" onclick="confirmDelete()">Hapus</button>
    </div>
  </div>
</div>

<!-- ============ MODAL: CLEAR ALL ============ -->
<div class="center-overlay" id="clearAllOverlay">
  <div class="center-modal">
    <h3>⚠️ Hapus Semua Data?</h3>
    <p>Semua transaksi, budget, dan goals akan dihapus permanen. Tindakan ini tidak bisa dibatalkan.</p>
    <div class="modal-btn-row">
      <button class="btn-cancel" onclick="closeClearAll()">Batal</button>
      <button class="btn-danger" onclick="doDeleteAll()">Hapus Semua</button>
    </div>
  </div>
</div>

<!-- ============ MODAL: BUDGET ============ -->
<div class="modal-overlay" id="budgetOverlay" onclick="handleBudgetOverlayClick(event)">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">🎯 Atur Budget Kategori</div>
    <div class="form-group">
      <label class="form-label">Kategori</label>
      <select class="form-select" id="budgetCatSelect">
        <option value="">— Pilih Kategori —</option>
      </select>
    </div>
    <div class="form-group">
      <label class="form-label">Budget per Bulan (Rp)</label>
      <input class="form-input" type="number" id="budgetAmount" placeholder="cth: 500000" inputmode="numeric" />
    </div>
    <button class="btn-save" onclick="saveBudget()">Simpan Budget</button>
    <div style="margin-top:10px;">
      <div class="form-label" style="margin-bottom:8px;">Budget Tersimpan</div>
      <div id="budgetSavedList"></div>
    </div>
  </div>
</div>

<!-- ============ MODAL: GOAL ============ -->
<div class="modal-overlay" id="goalOverlay" onclick="handleGoalOverlayClick(event)">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">🏆 Tambah Target Tabungan</div>
    <div class="form-group">
      <label class="form-label">Nama Target</label>
      <input class="form-input" type="text" id="goalName" placeholder="cth: Beli Laptop, Liburan..." maxlength="40" />
    </div>
    <div class="form-group">
      <label class="form-label">Target Jumlah (Rp)</label>
      <input class="form-input" type="number" id="goalTarget" placeholder="cth: 5000000" inputmode="numeric" />
    </div>
    <div class="form-group">
      <label class="form-label">Sudah Terkumpul (Rp)</label>
      <input class="form-input" type="number" id="goalCurrent" placeholder="cth: 500000" inputmode="numeric" />
    </div>
    <div class="form-group">
      <label class="form-label">Emoji / Ikon</label>
      <input class="form-input" type="text" id="goalEmoji" placeholder="🎯" maxlength="4" />
    </div>
    <button class="btn-save" onclick="saveGoal()">Simpan Target</button>
    <div style="margin-top:12px;">
      <div class="form-label" style="margin-bottom:8px;">Goals Tersimpan</div>
      <div id="goalSavedList"></div>
    </div>
  </div>
</div>

<!-- ============ MODAL: PROFILE ============ -->
<div class="modal-overlay" id="profileOverlay" onclick="handleProfileOverlayClick(event)">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">✏️ Edit Profil</div>
    <div class="form-group">
      <label class="form-label">Nama</label>
      <input class="form-input" type="text" id="profileNameInput" placeholder="Masukkan nama kamu" maxlength="30" />
    </div>
    <div class="form-group">
      <label class="form-label">Status / Kampus</label>
      <input class="form-input" type="text" id="profileSubInput" placeholder="cth: Mahasiswa UGM, Semester 4" maxlength="50" />
    </div>
    <div class="form-group">
      <label class="form-label">Avatar Emoji</label>
      <input class="form-input" type="text" id="profileEmojiInput" placeholder="👤" maxlength="4" />
    </div>
    <button class="btn-save" onclick="saveProfile()">Simpan Profil</button>
  </div>
</div>

<script>
// ===================== CONSTANTS =====================
const CATS_EXPENSE = ['🍜 Makan','🛵 Transport','🏠 Kos','📚 Kampus','🛒 Belanja','💊 Kesehatan','☕ Hiburan','📱 Pulsa','🖨 Print','💡 Listrik','🧴 Kebutuhan','📦 Lainnya'];
const CATS_INCOME = ['💸 Kiriman Ortu','🏧 ATM','💼 Kerja','🎁 Hadiah','💳 Beasiswa','📦 Lainnya'];
const MONTH_LABELS = ['Jan','Feb','Mar','Apr','Mei','Jun','Jul','Ags','Sep','Okt','Nov','Des'];
const CHART_COLORS = ['#00C896','#4F8EF7','#FFB020','#FF5252','#9B6DFF','#00D4FF','#FF7A45','#43CF85','#E53DB0','#A0F000','#FF4D8A','#7B61FF'];

// ===================== STATE =====================
let currentType = 'expense';
let selectedCat = '';
let deleteId = null;
let currentMonth = '';
let chartMode = 'line';
let flowChartInst = null;
let donutChartInst = null;
let currentReportTab = 'monthly';

// ===================== STORAGE =====================
function getAllData() { try { return JSON.parse(localStorage.getItem('koskeuangan_v3') || '{}'); } catch { return {}; } }
function saveAllData(d) { localStorage.setItem('koskeuangan_v3', JSON.stringify(d)); }
function getMonthData(m) { const a = getAllData(); return a[m] || []; }
function saveMonthData(m, txs) { const a = getAllData(); a[m] = txs; saveAllData(a); }
function getMeta() { try { return JSON.parse(localStorage.getItem('koskeuangan_meta') || '{}'); } catch { return {}; } }
function saveMeta(d) { localStorage.setItem('koskeuangan_meta', JSON.stringify(d)); }

// ===================== HELPERS =====================
function thisMonthKey() { const n = new Date(); return n.getFullYear() + '-' + String(n.getMonth()+1).padStart(2,'0'); }
function fmt(n) { return 'Rp ' + Math.abs(Math.round(n)).toLocaleString('id-ID'); }
function fmtShort(n) {
  n = Math.abs(Math.round(n));
  if (n >= 1000000) return 'Rp ' + (n/1000000).toFixed(1) + 'jt';
  if (n >= 1000) return 'Rp ' + (n/1000).toFixed(0) + 'rb';
  return 'Rp ' + n;
}
function getMonthLabel(key) {
  const [y, m] = key.split('-');
  return MONTH_LABELS[parseInt(m)-1] + ' ' + y;
}
function addMonths(key, n) {
  let [y, m] = key.split('-').map(Number);
  m += n; 
  while (m > 12) { m -= 12; y++; }
  while (m < 1) { m += 12; y--; }
  return y + '-' + String(m).padStart(2,'0');
}

// ===================== NAV =====================
function switchPage(page) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  document.getElementById('page-' + page).classList.add('active');
  document.getElementById('nav-' + page).classList.add('active');
  
  // Refresh on switch
  if (page === 'dashboard') renderDashboard();
  if (page === 'transactions') renderTxPage();
  if (page === 'budget') renderBudgetPage();
  if (page === 'report') renderReport();
  if (page === 'settings') renderSettings();
  updateAllMonthLabels();
  window.scrollTo(0, 0);
}

// ===================== MONTH CHANGE =====================
function changeMonth(dir) {
  currentMonth = addMonths(currentMonth, dir);
  localStorage.setItem('kk_month', currentMonth);
  updateAllMonthLabels();
  renderDashboard();
  renderTxPage();
  renderBudgetPage();
  renderReport();
}

function updateAllMonthLabels() {
  const lbl = getMonthLabel(currentMonth);
  ['monthNavLabel','monthNavLabel2','monthNavLabel3','monthNavLabel4'].forEach(id => {
    const el = document.getElementById(id);
    if (el) el.textContent = lbl;
  });
}

// ===================== INIT =====================
function init() {
  currentMonth = localStorage.getItem('kk_month') || thisMonthKey();
  const all = getAllData();
  if (!all[currentMonth]) { all[currentMonth] = []; saveAllData(all); }
  updateAllMonthLabels();
  renderDashboard();
  renderSettings();
}

// ===================== DASHBOARD =====================
function renderDashboard() {
  const txs = getMonthData(currentMonth);
  const income = txs.filter(t=>t.type==='income').reduce((s,t)=>s+t.amount,0);
  const expense = txs.filter(t=>t.type==='expense').reduce((s,t)=>s+t.amount,0);
  const balance = income - expense;

  // Balance
  const balEl = document.getElementById('balanceDisplay');
  balEl.textContent = fmt(balance);
  balEl.className = 'balance-amount' + (balance < 0 ? ' danger' : balance < income * 0.2 && income > 0 ? ' warn' : '');
  document.getElementById('incomeDisplay').textContent = fmt(income);
  document.getElementById('expenseDisplay').textContent = fmt(expense);

  // Progress
  const pct = income > 0 ? Math.min(100, Math.round((expense/income)*100)) : 0;
  const fill = document.getElementById('progressFill');
  fill.style.width = pct + '%';
  fill.className = 'progress-fill' + (pct >= 90 ? ' danger' : pct >= 70 ? ' warn' : '');
  document.getElementById('progressLabel').textContent = pct + '% terpakai';
  document.getElementById('progressDate').textContent = new Date().toLocaleDateString('id-ID',{day:'numeric',month:'short'});

  // Stats
  const now = new Date();
  const dayOfMonth = now.getDate();
  const daily = dayOfMonth > 0 ? Math.round(expense / dayOfMonth) : 0;
  document.getElementById('statDaily').textContent = fmtShort(daily);
  document.getElementById('statTx').textContent = txs.length;
  let daysLeftText = '—';
  if (daily > 0 && balance > 0) daysLeftText = Math.floor(balance / daily) + ' hari';
  else if (balance <= 0 && income > 0) daysLeftText = 'Habis!';
  document.getElementById('statDaysLeft').textContent = daysLeftText;

  // Alert
  const alertBox = document.getElementById('alertBox');
  const daysLeft = daily > 0 ? Math.floor(balance / daily) : 999;
  if (balance <= 0 && income > 0) {
    alertBox.className = 'alert-box show danger';
    document.getElementById('alertIcon').textContent = '🚨';
    document.getElementById('alertTitle').textContent = 'Saldo sudah habis!';
    document.getElementById('alertDesc').textContent = 'Pengeluaran melebihi pemasukan. Segera hemat atau tambah pemasukan.';
  } else if (daysLeft <= 5 && daily > 0 && income > 0) {
    alertBox.className = 'alert-box show danger';
    document.getElementById('alertIcon').textContent = '🚨';
    document.getElementById('alertTitle').textContent = 'Uang hampir habis dalam ' + daysLeft + ' hari!';
    document.getElementById('alertDesc').textContent = 'Rata-rata ' + fmt(daily) + '/hari. Segera hemat pengeluaran!';
  } else if (daysLeft <= 10 && daily > 0 && income > 0) {
    alertBox.className = 'alert-box show warn';
    document.getElementById('alertIcon').textContent = '⚠️';
    document.getElementById('alertTitle').textContent = 'Perhatian: sisa sekitar ' + daysLeft + ' hari';
    document.getElementById('alertDesc').textContent = 'Pertimbangkan untuk mengurangi pengeluaran tidak perlu.';
  } else {
    alertBox.className = 'alert-box';
  }

  // Recent TX (max 5)
  const sorted = [...txs].sort((a,b) => new Date(b.date+' '+(b.time||'')) - new Date(a.date+' '+(a.time||'')));
  const listEl = document.getElementById('recentTxList');
  if (sorted.length === 0) {
    listEl.innerHTML = `<div class="empty-state"><div class="emoji">📋</div><div>Belum ada transaksi bulan ini</div></div>`;
  } else {
    listEl.innerHTML = sorted.slice(0,5).map(tx => renderTxItem(tx)).join('');
  }

  // Charts
  renderFlowChart(txs);
  renderDonutChart(txs);
}

function renderTxItem(tx) {
  const catEmoji = tx.cat ? tx.cat.split(' ')[0] : (tx.type === 'expense' ? '💸' : '💰');
  const catName = tx.cat ? tx.cat.replace(/^.\s/,'') : '';
  const dateStr = new Date(tx.date).toLocaleDateString('id-ID',{day:'numeric',month:'short'});
  return `<div class="tx-item">
    <div class="tx-emoji ${tx.type}">${catEmoji}</div>
    <div class="tx-info">
      <div class="tx-name">${escHtml(tx.desc)}</div>
      <div class="tx-meta">${catName}${catName?' · ':''}${dateStr} ${tx.time||''}</div>
    </div>
    <div class="tx-amount ${tx.type}">${tx.type==='expense'?'−':'+'}${fmt(tx.amount)}</div>
    <button class="tx-del-btn" onclick="event.stopPropagation();promptDelete('${tx.id}','${escAttr(tx.desc)}','${fmt(tx.amount)}')">✕</button>
  </div>`;
}

function escHtml(s) { return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;'); }
function escAttr(s) { return String(s).replace(/'/g,'&#39;'); }

// ===================== FLOW CHART =====================
function renderFlowChart(txs) {
  const ctx = document.getElementById('flowChart').getContext('2d');
  if (flowChartInst) { flowChartInst.destroy(); flowChartInst = null; }

  // Group by day
  const dayMap = {};
  txs.forEach(tx => {
    if (!dayMap[tx.date]) dayMap[tx.date] = { inc: 0, exp: 0 };
    if (tx.type === 'income') dayMap[tx.date].inc += tx.amount;
    else dayMap[tx.date].exp += tx.amount;
  });

  const days = Object.keys(dayMap).sort();
  if (days.length === 0) {
    // Show empty chart with month days
    const [y, mo] = currentMonth.split('-').map(Number);
    const daysInMonth = new Date(y, mo, 0).getDate();
    for (let d = 1; d <= Math.min(daysInMonth, 15); d++) {
      const key = currentMonth + '-' + String(d).padStart(2,'0');
      dayMap[key] = { inc: 0, exp: 0 };
    }
  }

  const labels = (days.length ? days : Object.keys(dayMap)).map(d => {
    const dt = new Date(d);
    return dt.getDate() + '/' + (dt.getMonth()+1);
  });
  const incData = (days.length ? days : Object.keys(dayMap)).map(d => dayMap[d]?.inc || 0);
  const expData = (days.length ? days : Object.keys(dayMap)).map(d => dayMap[d]?.exp || 0);

  const commonProps = {
    tension: 0.4,
    pointRadius: 3,
    pointHoverRadius: 5,
  };

  flowChartInst = new Chart(ctx, {
    type: chartMode,
    data: {
      labels,
      datasets: [
        { label: 'Pemasukan', data: incData, borderColor: '#00C896', backgroundColor: chartMode === 'line' ? 'rgba(0,200,150,0.1)' : 'rgba(0,200,150,0.7)', fill: chartMode === 'line', ...commonProps },
        { label: 'Pengeluaran', data: expData, borderColor: '#FF5252', backgroundColor: chartMode === 'line' ? 'rgba(255,82,82,0.1)' : 'rgba(255,82,82,0.7)', fill: chartMode === 'line', ...commonProps }
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { labels: { color: '#8B92AA', font: { size: 11, family: 'Plus Jakarta Sans' }, boxWidth: 10, padding: 10 } },
        tooltip: {
          backgroundColor: '#1E2335',
          titleColor: '#F0F2FA',
          bodyColor: '#8B92AA',
          borderColor: '#2E3650',
          borderWidth: 1,
          callbacks: { label: ctx => ctx.dataset.label + ': Rp ' + ctx.parsed.y.toLocaleString('id-ID') }
        }
      },
      scales: {
        x: { ticks: { color: '#5A6180', font: { size: 10 } }, grid: { color: 'rgba(46,54,80,0.5)' } },
        y: { ticks: { color: '#5A6180', font: { size: 10 }, callback: v => fmtShort(v).replace('Rp ','') }, grid: { color: 'rgba(46,54,80,0.5)' } }
      }
    }
  });
}

function switchChart(mode) {
  chartMode = mode;
  document.getElementById('btnLine').className = 'chart-toggle-btn' + (mode==='line'?' active':'');
  document.getElementById('btnBar').className = 'chart-toggle-btn' + (mode==='bar'?' active':'');
  renderFlowChart(getMonthData(currentMonth));
}

// ===================== DONUT CHART =====================
function renderDonutChart(txs) {
  const ctx = document.getElementById('donutChart').getContext('2d');
  if (donutChartInst) { donutChartInst.destroy(); donutChartInst = null; }

  const expenses = txs.filter(t => t.type === 'expense');
  const catTotals = {};
  expenses.forEach(tx => {
    const cat = tx.cat || '📦 Lainnya';
    catTotals[cat] = (catTotals[cat] || 0) + tx.amount;
  });

  const sorted = Object.entries(catTotals).sort((a,b) => b[1]-a[1]);
  const labels = sorted.map(([c]) => c);
  const data = sorted.map(([,v]) => v);

  if (data.length === 0) {
    document.getElementById('donutLegend').innerHTML = '<span style="color:var(--text-hint);font-size:11px;">Belum ada pengeluaran</span>';
    donutChartInst = new Chart(ctx, {
      type: 'doughnut',
      data: { labels: ['Kosong'], datasets: [{ data: [1], backgroundColor: ['#2E3650'], borderWidth: 0 }] },
      options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false }, tooltip: { enabled: false } }, cutout: '70%' }
    });
    return;
  }

  const total = data.reduce((s,v) => s+v, 0);

  // Legend
  document.getElementById('donutLegend').innerHTML = labels.map((l, i) => {
    const pct = Math.round((data[i]/total)*100);
    return `<div class="legend-item"><div class="legend-dot" style="background:${CHART_COLORS[i%CHART_COLORS.length]}"></div>${l.split(' ').slice(1).join(' ')} ${pct}%</div>`;
  }).join('');

  donutChartInst = new Chart(ctx, {
    type: 'doughnut',
    data: {
      labels,
      datasets: [{
        data,
        backgroundColor: CHART_COLORS.slice(0, data.length),
        borderWidth: 2,
        borderColor: '#1E2335'
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      cutout: '68%',
      plugins: {
        legend: { display: false },
        tooltip: {
          backgroundColor: '#1E2335',
          titleColor: '#F0F2FA',
          bodyColor: '#8B92AA',
          borderColor: '#2E3650',
          borderWidth: 1,
          callbacks: {
            label: ctx => ' ' + fmt(ctx.parsed) + ' (' + Math.round((ctx.parsed/total)*100) + '%)'
          }
        }
      }
    }
  });
}

// ===================== TRANSACTION PAGE =====================
function renderTxPage() {
  const q = (document.getElementById('filterSearch')?.value || '').toLowerCase();
  const type = document.getElementById('filterType')?.value || '';
  const cat = document.getElementById('filterCat')?.value || '';

  const allCats = [...CATS_EXPENSE, ...CATS_INCOME];
  const catSel = document.getElementById('filterCat');
  if (catSel && catSel.options.length <= 1) {
    allCats.forEach(c => {
      const opt = document.createElement('option');
      opt.value = c; opt.textContent = c;
      catSel.appendChild(opt);
    });
  }

  let txs = getMonthData(currentMonth);
  if (type) txs = txs.filter(t => t.type === type);
  if (cat) txs = txs.filter(t => t.cat === cat);
  if (q) txs = txs.filter(t => t.desc.toLowerCase().includes(q) || (t.cat||'').toLowerCase().includes(q));

  const sorted = [...txs].sort((a,b) => new Date(b.date+' '+(b.time||'')) - new Date(a.date+' '+(a.time||'')));
  const listEl = document.getElementById('allTxList');

  if (sorted.length === 0) {
    listEl.innerHTML = `<div class="empty-state"><div class="emoji">🔍</div><div>Tidak ada transaksi ditemukan</div></div>`;
    return;
  }
  listEl.innerHTML = sorted.map(tx => renderTxItem(tx)).join('');
}

// ===================== EXPORT CSV =====================
function exportCSV() {
  const all = getAllData();
  const rows = [['Bulan','Tanggal','Waktu','Tipe','Kategori','Keterangan','Jumlah']];
  Object.entries(all).sort().forEach(([month, txs]) => {
    txs.sort((a,b) => new Date(a.date) - new Date(b.date)).forEach(tx => {
      rows.push([getMonthLabel(month), tx.date, tx.time||'', tx.type==='income'?'Pemasukan':'Pengeluaran', tx.cat||'', tx.desc, tx.amount]);
    });
  });
  const csv = rows.map(r => r.map(v => '"'+String(v).replace(/"/g,'""')+'"').join(',')).join('\n');
  const blob = new Blob(['\uFEFF'+csv], { type: 'text/csv;charset=utf-8;' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'koskeuangan_' + new Date().toISOString().slice(0,10) + '.csv';
  a.click();
}

// ===================== BUDGET PAGE =====================
function renderBudgetPage() {
  const meta = getMeta();
  const budgets = meta.budgets || {};
  const txs = getMonthData(currentMonth);
  const expenses = txs.filter(t => t.type === 'expense');

  // Budget list
  const listEl = document.getElementById('budgetList');
  const budgetEntries = Object.entries(budgets);
  if (budgetEntries.length === 0) {
    listEl.innerHTML = `<div class="empty-state"><div class="emoji">🎯</div><div>Belum ada budget.<br/>Tap "+ Atur" untuk mulai.</div></div>`;
  } else {
    listEl.innerHTML = budgetEntries.map(([cat, limit]) => {
      const spent = expenses.filter(t => t.cat === cat).reduce((s,t) => s+t.amount, 0);
      const pct = limit > 0 ? Math.min(100, Math.round((spent/limit)*100)) : 0;
      const cls = pct >= 100 ? 'over' : pct >= 80 ? 'warn' : 'ok';
      const fillCls = pct >= 100 ? ' danger' : pct >= 80 ? ' warn' : '';
      const emoji = cat.split(' ')[0];
      return `<div class="budget-item${pct>=100?' ' :''}">
        <div class="budget-item-header">
          <div class="budget-item-name">${escHtml(cat)}</div>
          <div class="budget-item-pct ${cls}">${pct}% ${pct>=100?'⚠️':''}</div>
        </div>
        <div class="progress-bar"><div class="progress-fill${fillCls}" style="width:${pct}%"></div></div>
        <div class="budget-item-amounts">
          <span>${fmt(spent)} terpakai</span>
          <span>dari ${fmt(limit)}</span>
        </div>
      </div>`;
    }).join('');
  }

  // Goals
  const goals = meta.goals || [];
  const goalListEl = document.getElementById('goalList');
  if (goals.length === 0) {
    goalListEl.innerHTML = `<div class="empty-state"><div class="emoji">🏆</div><div>Belum ada target tabungan.</div></div>`;
  } else {
    goalListEl.innerHTML = goals.map((g, i) => {
      const pct = g.target > 0 ? Math.min(100, Math.round((g.current/g.target)*100)) : 0;
      return `<div class="goal-item">
        <div class="goal-header">
          <div>
            <div class="goal-name">${g.emoji||'🎯'} ${escHtml(g.name)}</div>
            <div style="font-size:11px;color:var(--text-muted);margin-top:2px;">${fmt(g.current)} / ${fmt(g.target)}</div>
          </div>
          <div style="display:flex;gap:6px;align-items:center;">
            <div class="goal-pct">${pct}%</div>
            <button onclick="deleteGoal(${i})" style="background:var(--red-dim);border:none;color:var(--red);border-radius:6px;padding:4px 8px;cursor:pointer;font-size:11px;">✕</button>
          </div>
        </div>
        <div class="progress-bar goal-progress"><div class="progress-fill" style="width:${pct}%;background:var(--purple)"></div></div>
        <div class="goal-amounts"><span>Sisa: ${fmt(Math.max(0, g.target - g.current))}</span><span>${pct === 100 ? '🎉 Tercapai!' : 'Target belum tercapai'}</span></div>
      </div>`;
    }).join('');
  }

  // Prediction
  renderPrediction();
}

function renderPrediction() {
  const all = getAllData();
  const keys = Object.keys(all).sort().slice(-3);
  const avgExpenses = {};
  let monthCount = 0;
  keys.forEach(k => {
    const txs = all[k] || [];
    const exp = txs.filter(t => t.type === 'expense').reduce((s,t) => s+t.amount, 0);
    if (exp > 0) { avgExpenses[k] = exp; monthCount++; }
  });

  const avg = monthCount > 0 ? Object.values(avgExpenses).reduce((s,v) => s+v, 0) / monthCount : 0;
  // Project with slight trend (5% increase per month as conservative estimate)
  const grid = document.getElementById('predictGrid');
  grid.innerHTML = [1,2,3].map(i => {
    const mKey = addMonths(currentMonth, i);
    const predicted = Math.round(avg * (1 + i * 0.03));
    return `<div class="predict-month">
      <div class="predict-month-label">${getMonthLabel(mKey)}</div>
      <div class="predict-month-val">${predicted > 0 ? fmtShort(predicted) : '—'}</div>
      <div style="font-size:9px;color:var(--text-hint);margin-top:3px;">${avg > 0 ? '📊 Estimasi' : 'Kurang data'}</div>
    </div>`;
  }).join('');
}

// ===================== AI ANALYSIS =====================
async function runAIAnalysis() {
  const btn = document.getElementById('aiBtn');
  const content = document.getElementById('aiContent');
  btn.disabled = true;
  btn.textContent = '⏳ Menganalisis...';
  content.textContent = 'Sedang menganalisis pola keuangan kamu...';

  try {
    const all = getAllData();
    const meta = getMeta();
    const summaries = [];
    Object.entries(all).sort().slice(-4).forEach(([month, txs]) => {
      const inc = txs.filter(t=>t.type==='income').reduce((s,t)=>s+t.amount,0);
      const exp = txs.filter(t=>t.type==='expense').reduce((s,t)=>s+t.amount,0);
      const topCats = {};
      txs.filter(t=>t.type==='expense').forEach(t => { topCats[t.cat||'Lainnya'] = (topCats[t.cat||'Lainnya']||0)+t.amount; });
      const top = Object.entries(topCats).sort((a,b)=>b[1]-a[1]).slice(0,3).map(([c,v])=>c+':Rp'+Math.round(v/1000)+'rb').join(', ');
      summaries.push(`${getMonthLabel(month)}: pemasukan Rp${Math.round(inc/1000)}rb, pengeluaran Rp${Math.round(exp/1000)}rb, top kategori: ${top||'—'}`);
    });

    const budgets = meta.budgets || {};
    const budgetInfo = Object.entries(budgets).map(([c,v])=>c+': Rp'+Math.round(v/1000)+'rb').join(', ');
    const goals = (meta.goals||[]).map(g=>g.name+': '+Math.round((g.current/g.target)*100)+'%').join(', ');

    const prompt = `Kamu adalah asisten keuangan personal untuk mahasiswa kos di Indonesia. Analisis data keuangan berikut dan berikan saran yang praktis, spesifik, dan relevan dalam Bahasa Indonesia. Buat dalam 3-4 poin singkat.

Data Keuangan (4 bulan terakhir):
${summaries.join('\n')}

Budget tersimpan: ${budgetInfo || 'belum ada'}
Goals: ${goals || 'belum ada'}

Berikan:
1. Tren pengeluaran (naik/turun/stabil)
2. Kategori terboros dan saran hemat
3. Apakah goals realistis
4. 1 saran actionable spesifik untuk bulan depan

Format: singkat, pakai emoji, bahasa casual tapi profesional. Jangan lebih dari 120 kata.`;

    const res = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        model: "claude-sonnet-4-20250514",
        max_tokens: 1000,
        messages: [{ role: "user", content: prompt }]
      })
    });

    const data = await res.json();
    const text = data.content?.map(b => b.text || '').join('') || 'Tidak dapat menganalisis saat ini.';
    content.innerHTML = text.replace(/\n/g, '<br>');
    btn.textContent = '🔄 Analisis Ulang';
  } catch (e) {
    content.textContent = '⚠️ Gagal menganalisis. Pastikan koneksi internet aktif dan coba lagi.';
    btn.textContent = '🤖 Coba Lagi';
  }
  btn.disabled = false;
}

// ===================== REPORT =====================
function switchReportTab(tab) {
  currentReportTab = tab;
  document.querySelectorAll('#reportTabs .chip-tab').forEach((el, i) => {
    el.className = 'chip-tab' + ((i===0 && tab==='monthly') || (i===1 && tab==='category') ? ' active' : '');
  });
  document.getElementById('reportMonthly').style.display = tab === 'monthly' ? 'block' : 'none';
  document.getElementById('reportCategory').style.display = tab === 'category' ? 'block' : 'none';
  renderReport();
}

function renderReport() {
  if (currentReportTab === 'monthly') renderMonthlyReport();
  else renderCategoryReport();
}

function renderMonthlyReport() {
  const all = getAllData();
  const keys = Object.keys(all).sort().reverse();
  const tbody = document.getElementById('monthlyTableBody');
  if (keys.length === 0) {
    tbody.innerHTML = '<tr><td colspan="5" style="text-align:center;color:var(--text-muted)">Belum ada data</td></tr>';
    return;
  }
  tbody.innerHTML = keys.map(k => {
    const txs = all[k] || [];
    const inc = txs.filter(t=>t.type==='income').reduce((s,t)=>s+t.amount,0);
    const exp = txs.filter(t=>t.type==='expense').reduce((s,t)=>s+t.amount,0);
    const bal = inc - exp;
    return `<tr>
      <td><b>${getMonthLabel(k)}</b></td>
      <td class="num inc">+${fmtShort(inc)}</td>
      <td class="num exp">-${fmtShort(exp)}</td>
      <td class="num ${bal>=0?'pos':'neg'}">${bal>=0?'+':''}${fmtShort(Math.abs(bal))}</td>
      <td>${txs.length}</td>
    </tr>`;
  }).join('');
}

function renderCategoryReport() {
  const txs = getMonthData(currentMonth);
  const expenses = txs.filter(t => t.type === 'expense');
  const total = expenses.reduce((s,t) => s+t.amount, 0);
  const catMap = {};
  expenses.forEach(tx => {
    const c = tx.cat || '📦 Lainnya';
    if (!catMap[c]) catMap[c] = { count: 0, total: 0 };
    catMap[c].count++;
    catMap[c].total += tx.amount;
  });

  const tbody = document.getElementById('catTableBody');
  const sorted = Object.entries(catMap).sort((a,b) => b[1].total - a[1].total);
  if (sorted.length === 0) {
    tbody.innerHTML = '<tr><td colspan="4" style="text-align:center;color:var(--text-muted)">Belum ada pengeluaran</td></tr>';
    return;
  }
  tbody.innerHTML = sorted.map(([cat, d]) => {
    const pct = total > 0 ? Math.round((d.total/total)*100) : 0;
    return `<tr>
      <td>${escHtml(cat)}</td>
      <td>${d.count}x</td>
      <td class="num exp">-${fmtShort(d.total)}</td>
      <td><span class="badge badge-amber">${pct}%</span></td>
    </tr>`;
  }).join('');
}

// ===================== SETTINGS =====================
function renderSettings() {
  const meta = getMeta();
  const profile = meta.profile || {};
  document.getElementById('profileName').textContent = profile.name || 'Pengguna';
  document.getElementById('profileSub').textContent = profile.sub || 'Mahasiswa kos';
  document.getElementById('profileAvatar').textContent = profile.emoji || '👤';
}

function openProfileModal() {
  const meta = getMeta();
  const p = meta.profile || {};
  document.getElementById('profileNameInput').value = p.name || '';
  document.getElementById('profileSubInput').value = p.sub || '';
  document.getElementById('profileEmojiInput').value = p.emoji || '';
  document.getElementById('profileOverlay').classList.add('open');
}
function saveProfile() {
  const meta = getMeta();
  meta.profile = {
    name: document.getElementById('profileNameInput').value.trim() || 'Pengguna',
    sub: document.getElementById('profileSubInput').value.trim(),
    emoji: document.getElementById('profileEmojiInput').value.trim() || '👤'
  };
  saveMeta(meta);
  document.getElementById('profileOverlay').classList.remove('open');
  renderSettings();
}
function handleProfileOverlayClick(e) { if (e.target === document.getElementById('profileOverlay')) document.getElementById('profileOverlay').classList.remove('open'); }

// ===================== BUDGET MODAL =====================
function openBudgetModal() {
  const sel = document.getElementById('budgetCatSelect');
  sel.innerHTML = '<option value="">— Pilih Kategori —</option>';
  CATS_EXPENSE.forEach(c => { const o = document.createElement('option'); o.value = c; o.textContent = c; sel.appendChild(o); });
  document.getElementById('budgetAmount').value = '';
  renderBudgetSavedList();
  document.getElementById('budgetOverlay').classList.add('open');
}
function handleBudgetOverlayClick(e) { if (e.target === document.getElementById('budgetOverlay')) document.getElementById('budgetOverlay').classList.remove('open'); }
function saveBudget() {
  const cat = document.getElementById('budgetCatSelect').value;
  const amount = parseFloat(document.getElementById('budgetAmount').value);
  if (!cat) { alert('Pilih kategori!'); return; }
  if (!amount || amount <= 0) { alert('Masukkan jumlah yang valid!'); return; }
  const meta = getMeta();
  if (!meta.budgets) meta.budgets = {};
  meta.budgets[cat] = Math.round(amount);
  saveMeta(meta);
  document.getElementById('budgetAmount').value = '';
  renderBudgetSavedList();
  renderBudgetPage();
}
function deleteBudget(cat) {
  const meta = getMeta();
  if (meta.budgets) { delete meta.budgets[cat]; saveMeta(meta); }
  renderBudgetSavedList();
  renderBudgetPage();
}
function renderBudgetSavedList() {
  const meta = getMeta();
  const budgets = meta.budgets || {};
  const el = document.getElementById('budgetSavedList');
  const entries = Object.entries(budgets);
  if (entries.length === 0) { el.innerHTML = '<div style="font-size:12px;color:var(--text-hint)">Belum ada budget</div>'; return; }
  el.innerHTML = entries.map(([c, v]) => `<div style="display:flex;justify-content:space-between;align-items:center;padding:7px 0;border-bottom:1px solid var(--border);">
    <span style="font-size:12px;">${escHtml(c)}</span>
    <div style="display:flex;gap:8px;align-items:center;">
      <span style="font-size:11px;color:var(--text-muted);font-family:var(--mono);">${fmt(v)}</span>
      <button onclick="deleteBudget('${escAttr(c)}')" style="background:var(--red-dim);border:none;color:var(--red);border-radius:4px;padding:2px 7px;cursor:pointer;font-size:11px;">✕</button>
    </div>
  </div>`).join('');
}

// ===================== GOAL MODAL =====================
function openGoalModal() {
  document.getElementById('goalName').value = '';
  document.getElementById('goalTarget').value = '';
  document.getElementById('goalCurrent').value = '';
  document.getElementById('goalEmoji').value = '';
  renderGoalSavedList();
  document.getElementById('goalOverlay').classList.add('open');
}
function handleGoalOverlayClick(e) { if (e.target === document.getElementById('goalOverlay')) document.getElementById('goalOverlay').classList.remove('open'); }
function saveGoal() {
  const name = document.getElementById('goalName').value.trim();
  const target = parseFloat(document.getElementById('goalTarget').value);
  const current = parseFloat(document.getElementById('goalCurrent').value) || 0;
  const emoji = document.getElementById('goalEmoji').value.trim() || '🎯';
  if (!name) { alert('Masukkan nama target!'); return; }
  if (!target || target <= 0) { alert('Masukkan target jumlah yang valid!'); return; }
  const meta = getMeta();
  if (!meta.goals) meta.goals = [];
  meta.goals.push({ name, target: Math.round(target), current: Math.round(current), emoji });
  saveMeta(meta);
  document.getElementById('goalName').value = '';
  document.getElementById('goalTarget').value = '';
  document.getElementById('goalCurrent').value = '';
  renderGoalSavedList();
  renderBudgetPage();
}
function deleteGoal(i) {
  const meta = getMeta();
  if (meta.goals) { meta.goals.splice(i, 1); saveMeta(meta); }
  renderGoalSavedList();
  renderBudgetPage();
}
function renderGoalSavedList() {
  const meta = getMeta();
  const goals = meta.goals || [];
  const el = document.getElementById('goalSavedList');
  if (goals.length === 0) { el.innerHTML = '<div style="font-size:12px;color:var(--text-hint)">Belum ada goals</div>'; return; }
  el.innerHTML = goals.map((g, i) => `<div style="display:flex;justify-content:space-between;align-items:center;padding:7px 0;border-bottom:1px solid var(--border);">
    <span style="font-size:12px;">${g.emoji||'🎯'} ${escHtml(g.name)}</span>
    <div style="display:flex;gap:8px;align-items:center;">
      <span style="font-size:11px;color:var(--text-muted);font-family:var(--mono);">${fmt(g.current)}/${fmt(g.target)}</span>
      <button onclick="deleteGoal(${i})" style="background:var(--red-dim);border:none;color:var(--red);border-radius:4px;padding:2px 7px;cursor:pointer;font-size:11px;">✕</button>
    </div>
  </div>`).join('');
}

// ===================== ADD TRANSACTION MODAL =====================
function openModal(type) {
  setType(type);
  document.getElementById('inputAmount').value = '';
  document.getElementById('inputDesc').value = '';
  selectedCat = '';
  const now = new Date();
  document.getElementById('inputDate').value = now.toISOString().split('T')[0];
  document.getElementById('modalOverlay').classList.add('open');
  setTimeout(() => document.getElementById('inputAmount').focus(), 300);
}
function handleOverlayClick(e) { if (e.target === document.getElementById('modalOverlay')) closeModal(); }
function closeModal() { document.getElementById('modalOverlay').classList.remove('open'); }

function setType(type) {
  currentType = type;
  document.getElementById('modalTitle').textContent = type === 'expense' ? 'Tambah Pengeluaran' : 'Tambah Pemasukan';
  document.getElementById('btnExpense').className = 'type-btn expense' + (type==='expense'?' active':'');
  document.getElementById('btnIncome').className = 'type-btn income' + (type==='income'?' active':'');
  selectedCat = '';
  renderCats();
}

function renderCats() {
  const cats = currentType === 'expense' ? CATS_EXPENSE : CATS_INCOME;
  document.getElementById('catChips').innerHTML = cats.map(c =>
    `<button class="cat-chip${selectedCat===c?' selected '+currentType:''}" onclick="selectCat('${escAttr(c)}')">${c}</button>`
  ).join('');
}

function selectCat(cat) {
  selectedCat = selectedCat === cat ? '' : cat;
  renderCats();
}

function saveTransaction() {
  const amount = parseFloat(document.getElementById('inputAmount').value);
  const desc = document.getElementById('inputDesc').value.trim();
  const date = document.getElementById('inputDate').value;
  if (!amount || amount <= 0) { alert('Masukkan jumlah yang valid!'); return; }
  if (!desc) { alert('Masukkan keterangan!'); return; }
  if (!date) { alert('Pilih tanggal!'); return; }

  const [y, m] = date.split('-');
  const txMonth = y + '-' + m;

  const now = new Date();
  const tx = {
    id: Date.now().toString(),
    type: currentType,
    amount: Math.round(amount),
    desc,
    cat: selectedCat,
    date,
    time: now.toTimeString().slice(0,5)
  };

  const txs = getMonthData(txMonth);
  txs.push(tx);
  saveMonthData(txMonth, txs);
  closeModal();
  renderDashboard();
  renderTxPage();
  renderBudgetPage();
}

// ===================== DELETE =====================
function promptDelete(id, desc, amount) {
  deleteId = id;
  document.getElementById('deleteDesc').textContent = `"${desc}" — ${amount}`;
  document.getElementById('deleteOverlay').classList.add('open');
}
function closeDelete() { document.getElementById('deleteOverlay').classList.remove('open'); deleteId = null; }
function confirmDelete() {
  if (!deleteId) return;
  // Find and delete from all months
  const all = getAllData();
  Object.keys(all).forEach(m => { all[m] = all[m].filter(t => t.id !== deleteId); });
  saveAllData(all);
  closeDelete();
  renderDashboard();
  renderTxPage();
  renderBudgetPage();
}

// ===================== CLEAR ALL =====================
function confirmClearAll() { document.getElementById('clearAllOverlay').classList.add('open'); }
function closeClearAll() { document.getElementById('clearAllOverlay').classList.remove('open'); }
function doDeleteAll() {
  localStorage.removeItem('koskeuangan_v3');
  localStorage.removeItem('koskeuangan_meta');
  closeClearAll();
  currentMonth = thisMonthKey();
  localStorage.setItem('kk_month', currentMonth);
  updateAllMonthLabels();
  renderDashboard();
  renderSettings();
  switchPage('dashboard');
  alert('Semua data telah dihapus.');
}

// ===================== KEYBOARD =====================
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') {
    closeModal();
    closeDelete();
    closeClearAll();
    document.getElementById('budgetOverlay').classList.remove('open');
    document.getElementById('goalOverlay').classList.remove('open');
    document.getElementById('profileOverlay').classList.remove('open');
  }
});

// ===================== START =====================
init();
renderCats();
</script>
</body>
</html>
