<!DOCTYPE html>
<html lang="az">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MyHomeBaku - Kredit Kalkulyatoru</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f5f6fa;
            margin: 0;
            padding: 20px;
            color: #34495e;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
            background-color: #ffffff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            color: #34495e;
        }
        label {
            display: block;
            margin: 10px 0 5px;
            font-weight: bold;
        }
        input, select, button {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }
        button {
            background-color: #34495e;
            color: #ffffff;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #2c3e50;
        }
        .result {
            margin-top: 20px;
            padding: 10px;
            background-color: #f5f6fa;
            border-radius: 5px;
            display: none;
        }
        .radio-group {
            margin-bottom: 15px;
        }
        .radio-group label {
            display: inline-block;
            margin-right: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>MyHomeBaku - Kredit Kalkulyatoru</h1>
        <form id="creditForm">
            <label for="amount">Kredit məbləği (AZN):</label>
            <input type="number" id="amount" required min="0">

            <label for="term">Kredit müddəti (ay):</label>
            <select id="term" required>
                <option value="2">2 ay</option>
                <option value="3">3 ay</option>
                <option value="6">6 ay</option>
                <option value="12">12 ay</option>
                <option value="18">18 ay</option>
                <option value="24">24 ay</option>
                <option value="35">35 ay</option>
            </select>

            <label for="seller">Satıcı:</label>
            <select id="seller" required>
                <option value="Həsrət">Həsrət</option>
            </select>

            <label for="bank">Bank:</label>
            <select id="bank" required>
                <option value="Kapital">Kapital</option>
            </select>

            <div class="radio-group">
                <label>İş statusu:</label>
                <input type="radio" id="official" name="status" value="official" required>
                <label for="official">Rəsmi</label>
                <input type="radio" id="unofficial" name="status" value="unofficial">
                <label for="unofficial">Qeyri-rəsmi</label>
            </div>

            <button type="button" onclick="calculateCredit()">Hesabla</button>
            <button type="button" onclick="clearForm()">Təmizlə</button>
        </form>

        <div class="result" id="result">
            <h3>Nəticə</h3>
            <p>Aylıq ödəniş: <span id="monthlyPayment">0</span> AZN</p>
            <p>Ümumi ödəniş: <span id="totalPayment">0</span> AZN</p>
            <p>Faiz məbləği: <span id="interestAmount">0</span> AZN</p>
        </div>
    </div>

    <script>
        const interestRates = {
            official: {
                2: 3.1,
                3: 4.2,
                6: 7.4,
                12: 12.8,
                18: 18.8,
                24: 24.7,
                35: 31
            },
            unofficial: {
                2: 3.6,
                3: 4.8,
                6: 8.2,
                12: 14.7,
                18: 20.6,
                24: 26,
                35: 35.4
            }
        };

        function calculateCredit() {
            const amount = parseFloat(document.getElementById('amount').value);
            const term = parseInt(document.getElementById('term').value);
            const status = document.querySelector('input[name="status"]:checked').value;

            const interestRate = interestRates[status][term];
            const monthlyRate = interestRate / 100 / 12;
            const monthlyPayment = (amount * monthlyRate) / (1 - Math.pow(1 + monthlyRate, -term));
            const totalPayment = monthlyPayment * term;
            const interestAmount = totalPayment - amount;

            document.getElementById('monthlyPayment').textContent = monthlyPayment.toFixed(2);
            document.getElementById('totalPayment').textContent = totalPayment.toFixed(2);
            document.getElementById('interestAmount').textContent = interestAmount.toFixed(2);

            document.getElementById('result').style.display = 'block';
        }

        function clearForm() {
            document.getElementById('creditForm').reset();
            document.getElementById('result').style.display = 'none';
        }
    </script>
</body>
</html>
