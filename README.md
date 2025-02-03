<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prometeo Portafolio</title>
    <script>
        async function fetchCryptoPrices() {
            const apiUrl = "https://api.coingecko.com/api/v3/simple/price?ids=solana,bitcoin,shadow-token,nosana&vs_currencies=usd";
            try {
                const response = await fetch(apiUrl);
                const data = await response.json();
                updatePortfolio(data);
            } catch (error) {
                console.error("Error fetching prices:", error);
            }
        }

        function formatCurrency(value) {
            return `$${Math.round(value).toLocaleString()}`;
        }

        function updatePortfolio(prices) {
            const holdings = {
                solana: { quantity: 22.21, cost: 216.91 },
                bitcoin: { quantity: 0.035, cost: 105760.63 },
                "shadow-token": { quantity: 4268.38, cost: 0.48 },
                nosana: { quantity: 317.5, cost: 2.89 }
            };
            
            const investors = {
                "Candelaria Rodriguez": 0.30,
                "Nicolás Jaramillo": 0.30,
                "Camilo Medina": 0.10,
                "Alejandro Villegas": 0.30
            };
            
            let totalValue = 0;
            let totalCost = 0;
            
            Object.keys(holdings).forEach(token => {
                const price = prices[token]?.usd || 0;
                const value = holdings[token].quantity * price;
                const cost = holdings[token].quantity * holdings[token].cost;
                totalValue += value;
                totalCost += cost;
                document.getElementById(`${token}-price`).innerText = formatCurrency(price);
                document.getElementById(`${token}-value`).innerText = formatCurrency(value);
                document.getElementById(`${token}-cost`).innerText = formatCurrency(cost);
            });
            
            const totalReturn = totalValue - totalCost;
            const percentageReturn = Math.round((totalReturn / totalCost) * 100);
            document.getElementById("total-value").innerText = formatCurrency(totalValue);
            document.getElementById("total-cost").innerText = formatCurrency(totalCost);
            document.getElementById("total-return").innerText = formatCurrency(totalReturn);
            document.getElementById("total-return-percent").innerText = `${percentageReturn}%`;
            
            Object.keys(investors).forEach(investor => {
                const investorValue = totalValue * investors[investor];
                document.getElementById(`${investor.replace(/ /g, '-')}-value`).innerText = formatCurrency(investorValue);
            });
        }

        setInterval(fetchCryptoPrices, 60000);
        window.onload = fetchCryptoPrices;
    </script>
</head>
<body>
    <h1>Prometeo Portafolio</h1>
    <table border="1">
        <tr>
            <th>Asset</th>
            <th>Current Price (USD)</th>
            <th>Investment Cost (USD)</th>
            <th>Value (USD)</th>
        </tr>
        <tr>
            <td>Solana (SOL)</td>
            <td id="solana-price">Loading...</td>
            <td id="solana-cost">Loading...</td>
            <td id="solana-value">Loading...</td>
        </tr>
        <tr>
            <td>Bitcoin (BTC)</td>
            <td id="bitcoin-price">Loading...</td>
            <td id="bitcoin-cost">Loading...</td>
            <td id="bitcoin-value">Loading...</td>
        </tr>
        <tr>
            <td>Shadow Token (SHDW)</td>
            <td id="shadow-token-price">Loading...</td>
            <td id="shadow-token-cost">Loading...</td>
            <td id="shadow-token-value">Loading...</td>
        </tr>
        <tr>
            <td>Nosana Token (NOS)</td>
            <td id="nosana-price">Loading...</td>
            <td id="nosana-cost">Loading...</td>
            <td id="nosana-value">Loading...</td>
        </tr>
    </table>
    <h2>Total Portfolio Value: <span id="total-value">Loading...</span></h2>
    <h2>Total Portfolio Cost: <span id="total-cost">Loading...</span></h2>
    <h2>Total Return: <span id="total-return">Loading...</span></h2>
    <h2>Portfolio Return %: <span id="total-return-percent">Loading...</span></h2>
    <h2>Investor Portfolio Values:</h2>
    <ul>
        <li>Candelaria Rodriguez: <span id="Candelaria-Rodriguez-value">Loading...</span></li>
        <li>Nicolás Jaramillo: <span id="Nicolás-Jaramillo-value">Loading...</span></li>
        <li>Camilo Medina: <span id="Camilo-Medina-value">Loading...</span></li>
        <li>Alejandro Villegas: <span id="Alejandro-Villegas-value">Loading...</span></li>
    </ul>
</body>
</html>
