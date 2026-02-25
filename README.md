KompetenzTeam
<!doctype html>
<html lang="de">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>NCSD – Bußgeldbescheid Generator</title>

  <style>
    :root{
      font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;

      /* Dark theme variables */
      --bg: #0b1220;
      --text: #e5e7eb;
      --muted: #9ca3af;
      --border: rgba(255,255,255,.12);
      --border2: rgba(255,255,255,.18);
      --shadow: 0 10px 28px rgba(0,0,0,.55);
      --inputBg: rgba(255,255,255,.06);
      --inputBg2: rgba(255,255,255,.08);
      --focus: rgba(59,130,246,.35);
      --danger: #ef4444;
      --warnBorder: rgba(245,158,11,.55);
    }

    body{
      margin:0;
      padding:24px;
      background: radial-gradient(1200px 700px at 20% -10%, rgba(99,102,241,.20), transparent 60%),
                  radial-gradient(900px 600px at 100% 0%, rgba(59,130,246,.16), transparent 55%),
                  var(--bg);
      color: var(--text);
    }

    .wrap{ max-width:1200px; margin:0 auto; display:grid; gap:16px; }

    .card{
      background: linear-gradient(180deg, rgba(255,255,255,.04), rgba(255,255,255,.02));
      border: 1px solid var(--border);
      border-radius:14px;
      padding:16px;
      box-shadow: var(--shadow);
      backdrop-filter: blur(6px);
    }

    h1{
      margin:0 0 8px;
      font-size:20px;
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:12px;
      flex-wrap:wrap;
    }
    .titleLeft{ display:flex; align-items:center; gap:10px; flex-wrap:wrap; }
    h2{ margin:0 0 8px; font-size:16px; color: var(--text); }
    .grid{ display:grid; grid-template-columns:1fr 1fr; gap:12px; }

    label{ display:grid; gap:6px; font-size:13px; color: var(--text); }

    input, textarea, select{
      border: 1px solid var(--border2);
      border-radius:10px;
      padding:10px 12px;
      font-size:14px;
      outline:none;
      background: var(--inputBg);
      color: var(--text);
      transition: border-color .15s, box-shadow .15s, background .15s;
    }

    input::placeholder, textarea::placeholder{ color: rgba(229,231,235,.55); }

    input:focus, textarea:focus, select:focus{
      border-color: rgba(59,130,246,.6);
      box-shadow: 0 0 0 3px var(--focus);
      background: var(--inputBg2);
    }

    textarea{ min-height:80px; resize:vertical; }
    .requiredHint{ font-size:12px; color: var(--muted); }

    .fieldError input, .fieldError textarea, .fieldError select{
      border-color: var(--danger) !important;
      box-shadow: 0 0 0 3px rgba(239,68,68,.22);
    }
    .errorText{ font-size:12px; color: var(--danger); display:none; }
    .fieldError .errorText{ display:block; }

    .optionalHint{
      font-size:12px;
      color: #fbbf24;
      background: rgba(245,158,11,.12);
      border: 1px solid var(--warnBorder);
      padding: 2px 8px;
      border-radius:999px;
      width:fit-content;
    }
    .optionalField input, .optionalField textarea, .optionalField select{
      border-color: rgba(245,158,11,.40);
      box-shadow: 0 0 0 3px rgba(245,158,11,.12);
    }

    .row{ display:flex; gap:10px; flex-wrap:wrap; align-items:center; }

    button{
      border: 1px solid rgba(255,255,255,.12);
      border-radius:10px;
      padding:10px 14px;
      font-weight:700;
      cursor:pointer;
      background: rgba(255,255,255,.06);
      color: var(--text);
      transition: transform .06s, background .15s, border-color .15s;
    }
    button:hover{ background: rgba(255,255,255,.09); border-color: rgba(255,255,255,.18); }
    button:active{ transform: translateY(1px); }

    button.success{
      background: rgba(34,197,94,.16);
      border-color: rgba(34,197,94,.28);
    }
    button.success:disabled{ opacity:.55; cursor:not-allowed; }

    #copyStatus{ font-size:13px; color:#22c55e; font-weight:800; display:none; }
    .muted{ color: var(--muted); font-size:13px; }

    .out{
      white-space:pre-wrap;
      font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, "Liberation Mono", monospace;
      font-size:13px;
      line-height:1.35;
      background: #050914;
      color: #e5e7eb;
      border: 1px solid rgba(255,255,255,.10);
      border-radius:14px;
      padding:16px;
    }

    .badge{
      font-size:12px;
      padding:3px 8px;
      border-radius:999px;
      background: rgba(99,102,241,.16);
      color: #c7d2fe;
      border: 1px solid rgba(99,102,241,.22);
    }

    .ktBadge{
      font-size:12px;
      padding:8px 12px;
      border-radius:999px;
      font-weight:900;
      letter-spacing:.3px;
      background: linear-gradient(135deg, rgba(99,102,241,.75), rgba(59,130,246,.55));
      color:#fff;
      box-shadow: 0 10px 22px rgba(0,0,0,.35);
      border: 1px solid rgba(255,255,255,.16);
      text-transform: uppercase;
      white-space: nowrap;
    }

    @media (max-width: 760px){ .grid{ grid-template-columns:1fr; } }

    .dirRow{ display:flex; gap:10px; align-items:center; flex-wrap:wrap; }
    .dirPill{
      display:flex; align-items:center; gap:8px;
      padding: 8px 10px;
      border: 1px solid var(--border2);
      border-radius:999px;
      background: rgba(255,255,255,.05);
      cursor:pointer; user-select:none;
    }
    .dirPill:hover{ background: rgba(255,255,255,.08); }
    .dirPill input{ margin:0; accent-color:#60a5fa; }

    .fineBox{
      display:flex; gap:10px; align-items:center; flex-wrap:wrap;
      padding:10px 12px;
      border:1px dashed rgba(255,255,255,.18);
      border-radius:12px;
      background: rgba(255,255,255,.03);
      margin-top:12px;
    }
  </style>
</head>

<body>
  <div class="wrap">
    <div class="card">
      <h1>
        <span class="titleLeft">
          ⭐ NARCO COUNTY SHERIFF DEPARTMENT ⭐
          <span class="badge">Bußgeldbescheid-Generator</span>
        </span>
        <span class="ktBadge">Kompetenzteam</span>
      </h1>

      <p class="muted">
        Dark Mode aktiv. Datum/Uhrzeit laufen live. Bußgeld wird automatisch anhand der Überschreitung berechnet.
      </p>

      <div class="grid" style="margin-top:12px;">
        <label>
          Datum (auto – live)
          <input id="date" type="text" readonly />
          <span class="requiredHint">&nbsp;</span>
        </label>

        <label>
          Uhrzeit (auto – live)
          <input id="time" type="text" readonly />
          <span class="requiredHint">&nbsp;</span>
        </label>

        <label>
          Ort / Einsatzgebiet
          <input id="locationBase" type="text" value="Jägerjob Strasse, 1077 PLZ" />
          <span class="requiredHint">Fahrtrichtung wird automatisch angehängt.</span>
        </label>

        <label class="optionalField">
          Fahrtrichtung <span class="optionalHint">Optional</span>
          <div class="dirRow">
            <label class="dirPill"><input type="radio" name="direction" id="dirNorth" value="Norden"> Norden</label>
            <label class="dirPill"><input type="radio" name="direction" id="dirSouth" value="Süden"> Süden</label>
          </div>
          <span class="requiredHint">Wenn keine Auswahl, wird keine Fahrtrichtung angezeigt.</span>
        </label>

        <label id="wrapLimit">
          Zulässige Höchstgeschwindigkeit (km/h) <span class="requiredHint">(Pflicht)</span>
          <input id="limit" type="number" min="0" step="1" value="130" />
          <span class="errorText">Pflichtfeld fehlt oder ist ungültig.</span>
        </label>

        <label id="wrapMeasured">
          Gemessene Geschwindigkeit (km/h) <span class="requiredHint">(Pflicht)</span>
          <input id="measured" type="number" min="0" step="1" placeholder="z.B. 170" />
          <span class="errorText">Bitte eine gemessene Geschwindigkeit eintragen.</span>
        </label>

        <label id="wrapTolerance">
          Toleranzabzug (km/h) <span class="requiredHint">(Pflicht)</span>
          <input id="tolerance" type="number" min="0" step="1" value="10" />
          <span class="errorText">Pflichtfeld fehlt oder ist ungültig.</span>
        </label>

        <label class="optionalField">
          Kennzeichen <span class="optionalHint">Optional</span>
          <input id="plate" type="text" placeholder="z.B. NC-12345" />
          <span class="requiredHint">Wenn leer, wird „FAHRZEUGDATEN“ ausgelassen.</span>
        </label>

        <label class="optionalField">
          Gezeichnet von <span class="optionalHint">Optional</span>
          <select id="officer">
            <option selected>Andrew Petzenstein | SD-126 | Coporal</option>
            <option>Diddy Messerschmidt | SD-154 | Senior Deputy</option>
            <option>Earl Thomas | SD-82 | Coporal</option>
          </select>
          <span class="requiredHint">&nbsp;</span>
        </label>

        <label style="grid-column: 1 / -1;">
          Messverfahren (Text wie im Bescheid)
          <textarea id="method">Die Geschwindigkeitsmessung erfolgte mobil aus einem gekennzeichneten Streifenfahrzeug des Narco County Sheriff Departments mittels fest verbautem Radarmesssystem (Wraith ARS 2 / wk_wars2x Radar-System).
Die Messung erfolgte stationär aus dem Streifenfahrzeug heraus ohne Verfolgungsfahrt.
Es bestand freie und uneingeschränkte Sicht auf das gemessene Fahrzeug.
Zwischenverkehr oder andere Störquellen waren nicht vorhanden.
Das Fahrzeug wurde visuell eindeutig identifiziert.</textarea>
          <span class="requiredHint">&nbsp;</span>
        </label>
      </div>

      <div class="fineBox">
        <div><strong>Auto-Bußgeld:</strong></div>
        <div class="muted">Überschreitung:</div>
        <div><strong id="overDisplay">–</strong></div>
        <div class="muted">→ Betrag:</div>
        <div><strong id="fineDisplay">–</strong></div>
      </div>

      <div class="row" style="margin-top:12px;">
        <button id="btnGenerate">Bescheid erstellen / aktualisieren</button>
        <button class="success" id="btnCopy" disabled>Bescheid kopieren</button>
        <span id="copyStatus">✅ Kopiert!</span>
        <span class="muted" id="calcHint"></span>
      </div>
    </div>

    <div class="card" id="previewCard">
      <h2>Vorschau</h2>
      <div class="out" id="output">Noch kein Bescheid erstellt.</div>
    </div>
  </div>

  <script>
    function pad2(n){ return String(n).padStart(2,"0"); }

    function updateNowLive() {
      const d = new Date();
      const day = pad2(d.getDate());
      const month = pad2(d.getMonth() + 1);
      const year = d.getFullYear();
      const hours = pad2(d.getHours());
      const mins = pad2(d.getMinutes());
      const secs = pad2(d.getSeconds());

      document.getElementById("date").value = `${day}.${month}.${year}`;
      document.getElementById("time").value = `${hours}:${mins}:${secs} Uhr`;
    }

    function numberOrNull(id) {
      const v = document.getElementById(id).value;
      if (v === "" || v === null || v === undefined) return null;
      const n = Number(v);
      return Number.isFinite(n) ? n : null;
    }

    function fmtMoneyUSD(n){
      const s = Math.round(n).toString();
      return s.replace(/\B(?=(\d{3})+(?!\d))/g, ".") + " $";
    }

    function setFieldError(wrapperId, hasError){
      const el = document.getElementById(wrapperId);
      if (!el) return;
      el.classList.toggle("fieldError", !!hasError);
    }

    async function copyToClipboard(text){
      if (navigator.clipboard && window.isSecureContext) {
        await navigator.clipboard.writeText(text);
        return;
      }
      const ta = document.createElement("textarea");
      ta.value = text;
      ta.style.position = "fixed";
      ta.style.left = "-9999px";
      document.body.appendChild(ta);
      ta.select();
      document.execCommand("copy");
      document.body.removeChild(ta);
    }

    function getDirectionText(){
      if (document.getElementById("dirNorth").checked) return "Fahrtrichtung Norden";
      if (document.getElementById("dirSouth").checked) return "Fahrtrichtung Süden";
      return "";
    }

    function getLocationWithDirection(){
      const base = document.getElementById("locationBase").value.trim();
      const dir = getDirectionText();
      if (!dir) return base;
      return `${base} (${dir})`;
    }

    // Bußgeldlogik (41+ entfernt, 31+ = 15.000)
    function calcFineByOver(overKmH){
      if (!Number.isFinite(overKmH) || overKmH <= 0) return 0;
      if (overKmH <= 10) return 5000;
      if (overKmH <= 20) return 8000;
      if (overKmH <= 30) return 12000;
      return 15000; // 31+
    }

    function generate() {
      const date = document.getElementById("date").value.trim();
      const time = document.getElementById("time").value.trim();

      const location = getLocationWithDirection();
      const limit = numberOrNull("limit");
      const measured = numberOrNull("measured");
      const tolerance = numberOrNull("tolerance");

      const plate = document.getElementById("plate").value.trim();
      const officer = document.getElementById("officer").value.trim();
      const method = document.getElementById("method").value.trim();

      // Pflichtfelder
      const errMeasured = (measured === null);
      const errLimit = (limit === null);
      const errTol = (tolerance === null);

      setFieldError("wrapMeasured", errMeasured);
      setFieldError("wrapLimit", errLimit);
      setFieldError("wrapTolerance", errTol);

      const btnCopy = document.getElementById("btnCopy");

      if (errMeasured || errLimit || errTol) {
        const errors = [];
        if (errMeasured) errors.push("Gemessene Geschwindigkeit fehlt.");
        if (errLimit) errors.push("Höchstgeschwindigkeit fehlt.");
        if (errTol) errors.push("Toleranzabzug fehlt.");

        document.getElementById("output").textContent = "❗ Bitte ergänzen:\n- " + errors.join("\n- ");
        document.getElementById("calcHint").textContent = "";
        btnCopy.disabled = true;
        btnCopy.dataset.copyText = "";

        document.getElementById("overDisplay").textContent = "–";
        document.getElementById("fineDisplay").textContent = "–";
        return;
      }

      const usable = Math.max(0, measured - tolerance);
      const over = Math.max(0, usable - limit);
      const fineAuto = calcFineByOver(over);

      document.getElementById("overDisplay").textContent = `${over} km/h`;
      document.getElementById("fineDisplay").textContent = fmtMoneyUSD(fineAuto);

      document.getElementById("calcHint").textContent =
        `Berechnung: verwertbar ${usable} km/h | Überschreitung ${over} km/h | Bußgeld ${fmtMoneyUSD(fineAuto)}`;

      // Fahrzeugdaten-Block nur wenn Kennzeichen vorhanden
      let vehicleBlock = "";
      if (plate.length > 0) {
        vehicleBlock =
`〚 FAHRZEUGDATEN 〛

Kennzeichen: ${plate}

`;
      }

      const paragraphLine = `§ 2 Abs. 07 StVO - ${date}`;

      const text =
`⭐ NARCO COUNTY SHERIFF DEPARTMENT ⭐

〚 DATEN ZUM VERSTOSS 〛

Datum: ${date}

Uhrzeit: ${time}
Ort: ${location}

Im betroffenen Streckenabschnitt gilt eine zulässige Höchstgeschwindigkeit von ${limit} km/h.

〚 MESSVERFAHREN 〛
${method}

〚 MESSDATEN 〛
Gemessene Geschwindigkeit: ${measured} km/h
Gesetzlicher Toleranzabzug: ${tolerance} km/h
Verwertbare Geschwindigkeit: ${usable} km/h
Tatsächliche Überschreitung: ${over} km/h

${vehicleBlock}〚 RECHTSFOLGE 〛
Gemäß dem geltenden Bußgeldkatalog vom Staat wird folgende Maßnahme verhängt: 
⠂ Bußgeldbetrag: ${fmtMoneyUSD(fineAuto)}
⠂ Zusätzliche Maßnahmen: Keine

〚 GEZEICHNET VON 〛
${officer}

${paragraphLine}`;

      document.getElementById("output").textContent = text;

      btnCopy.disabled = false;
      btnCopy.dataset.copyText = text;
    }

    // Live Datum/Uhrzeit + Live-Preview
    updateNowLive();
    setInterval(() => { updateNowLive(); generate(); }, 1000);

    document.getElementById("btnGenerate").addEventListener("click", generate);

    document.getElementById("btnCopy").addEventListener("click", async () => {
      const text = document.getElementById("btnCopy").dataset.copyText || "";
      if (!text) return;
      try{
        await copyToClipboard(text);
        const st = document.getElementById("copyStatus");
        st.style.display = "inline";
        setTimeout(() => st.style.display = "none", 1200);
      } catch(e){
        alert("Kopieren nicht möglich (Browser-Restriktion). Bitte manuell markieren.");
      }
    });

    ["locationBase","limit","measured","tolerance","plate","officer","method","dirNorth","dirSouth"]
      .forEach(id => document.getElementById(id).addEventListener("input", generate));
    ["dirNorth","dirSouth"].forEach(id => document.getElementById(id).addEventListener("change", generate));
  </script>
</body>
</html>
