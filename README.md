<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minería Pambicoin</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            text-align: center;
            padding: 20px;
            margin: 0;
        }
        .container {
            max-width: 800px;
            margin: auto;
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            text-align: left;
        }
        canvas {
            max-width: 100%;
            margin-bottom: 20px;
        }
        .button {
            display: inline-block;
            padding: 10px 20px;
            background-color: #007bff;
            color: #fff;
            text-decoration: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 10px;
        }
        .button:hover {
            background-color: #0056b3;
        }
        #miningStatus {
            margin-bottom: 20px;
            display: none; /* Oculto inicialmente */
        }
        #comment {
            font-style: italic;
            margin-top: 20px;
            display: none; /* Oculto inicialmente */
        }
        #blocksList, #earnings {
            display: none; /* Oculto inicialmente */
        }
        .footer {
            font-size: 12px;
            color: #777;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Minería Pambicoin</h1>
        <p>Bienvenidos al sistema de minería Pambi. Aquí podréis ver gráficos de rendimiento de la CPU, bloques encontrados y dinero ganado por bloque minado.</p>
        
        <button id="startMining" class="button">Iniciar Minería</button>
        <p id="comment">Mis queridos pambisitos vais a ser millonarios.</p>
        
        <div id="miningStatus">
            <canvas id="cpuChart"></canvas>
            <p>Bloques encontrados: <span id="blocksFound">0</span></p>
            <p>Nodos conectados: <span id="connectedNodes">100</span></p>
        </div>
        
        <button id="stopMining" class="button" style="display: none;">Detener Minería</button>
        
        <div id="earnings">
            <h2>Ganancias Acumuladas</h2>
            <ul id="earningsList"></ul>
        </div>

        <div id="blocksList">
            <h2>Detalles de los Últimos Bloques</h2>
            <ul id="blocksDetails"></ul>
        </div>

        <p class="footer">Este software está desarrollado por la Calvocracia Asertiva.</p>
    </div>
    
    <script>
        let miningInterval;
        let blocksFound = 0;
        let moneyEarned = 0;
        let cpuData = [];
        let earningsList = document.getElementById('earningsList');
        let blocksDetails = document.getElementById('blocksDetails');

        // Genera datos de rendimiento de CPU aleatorios
        function generateCPUData() {
            return Math.floor(Math.random() * (80 - 20 + 1)) + 20; // Rango entre 20% y 80% de uso
        }

        // Simula la búsqueda de un nuevo bloque
        function findNewBlock() {
            let blockHash = Math.random().toString(36).substring(7); // Simula un hash de bloque aleatorio
            let timestamp = new Date().toLocaleTimeString(); // Hora actual
            let algorithm = "SHA-256"; // Algoritmo utilizado (ejemplo)
            return { blockHash, timestamp, algorithm };
        }

        // Actualiza el gráfico de rendimiento de CPU
        function updateCPUChart() {
            let cpuUsage = generateCPUData();
            cpuData.push(cpuUsage);
            if (cpuData.length > 10) {
                cpuData.shift();
            }
            cpuChart.data.datasets[0].data = cpuData;
            cpuChart.update();
            return cpuUsage;
        }

        // Actualiza la lista de ganancias acumuladas
        function updateEarningsList(amount) {
            let listItem = document.createElement('li');
            listItem.textContent = `+ ${amount} euros en Pambicoin`;
            earningsList.appendChild(listItem);
        }

        // Actualiza la lista de detalles de los bloques encontrados
        function updateBlocksList(blockDetails) {
            let listItem = document.createElement('li');
            listItem.textContent = `Bloque Hash: ${blockDetails.blockHash}, Algoritmo: ${blockDetails.algorithm}, Hora: ${blockDetails.timestamp}`;
            blocksDetails.appendChild(listItem);
        }

        // Configuración inicial del gráfico de CPU
        const ctx = document.getElementById('cpuChart').getContext('2d');
        const cpuChart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: Array.from(Array(10).keys()),
                datasets: [{
                    label: 'Rendimiento de CPU (%)',
                    backgroundColor: 'rgba(0, 123, 255, 0.2)',
                    borderColor: 'rgba(0, 123, 255, 1)',
                    borderWidth: 1,
                    data: cpuData,
                }]
            },
            options: {
                responsive: true,
                scales: {
                    y: {
                        suggestedMin: 0,
                        suggestedMax: 100
                    }
                }
            }
        });

        // Función para mostrar el mensaje después de hacer clic en "Iniciar Minería"
        function showWelcomeMessage() {
            document.getElementById('comment').style.display = 'block';
        }

        // Función para simular la minería
        function startMining() {
            showWelcomeMessage();
            document.getElementById('startMining').style.display = 'none';
            document.getElementById('miningStatus').style.display = 'block';
            document.getElementById('earnings').style.display = 'block';
            document.getElementById('blocksList').style.display = 'block';
            document.getElementById('stopMining').style.display = 'inline-block';
            miningInterval = setInterval(function() {
                let cpuUsage = updateCPUChart();
                let newBlock = findNewBlock();
                blocksFound++;
                moneyEarned += 1000; // Cada bloque minado representa 1000 euros en Pambicoin
                document.getElementById('blocksFound').textContent = blocksFound;
                document.getElementById('comment').textContent = `Mis queridos pambisitos vais a ser millonarios. Lleváis ganados ${moneyEarned} euros en Pambicoin.`;
                updateEarningsList(moneyEarned);
                updateBlocksList(newBlock);
            }, 3000); // Simula cada 3 segundos
        }

        // Función para detener la simulación de minería
        function stopMining() {
            clearInterval(miningInterval);
            document.getElementById('startMining').style.display = 'inline-block';
            document.getElementById('stopMining').style.display = 'none';
            document.getElementById('comment').textContent = "¡Cobren sus Pambicoins a Dalas Review!";
        }

        // Eventos de clic para los botones
        document.getElementById('startMining').addEventListener('click', startMining);
        document.getElementById('stopMining').addEventListener('click', stopMining);
    </script>
</body>
</html>
