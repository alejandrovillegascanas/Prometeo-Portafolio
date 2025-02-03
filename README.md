<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prometeo Portfolio</title>
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

        function updatePortfolio(prices) {
            const holdings = {
                solana: { quantity: 22.21, cost: 216.91 },
                bitcoin: { quantity: 0.035, cost: 105760.63 },
                "shadow-token": { quantity: 4268.38, cost: 0.48 },
                nosana: { quantity: 317.5, cost: 2.89 }
            };
            
            let totalValue = 0;
            let totalCost = 0;
            
            Object.keys(holdings).forEach(token => {
                const price = prices[token]?.usd || 0;
                const value = holdings[token].quantity * price;
                totalValue += value;
                totalCost += holdings[token].quantity * holdings[token].cost;
                document.getElementById(`${token}-price`).innerText = `$${price.toFixed(2)}`;
                document.getElementById(`${token}-value`).innerText = `$${value.toFixed(2)}`;
            });
            
            const result = totalValue - totalCost;
            const percentageChange = ((result / totalCost) * 100).toFixed(2);
            document.getElementById("total-value").innerText = `$${totalValue.toFixed(2)}`;
            document.getElementById("total-change").innerText = `${percentageChange}%`;
        }

        setInterval(fetchCryptoPrices, 60000);
        window.onload = fetchCryptoPrices;
    </script>
</head>
<body>
    <h1>Prometeo Portfolio</h1>
    <table border="1">
        <tr>
            <th>Asset</th>
            <th>Current Price (USD)</th>
            <th>Value (USD)</th>
        </tr>
        <tr>
            <td>Solana (SOL)</td>
            <td id="solana-price">Loading...</td>
            <td id="solana-value">Loading...</td>
        </tr>
        <tr>
            <td>Bitcoin (BTC)</td>
            <td id="bitcoin-price">Loading...</td>
            <td id="bitcoin-value">Loading...</td>
        </tr>
        <tr>
            <td>Shadow Token (SHDW)</td>
            <td id="shadow-token-price">Loading...</td>
            <td id="shadow-token-value">Loading...</td>
        </tr>
        <tr>
            <td>Nosana Token (NOS)</td>
            <td id="nosana-price">Loading...</td>
            <td id="nosana-value">Loading...</td>
        </tr>
    </table>
    <h2>Total Portfolio Value: <span id="total-value">Loading...</span></h2>
    <h2>Portfolio Change: <span id="total-change">Loading...</span></h2>
</body>
</html>
