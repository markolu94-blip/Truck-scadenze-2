<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Truck Scadenze</title>
<style>
body {
  font-family: Arial, sans-serif;
  background: #eef2f6;
  margin: 0;
  padding: 18px;
  color: #172b43;
}
header {
  background: #142b45;
  color: white;
  padding: 22px;
  text-align: center;
  border-radius: 15px;
}
.card {
  background: white;
  padding: 18px;
  margin-top: 18px;
  border-radius: 14px;
  box-shadow: 0 3px 10px #0001;
}
h2 { margin-top: 0; }
label {
  font-weight: bold;
  display: block;
  margin-bottom: 8px;
  font-size: 14px;
  color: #142b45;
}
input[type="date"] {
  width: 100%;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 10px;
  box-sizing: border-box;
  font-size: 15px;
  background: #f9fbfd;
}
button {
  width: 100%;
  padding: 16px;
  margin-top: 12px;
  border: 0;
  border-radius: 10px;
  background: #1476d4;
  color: white;
  font-size: 15px;
  font-weight: bold;
  cursor: pointer;
}
.btn-reset {
  background: #d93025;
}
.status {
  font-weight: bold;
  color: #1476d4;
  line-height: 1.6;
}
.history {
  border-bottom: 1px solid #ddd;
  padding: 9px 0;
}
.license-text {
  font-size: 12px;
  color: #475569;
  font-family: monospace;
  white-space: pre-wrap;
  line-height: 1.5;
  background: #f8fafc;
  padding: 12px;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}
</style>
</head>
<body>

<header>
  <h1>🚛 TRUCK SCADENZE</h1>
  <p>Il tuo assistente per il tachigrafo</p>
</header>

<div class="card">
  <h2>📅 Data dello scarico</h2>
  <label for="referenceDate">Seleziona la data in cui hai fatto lo scarico:</label>
  <input type="date" id="referenceDate">
</div>

<div class="card">
  <h2>💳 Carta conducente</h2>
  <p id="cardStatus" class="status"></p>
  <button onclick="register('card')">
    HO SCARICATO LA CARTA
  </button>
</div>

<div class="card">
  <h2>🚛 Memoria camion</h2>
  <p id="truckStatus" class="status"></p>
  <button onclick="register('truck')">
    HO SCARICATO LA MEMORIA
  </button>
</div>

<div class="card">
  <h2>🔔 Promemoria</h2>
  <p>Un giorno prima e il giorno della scadenza.</p>
  <button onclick="enableNotifications()">
    ATTIVA NOTIFICHE
  </button>
</div>

<div class="card">
  <h2>📋 Storico scarichi</h2>
  <div id="history"></div>
  <button class="btn-reset" onclick="resetHistory()">
    🗑️ RESETTA STORICO
  </button>
</div>

<div class="card">
  <h2>📜 Licenza</h2>
  <div class="license-text">MIT License

Copyright (c) 2026 Markolu94

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.</div>
</div>

<script>
function parseLocalDate(dateStr) {
  if (!dateStr) return new Date();
  const [year, month, day] = dateStr.split('-').map(Number);
  return new Date(year, month - 1, day, 12, 0, 0);
}

// Imposta di default la data odierna nel selettore
const today = new Date();
const year = today.getFullYear();
const month = String(today.getMonth() + 1).padStart(2, '0');
const day = String(today.getDate()).padStart(2, '0');
document.getElementById('referenceDate').value = `${year}-${month}-${day}`;

const intervals = {
  card: 28,
  truck: 90
};

let records = JSON.parse(
  localStorage.getItem("truckRecords") || "[]"
);

function save() {
  localStorage.setItem(
    "truckRecords",
    JSON.stringify(records)
  );
}

function register(type) {
  const refInput = document.getElementById('referenceDate').value;
  const dataDaRegistrare = parseLocalDate(refInput);

  const name = type === 'card' ? 'Carta conducente' : 'Memoria camion';
  if (!confirm("Confermi di aver scaricato " + name + " in data " + dataDaRegistrare.toLocaleDateString("it-IT") + "?")) return;

  records.push({
    type,
    date: dataDaRegistrare.toISOString()
  });

  save();
  render();

  creaEventoCalendario(type, dataDaRegistrare);
}

