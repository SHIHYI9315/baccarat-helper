# baccarat-helper
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <title>百家樂輔助工具</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; background-color: #f9f9f9; }
        button { margin: 5px; padding: 10px 20px; font-size: 16px; }
        #history, #stats, #advice { margin-top: 20px; font-size: 18px; }
    </style>
</head>
<body>
    <h1>百家樂輔助工具</h1>

    <button onclick="record('莊')">莊</button>
    <button onclick="record('閒')">閒</button>
    <button onclick="record('和')">和</button>
    <button onclick="reset()">重置</button>

    <div id="history"></div>
    <div id="stats"></div>
    <div id="advice"></div>

    <script>
        let history = JSON.parse(localStorage.getItem('baccaratHistory')) || [];

        function record(result) {
            history.push(result);
            localStorage.setItem('baccaratHistory', JSON.stringify(history));
            updateDisplay();
        }

        function reset() {
            history = [];
            localStorage.removeItem('baccaratHistory');
            updateDisplay();
        }

        function updateDisplay() {
            document.getElementById('history').innerText = "路單: " + history.join(' - ');
            const count = { "莊": 0, "閒": 0, "和": 0 };
            history.forEach(r => count[r]++);
            const total = history.length || 1;
            document.getElementById('stats').innerText = `統計：莊 ${count["莊"]} (${(count["莊"]/total*100).toFixed(1)}%) | 閒 ${count["閒"]} (${(count["閒"]/total*100).toFixed(1)}%) | 和 ${count["和"]} (${(count["和"]/total*100).toFixed(1)}%)`;

            const recent = history.slice(-5);
            const rc = { "莊": 0, "閒": 0, "和": 0 };
            recent.forEach(r => rc[r]++);
            let suggestion = "建議：";
            if (rc["莊"] > rc["閒"]) {
                suggestion += "可考慮 閒";
            } else if (rc["閒"] > rc["莊"]) {
                suggestion += "可考慮 莊";
            } else {
                suggestion += "自行判斷";
            }
            document.getElementById('advice').innerText = suggestion;
        }

        updateDisplay();
    </script>
</body>
</html>
