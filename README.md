<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>電腦狀態監控儀表板</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.1/normalize.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        /* 保持原有的 CSS 不變 */
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f7fa;
            color: #333;
            padding: 20px;
        }
        .dashboard-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
        }
        .computer-card {
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            padding: 20px;
            transition: box-shadow 0.3s ease;
        }
        .computer-card.alert {
            border: 2px solid #ff0000;
            animation: pulse 1s infinite;
        }
        .computer-card.warning {
            border: 2px solid #ffcc00;
            background-color: #fff8e1;
        }
        @keyframes pulse {
            0% {
                box-shadow: 0 0 5px #ff0000;
            }
            50% {
                box-shadow: 0 0 20px #ff0000;
            }
            100% {
                box-shadow: 0 0 5px #ff0000;
            }
        }
        .computer-card:hover {
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.15);
        }
        .computer-name {
            font-size: 1.2em;
            margin-bottom: 10px;
            font-weight: bold;
        }
        .cpu-usage, .memory-usage, .disk-space {
            margin-bottom: 10px;
            font-size: 1em;
        }
        .progress-bar {
            background-color: #e0e0e0;
            border-radius: 10px;
            overflow: hidden;
            height: 20px;
            margin-bottom: 5px;
        }
        .progress-bar-inner {
            height: 100%;
            text-align: center;
            line-height: 20px;
            color: #fff;
            border-radius: 10px;
        }
        .cpu-bar-inner {
            background-color: #ff6b6b;
        }
        .memory-bar-inner {
            background-color: #4caf50;
        }
        .disk-bar-inner {
            background-color: #2196f3;
        }
    </style>
</head>
<body>
    <h1>電腦狀態監控儀表板</h1>
    <div class="dashboard-container" id="dashboard">
        <!-- 電腦卡片將動態生成 -->
    </div>

    <script>
        // 後端 API URL
        const backendApiUrl = 'https://3701622c9ed9.ngrok.app/api/systeminfo';

        // 更新間隔時間（毫秒）
        const updateInterval = 1000; // 可以自由設定的參數，例如每 1000 毫秒（1 秒）更新一次

        // 取得 API 數據
        async function fetchComputerData() {
            try {
                const response = await fetch(backendApiUrl);
                if (!response.ok) {
                    throw new Error('Failed to fetch data from backend');
                }
                const computers = await response.json();
                updateDashboard(computers);
            } catch (error) {
                console.error(error);
                // 顯示錯誤訊息
                const dashboard = document.getElementById("dashboard");
                dashboard.innerHTML = `<div class="computer-card warning">
                    <div class="computer-name">錯誤</div>
                    <div>無法取得監控數據，請稍後再試。</div>
                </div>`;
            }
        }

        function createComputerCard(computer) {
            const card = document.createElement("div");
            card.classList.add("computer-card");

            // 如果是無法取得數據的情況，標示為警告
            if (computer.error) {
                card.classList.add("warning");
                const name = document.createElement("div");
                name.classList.add("computer-name");
                name.textContent = computer.computerName;
                const warningMessage = document.createElement("div");
                warningMessage.textContent = computer.message || "無法取得數據，該主機可能停機。";
                card.appendChild(name);
                card.appendChild(warningMessage);
                return card;
            }

            // 門檻值設置
            const cpuThreshold = 80;
            const memoryThreshold = 2; // GB
            const diskThreshold = 10; // GB

            // 檢查是否超過門檻
            if (computer.cpuUsage > cpuThreshold || computer.availableMemoryGB < memoryThreshold || computer.availableDiskSpaceGB < diskThreshold) {
                card.classList.add("alert");
            }

            const name = document.createElement("div");
            name.classList.add("computer-name");
            name.textContent = computer.computerName;

            const ipAddress = document.createElement("div");
            ipAddress.classList.add("ip-address");
            ipAddress.innerHTML = `IP 位址: ${computer.ipAddress}`;

            const cpuUsage = document.createElement("div");
            cpuUsage.classList.add("cpu-usage");
            cpuUsage.innerHTML = `CPU 使用率: ${computer.cpuUsage.toFixed(2)}%`;

            const cpuProgressBar = document.createElement("div");
            cpuProgressBar.classList.add("progress-bar");
            const cpuProgressInner = document.createElement("div");
            cpuProgressInner.classList.add("progress-bar-inner", "cpu-bar-inner");
            cpuProgressInner.style.width = `${computer.cpuUsage}%`;
            cpuProgressInner.textContent = `${computer.cpuUsage.toFixed(2)}%`;
            cpuProgressBar.appendChild(cpuProgressInner);

            const memoryUsage = document.createElement("div");
            memoryUsage.classList.add("memory-usage");
            memoryUsage.innerHTML = `剩餘可用記憶體: ${computer.availableMemoryGB.toFixed(2)} GB`;

            const memoryProgressBar = document.createElement("div");
            memoryProgressBar.classList.add("progress-bar");
            const memoryProgressInner = document.createElement("div");
            memoryProgressInner.classList.add("progress-bar-inner", "memory-bar-inner");
            const memoryPercentage = 100-(((computer.availableMemoryGB - memoryThreshold) / computer.availableMemoryGB) * 100);
            memoryProgressInner.style.width = `${Math.min(memoryPercentage, 100)}%`;
            memoryProgressInner.textContent = `${Math.min(memoryPercentage.toFixed(1), 100)}%`;
            memoryProgressBar.appendChild(memoryProgressInner);

            const diskSpace = document.createElement("div");
            diskSpace.classList.add("disk-space");
            diskSpace.innerHTML = `剩餘磁碟空間: ${computer.availableDiskSpaceGB.toFixed(2)} GB`;

            const diskProgressBar = document.createElement("div");
            diskProgressBar.classList.add("progress-bar");
            const diskProgressInner = document.createElement("div");
            diskProgressInner.classList.add("progress-bar-inner", "disk-bar-inner");
            const diskPercentage = 100-(((computer.availableDiskSpaceGB - diskThreshold) / computer.availableDiskSpaceGB) * 100);
            diskProgressInner.style.width = `${Math.min(diskPercentage, 100)}%`;
            diskProgressInner.textContent = `${Math.min(diskPercentage.toFixed(1), 100)}%`;
            diskProgressBar.appendChild(diskProgressInner);

            card.appendChild(name);
            card.appendChild(ipAddress);
            card.appendChild(cpuUsage);
            card.appendChild(cpuProgressBar);
            card.appendChild(memoryUsage);
            card.appendChild(memoryProgressBar);
            card.appendChild(diskSpace);
            card.appendChild(diskProgressBar);

            return card;
        }

        function updateDashboard(computers) {
            const dashboard = document.getElementById("dashboard");
            dashboard.innerHTML = "";
            computers.forEach(computer => {
                const card = createComputerCard(computer);
                dashboard.appendChild(card);
            });
        }

        // 初始化，呼叫後端 API 並定期更新儀表板
        fetchComputerData(); // 立即呼叫一次
        setInterval(fetchComputerData, updateInterval);
    </script>
</body>
</html>