function creaEventoCalendario(type, dataUltimoScarico) {
  const giorniIntervallo = intervals[type];
  const dataScadenza = new Date(dataUltimoScarico);
  dataScadenza.setDate(dataScadenza.getDate() + giorniIntervallo);

  const nomeEvento = type === 'card' ? 'Scadenza Scarico Carta Conducente' : 'Scadenza Scarico Memoria Camion';
  
  const a = dataScadenza.getFullYear();
  const m = String(dataScadenza.getMonth() + 1).padStart(2, '0');
  const d = String(dataScadenza.getDate()).padStart(2, '0');
  const dataFormattata = `${a}${m}${d}`;

  const icsContent = `BEGIN:VCALENDAR
VERSION:2.0
BEGIN:VEVENT
SUMMARY:${nomeEvento}
DESCRIPTION:Ricordati di scaricare ${type === 'card' ? 'la carta conducente' : 'la memoria del camion'}.
DTSTART;VALUE=DATE:${dataFormattata}
DTEND;VALUE=DATE:${dataFormattata}
END:VEVENT
END:VCALENDAR`;

  const blob = new Blob([icsContent], { type: 'text/calendar;charset=utf-8' });
  const link = document.createElement('a');
  link.href = window.URL.createObjectURL(blob);
  link.download = `${type}_scadenza.ics`;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
}

function resetHistory() {
  if (!confirm("Sei sicuro di voler cancellare tutto lo storico e i dati salvati?")) return;
  records = [];
  localStorage.removeItem("truckRecords");
  render();
}

function lastRecord(type) {
  return records.filter(r => r.type === type)
    .sort((a,b) => new Date(b.date)-new Date(a.date))[0];
}

function renderStatus(type, element) {
  const record = lastRecord(type);
  const name = type === "card"
    ? "Carta conducente"
    : "Memoria camion";

  if (!record) {
    element.innerHTML = name +
      "<br>Nessuno scarico registrato.";
    return;
  }

  const last = new Date(record.date);
  const due = new Date(last);
  due.setDate(due.getDate() + intervals[type]);

  const realToday = new Date();

  const dueMidnight = new Date(due.getFullYear(), due.getMonth(), due.getDate());
  const todayMidnight = new Date(realToday.getFullYear(), realToday.getMonth(), realToday.getDate());

  const days = Math.round((dueMidnight - todayMidnight) / 86400000);

  element.innerHTML =
    name +
    "<br>Ultimo scarico: " + last.toLocaleDateString("it-IT") +
    "<br>Prossima scadenza: " + due.toLocaleDateString("it-IT") +
    "<br>" + (days < 0
      ? `⚠️ SCADENZA SUPERATA da <strong>${Math.abs(days)}</strong> giorni`
      : days === 0
      ? "🚨 SCADE OGGI!"
      : "Giorni rimanenti: " + days);
}

function render() {
  renderStatus("card", document.getElementById("cardStatus"));
  renderStatus("truck", document.getElementById("truckStatus"));

  const history = document.getElementById("history");
  history.innerHTML = "";

  records.slice().reverse().forEach(r => {
    const div = document.createElement("div");
    div.className = "history";
    div.textContent =
      (r.type === "card" ? "💳 Carta conducente" : "🚛 Memoria camion") +
      " — " + new Date(r.date).toLocaleDateString("it-IT");
    history.appendChild(div);
  });

  if (!records.length) {
    history.textContent = "Nessuno scarico registrato.";
  }
}

async function enableNotifications() {
  if (!("Notification" in window)) {
    alert("Il browser non supporta le notifiche.");
    return;
  }

  const permission = await Notification.requestPermission();

  if (permission === "granted") {
    alert("Notifiche autorizzate!");
  } else {
    alert("Autorizza le notifiche nelle impostazioni del browser.");
  }
}

render();
</script>

</body>
</html>
