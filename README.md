<!doctype html>
<html lang="tr">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Açıyla Yükseklik Ölçer</title>
<style>
  body {
    font-family: "Poppins", sans-serif;
    background: linear-gradient(135deg, #012E40, #026773);
    color: #F2E3D5;
    text-align: center;
    padding: 2rem;
  }
  h2 {
    color: #3CA6A6;
    font-size: 1.8rem;
    margin-bottom: 1rem;
  }
  input {
    border: none;
    border-radius: 8px;
    padding: 0.5rem;
    width: 5rem;
    text-align: center;
    font-size: 1rem;
    margin: 0.5rem;
  }
  #startBtn {
    background-color: #3CA6A6;
    color: #012E40;
    border: none;
    border-radius: 10px;
    padding: 0.6rem 1rem;
    font-size: 1rem;
    margin-top: 1rem;
    cursor: pointer;
  }
  #startBtn:hover {
    background-color: #F2E3D5;
    color: #012E40;
  }
  .data-box {
    background-color: rgba(242, 227, 213, 0.1);
    border: 1px solid #3CA6A6;
    border-radius: 10px;
    display: inline-block;
    margin-top: 1rem;
    padding: 1rem 2rem;
  }
  .value {
    font-weight: bold;
    color: #3CA6A6;
    font-size: 1.2rem;
  }
</style>
</head>

<body>
  <h2>Açıyla Yükseklik Ölçer 📐</h2>
  <p>Telefonu yere paralel tutup cismin tepesine nişan al.  
  Mesafeyi gir, ardından “Başlat” butonuna bas.</p>

  <label>Mesafe (m):</label>
  <input id="d" value="2" type="number" step="0.1"><br>

  <button id="startBtn">Başlat</button>

  <div class="data-box">
    <p>Telefon Açısı: <span id="pitch" class="value">—</span>°</p>
    <p>Hesaplanan Yükseklik: <span id="h" class="value">—</span> m</p>
  </div>

  <script>
    const pitchEl = document.getElementById("pitch");
    const heightEl = document.getElementById("h");
    const startBtn = document.getElementById("startBtn");

    function toRad(deg) {
      return deg * Math.PI / 180;
    }

    function handleOrientation(e) {
      let pitch = e.beta || 0; // öne/arkaya eğim
      let distance = parseFloat(document.getElementById("d").value) || 1;

      pitchEl.innerText = pitch.toFixed(1);
      let height = Math.tan(toRad(pitch)) * distance;
      heightEl.innerText = height.toFixed(2);
    }

    function startMeasurement() {
      if (typeof DeviceOrientationEvent.requestPermission === "function") {
        // iOS 13+ için izin
        DeviceOrientationEvent.requestPermission()
          .then(response => {
            if (response === "granted") {
              window.addEventListener("deviceorientation", handleOrientation);
              startBtn.innerText = "Ölçüm Aktif ✅";
              startBtn.disabled = true;
            } else {
              alert("Lütfen sensör erişimine izin ver.");
            }
          })
          .catch(() => alert("Cihaz sensörüne erişilemedi."));
      } else {
        // Android veya izin gerektirmeyen tarayıcılar
        window.addEventListener("deviceorientation", handleOrientation);
        startBtn.innerText = "Ölçüm Aktif ✅";
        startBtn.disabled = true;
      }
    }

    startBtn.addEventListener("click", startMeasurement);
  </script>
</body>
</html>
