<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Volatility Trading Bot</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body class="bg-gray-100">
    <div class="container mx-auto px-4 py-8">
        <div class="max-w-md mx-auto bg-white rounded-lg shadow-lg p-6">
            <h1 class="text-2xl font-bold mb-6 text-center">Volatility Trading Bot</h1>
            
            <div class="mb-4">
                <label class="block text-gray-700 mb-2">Volatility Index</label>
                <select id="symbol" class="w-full p-2 border rounded">
                    <option value="V75">V75</option>
                    <option value="V100">V100</option>
                    <option value="V50">V50</option>
                    <option value="V25">V25</option>
                </select>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700 mb-2">Stake Amount (USD)</label>
                <input type="number" id="stake" class="w-full p-2 border rounded" value="10">
            </div>
            
            <div class="mb-6">
                <label class="block text-gray-700 mb-2">API Token</label>
                <input type="password" id="apiToken" class="w-full p-2 border rounded">
            </div>
            
            <div class="flex space-x-4">
                <button id="startBtn" class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600 w-1/2">
                    Start Trading
                </button>
                <button id="stopBtn" class="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600 w-1/2" disabled>
                    Stop Trading
                </button>
            </div>
            
            <div id="status" class="mt-4 text-center"></div>
        </div>
        
        <div class="max-w-md mx-auto mt-6">
            <div id="trades" class="bg-white rounded-lg shadow-lg p-4">
                <h2 class="text-xl font-bold mb-4">Trade History</h2>
                <div id="tradeList" class="space-y-2"></div>
            </div>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/4.0.1/socket.io.js"></script>
    <script>
        const socket = io();
        const userId = 'user_' + Math.random().toString(36).substr(2, 9);
        
        document.getElementById('startBtn').addEventListener('click', async () => {
            const apiToken = document.getElementById('apiToken').value;
            const symbol = document.getElementById('symbol').value;
            const stake = document.getElementById('stake').value;
            
            const response = await fetch('/api/start', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ user_id: userId, api_token: apiToken, symbol, stake })
            });
            
            const data = await response.json();
            document.getElementById('status').textContent = data.message;
            document.getElementById('startBtn').disabled = true;
            document.getElementById('stopBtn').disabled = false;
        });
        
        document.getElementById('stopBtn').addEventListener('click', async () => {
            const response = await fetch('/api/stop', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ user_id: userId })
            });
            
            const data = await response.json();
            document.getElementById('status').textContent = data.message;
            document.getElementById('startBtn').disabled = false;
            document.getElementById('stopBtn').disabled = true;
        });
        
        socket.on('trade_update', (data) => {
            const tradeList = document.getElementById('tradeList');
            const tradeItem = document.createElement('div');
            tradeItem.className = 'p-2 border rounded';
            tradeItem.textContent = `${data.symbol}: ${data.type} @ ${data.price}`;
            tradeList.prepend(tradeItem);
        });
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Volatility Trading Bot</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body class="bg-gray-100">
    <div class="container mx-auto px-4 py-8">
        <div class="max-w-md mx-auto bg-white rounded-lg shadow-lg p-6">
            <h1 class="text-2xl font-bold mb-6 text-center">Volatility Trading Bot</h1>
            
            <div class="mb-4">
                <label class="block text-gray-700 mb-2">Volatility Index</label>
                <select id="symbol" class="w-full p-2 border rounded">
                    <option value="V75">V75</option>
                    <option value="V100">V100</option>
                    <option value="V50">V50</option>
                    <option value="V25">V25</option>
                </select>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700 mb-2">Stake Amount (USD)</label>
                <input type="number" id="stake" class="w-full p-2 border rounded" value="10">
            </div>
            
            <div class="mb-6">
                <label class="block text-gray-700 mb-2">API Token</label>
                <input type="password" id="apiToken" class="w-full p-2 border rounded">
            </div>
            
            <div class="flex space-x-4">
                <button id="startBtn" class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600 w-1/2">
                    Start Trading
                </button>
                <button id="stopBtn" class="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600 w-1/2" disabled>
                    Stop Trading
                </button>
            </div>
            
            <div id="status" class="mt-4 text-center"></div>
        </div>
        
        <div class="max-w-md mx-auto mt-6">
            <div id="trades" class="bg-white rounded-lg shadow-lg p-4">
                <h2 class="text-xl font-bold mb-4">Trade History</h2>
                <div id="tradeList" class="space-y-2"></div>
            </div>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/4.0.1/socket.io.js"></script>
    <script>
        const socket = io();
        const userId = 'user_' + Math.random().toString(36).substr(2, 9);
        
        document.getElementById('startBtn').addEventListener('click', async () => {
            const apiToken = document.getElementById('apiToken').value;
            const symbol = document.getElementById('symbol').value;
            const stake = document.getElementById('stake').value;
            
            const response = await fetch('/api/start', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ user_id: userId, api_token: apiToken, symbol, stake })
            });
            
            const data = await response.json();
            document.getElementById('status').textContent = data.message;
            document.getElementById('startBtn').disabled = true;
            document.getElementById('stopBtn').disabled = false;
        });
        
        document.getElementById('stopBtn').addEventListener('click', async () => {
            const response = await fetch('/api/stop', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ user_id: userId })
            });
            
            const data = await response.json();
            document.getElementById('status').textContent = data.message;
            document.getElementById('startBtn').disabled = false;
            document.getElementById('stopBtn').disabled = true;
        });
        
        socket.on('trade_update', (data) => {
            const tradeList = document.getElementById('tradeList');
            const tradeItem = document.createElement('div');
            tradeItem.className = 'p-2 border rounded';
            tradeItem.textContent = `${data.symbol}: ${data.type} @ ${data.price}`;
            tradeList.prepend(tradeItem);
        });
    </script>
</body>
</html>
