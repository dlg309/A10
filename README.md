<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Cookie Demo</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: Arial, sans-serif;
      background: #f0f4f8;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
    }

    .card {
      background: #ffffff;
      border-radius: 12px;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
      max-width: 520px;
      width: 100%;
      padding: 36px 40px;
    }

    h1 {
      font-size: 1.6rem;
      color: #1f3864;
      margin-bottom: 6px;
    }

    .subtitle {
      font-size: 0.9rem;
      color: #666;
      margin-bottom: 28px;
    }

    label {
      display: block;
      font-size: 0.88rem;
      font-weight: bold;
      color: #333;
      margin-bottom: 6px;
    }

    input[type="text"] {
      width: 100%;
      padding: 10px 14px;
      border: 1.5px solid #ccc;
      border-radius: 8px;
      font-size: 1rem;
      outline: none;
      transition: border-color 0.2s;
      margin-bottom: 16px;
    }

    input[type="text"]:focus {
      border-color: #2e74b5;
    }

    .row {
      display: flex;
      gap: 10px;
      margin-bottom: 28px;
    }

    button {
      flex: 1;
      padding: 11px;
      border: none;
      border-radius: 8px;
      font-size: 0.95rem;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.2s, transform 0.1s;
    }

    button:active {
      transform: scale(0.97);
    }

    .btn-set {
      background: #2e74b5;
      color: white;
    }

    .btn-set:hover {
      background: #1f5490;
    }

    .btn-clear {
      background: #e8f0fe;
      color: #2e74b5;
      border: 1.5px solid #2e74b5;
    }

    .btn-clear:hover {
      background: #d0e3fb;
    }

    .cookie-display {
      background: #f7f9fc;
      border: 1.5px solid #dde3ed;
      border-radius: 8px;
      padding: 18px 20px;
    }

    .cookie-display h2 {
      font-size: 0.85rem;
      text-transform: uppercase;
      letter-spacing: 0.06em;
      color: #888;
      margin-bottom: 10px;
    }

    .cookie-value {
      font-size: 1.05rem;
      color: #1f3864;
      font-weight: bold;
      word-break: break-all;
    }

    .cookie-value.empty {
      color: #aaa;
      font-weight: normal;
      font-style: italic;
    }

    .cookie-meta {
      font-size: 0.8rem;
      color: #999;
      margin-top: 8px;
    }

    .status-msg {
      font-size: 0.85rem;
      margin-top: 14px;
      min-height: 20px;
      color: #2e74b5;
      font-style: italic;
    }
  </style>
</head>
<body>

<div class="card">
  <h1>Cookie Demo</h1>
  <p class="subtitle">IS 117 &mdash; A10</p>

  <label for="nameInput">Enter your name:</label>
  <input type="text" id="nameInput" placeholder="e.g. Name" />

  <div class="row">
    <button class="btn-set" onclick="saveCookie()">Set Cookie</button>
    <button class="btn-clear" onclick="clearCookie()">Clear Cookie</button>
  </div>

  <div class="cookie-display">
    <h2>Stored Cookie Value</h2>
    <div class="cookie-value" id="cookieOutput">Loading…</div>
    <div class="cookie-meta" id="cookieMeta"></div>
  </div>

  <div class="status-msg" id="statusMsg"></div>
</div>

<script>
  function setCookie(name, value, days) {
    var expires = "";
    if (days) {
      var d = new Date();
      d.setTime(d.getTime() + days * 24 * 60 * 60 * 1000);
      expires = "; expires=" + d.toUTCString();
    }
    document.cookie = name + "=" + encodeURIComponent(value) + expires + "; path=/";
  }

  function getCookie(name) {
    var nameEQ = name + "=";
    var ca = document.cookie.split(";");
    for (var i = 0; i < ca.length; i++) {
      var c = ca[i].trim();
      if (c.indexOf(nameEQ) === 0) {
        return decodeURIComponent(c.substring(nameEQ.length));
      }
    }
    return null;
  }

  function deleteCookie(name) {
    document.cookie = name + "=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/";
  }


  function saveCookie() {
    var name = document.getElementById("nameInput").value.trim();
    if (!name) {
      setStatus("Please enter a name first.", "#c0392b");
      return;
    }
    setCookie("visitorName", name, 7); // expires in 7 days
    displayCookie();
    setStatus("Cookie set! It will persist for 7 days.", "#2e74b5");
    document.getElementById("nameInput").value = "";
  }

  function clearCookie() {
    deleteCookie("visitorName");
    displayCookie();
    setStatus("Cookie cleared.", "#888");
  }

  function displayCookie() {
    var val = getCookie("visitorName");
    var output = document.getElementById("cookieOutput");
    var meta   = document.getElementById("cookieMeta");

    if (val) {
      output.textContent = val;
      output.classList.remove("empty");
      meta.textContent = 'Raw cookie string: "visitorName=' + encodeURIComponent(val) + '"';
    } else {
      output.textContent = "No cookie found";
      output.classList.add("empty");
      meta.textContent = "";
    }
  }

  function setStatus(msg, color) {
    var el = document.getElementById("statusMsg");
    el.textContent = msg;
    el.style.color = color;
  }

  // ── On page load: display any existing cookie ─────────────────────────────
  window.onload = function () {
    displayCookie();
    var existing = getCookie("visitorName");
    if (existing) {
      setStatus('Welcome back, ' + existing + '! (cookie loaded from previous visit)', "#27ae60");
    }
  };
</script>

  <div class="status-msg" id="statusMsg"></div>
  <footer>A10 2026 - IS 117 - Diego Guevara</footer>
</div>
</body>
</html>
