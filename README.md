<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>تحليل المباراة</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f0f4f8;
      direction: rtl;
      text-align: center;
      padding: 40px;
    }
    h1 {
      color: #2c3e50;
    }
    input, button {
      padding: 10px;
      font-size: 16px;
      margin: 10px;
      width: 200px;
      border: 1px solid #ccc;
      border-radius: 8px;
    }
    button {
      background-color: #27ae60;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background-color: #219150;
    }
    .result {
      margin-top: 30px;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      white-space: pre-wrap;
    }
  </style>
</head>
<body>

  <h1>🔍 تحليل المباراة</h1>

  <input type="text" id="team1" placeholder="الفريق 1">
  <br>
  <input type="text" id="team2" placeholder="الفريق 2">
  <br>
  <button onclick="analyzeMatch()">حلل 👇</button>

  <div class="result" id="output"></div>

  <script>
    const apiKey = 8dfb10efdaba416d9293da7581bd0a26; // استبدل بـ API Key ديالك
    const baseUrl = "https://api.football-data.org/v2/";

    async function getLiveMatches() {
      const response = await fetch(`${baseUrl}matches`, {
        headers: { "X-Auth-Token": apiKey },
      });
      const data = await response.json();
      return data.matches;
    }

    function getRandom(min, max) {
      return (Math.random() * (max - min) + min).toFixed(1);
    }

    function analyzeMatch() {
      const team1 = document.getElementById("team1").value.trim();
      const team2 = document.getElementById("team2").value.trim();
      const output = document.getElementById("output");

      if (!team1 || !team2) {
        output.textContent = "⚠️ عفاك دخل أسماء الفريقين!";
        return;
      }

      // جلب المباريات الحية
      getLiveMatches().then((matches) => {
        const match = matches.find((m) =>
          (m.homeTeam.name === team1 && m.awayTeam.name === team2) ||
          (m.homeTeam.name === team2 && m.awayTeam.name === team1)
        );

        if (match) {
          const avg1 = getRandom(0.5, 3.0);
          const avg2 = getRandom(0.5, 3.0);
          const win1 = Math.floor(Math.random() * 51) + 25;
          const win2 = 100 - win1;

          const btts = (avg1 > 1.2 && avg2 > 1.2) ? "✅ توقع: الفريقان سيسجلان (BTTS)" : "❌ توقع: مباراة مغلقة";

          output.textContent = `📊 تحليل المباراة بين ${team1} و ${team2}:\n
⚽️ أهداف متوقعة:
- ${team1}: ${avg1}
- ${team2}: ${avg2}

🔮 التوقع:
- فوز ${team1}: ${win1}%
- فوز ${team2}: ${win2}%

${btts}`;
        } else {
          output.textContent = "⚠️ المباراة غير موجودة في المباريات الحالية.";
        }
      });
    }
  </script>

</body>
</html>

