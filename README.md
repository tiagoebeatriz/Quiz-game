<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo de Quiz</title>

    <!-- Adicionando o script de anúncio -->
    <script type='text/javascript' src='//pl26391059.profitableratecpm.com/dc/a9/72/dca972cc07507145ad5241b658496cc3.js'></script>

    <!-- Estilos CSS -->
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f0f4f7;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
        }

        h1 {
            color: #333;
            font-size: 2em;
            margin-bottom: 20px;
        }

        p {
            color: #666;
            font-size: 1.1em;
            margin-bottom: 20px;
        }

        button {
            background-color: #4CAF50;
            color: white;
            font-size: 1.1em;
            padding: 15px 32px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s;
            margin: 10px 0;
            width: 200px;
        }

        button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }

        button:hover:not(:disabled) {
            background-color: #45a049;
        }

        #saldo {
            font-size: 1.2em;
            font-weight: bold;
            color: #333;
            margin-top: 20px;
        }

        .container {
            background-color: white;
            border-radius: 10px;
            padding: 40px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            text-align: center;
        }

        .container button {
            width: 100%;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Bem-vindo ao Quiz!</h1>
        <p>Assista aos anúncios para jogar!</p>

        <!-- Botões e interação -->
        <button onclick="watchAd()">Assistir Anúncio (0/5)</button>
        <button id="startBtn" onclick="startGame()" disabled>Começar Quiz</button>
        <button onclick="withdraw()">Solicitar Saque</button>

        <p id="saldo">Saldo: $0</p>
    </div>

    <script>
        // Configurações iniciais
        let currentQuestion = 0;
        let score = 0;
        let dailyWins = 0;
        let balance = 0;
        const maxDailyWins = 10;
        let adsWatched = 0;
        const totalAdsRequired = 5;

        // Controle de datas e saque
        const today = new Date().toISOString().split('T')[0];
        let lastPlayDate = localStorage.getItem('lastPlayDate');
        if (lastPlayDate !== today) {
            dailyWins = 0;
            localStorage.setItem('lastPlayDate', today);
        }

        let lastWithdrawalDate = localStorage.getItem('lastWithdrawalDate') || null;

        // Previne sair da página (simulado)
        window.onblur = () => {
            alert("Você saiu da página! Reiniciando jogo...");
            location.reload();
        };

        function watchAd() {
            adsWatched++;
            document.querySelector('button').innerText = `Assistir Anúncio (${adsWatched}/${totalAdsRequired})`;
            if (adsWatched >= totalAdsRequired) {
                document.getElementById('startBtn').disabled = false;
                alert("Anúncios completos! Agora você pode jogar.");
            } else {
                alert(`Assista ao anúncio ${adsWatched}/${totalAdsRequired}`);
            }
        }

        function startGame() {
            if (adsWatched < totalAdsRequired) {
                alert("Você precisa assistir aos 5 anúncios antes de jogar.");
                return;
            }

            if (dailyWins >= maxDailyWins) {
                alert("Você já atingiu o limite de 10 vitórias hoje. Volte amanhã!");
                return;
            }

            currentQuestion = 0;
            score = 0;
            showQuestion();
        }

        function showQuestion() {
            const questions = [
                { q: "Qual é o número primo entre 89 e 91?", a: "89" },
                { q: "Qual é o nome do décimo planeta anão?", a: "Eris" },
                { q: "Quantos elétrons tem o urânio?", a: "92" },
                { q: "Quem formulou o paradoxo do gato?", a: "Schrödinger" },
                { q: "Qual a raiz quadrada de 9801?", a: "99" },
            ];

            const current = questions[currentQuestion];
            const answer = prompt(current.q);

            if (answer === null) {
                alert("Jogo cancelado.");
                return;
            }

            if (answer.trim().toLowerCase() === current.a.toLowerCase()) {
                currentQuestion++;
                if (currentQuestion === questions.length) {
                    score++;
                    dailyWins++;
                    balance++;
                    alert("Parabéns! Você ganhou $1.00. Seu saldo: $" + balance);
                    adsWatched = 0;
                } else {
                    showQuestion();
                }
            } else {
                alert("Errou! Voltando para o início. Você deverá assistir os anúncios novamente.");
                adsWatched = 0;
                score = 0;
                currentQuestion = 0;
            }
        }

        function withdraw() {
            const now = new Date();
            const currentDay = now.getDay(); // 3 = quarta-feira
            const today = now.toISOString().split('T')[0];

            if (currentDay !== 3) {
                alert("Saques só podem ser feitos às quartas-feiras.");
                return;
            }

            if (lastWithdrawalDate === today) {
                alert("Você já fez um saque esta semana.");
                return;
            }

            if (balance < 50) {
                alert("Saldo insuficiente. É necessário pelo menos $50 para sacar.");
                return;
            }

            const method = prompt("Digite o método de saque (pix ou paypal):");
            if (method !== "pix" && method !== "paypal") {
                alert("Método inválido.");
                return;
            }

            const key = prompt("Digite a chave PIX ou e-mail do PayPal:");
            if (!key) {
                alert("Chave inválida.");
                return;
            }

            alert(`Saque de $50 solicitado via ${method}. Entraremos em contato se necessário.`);
            balance -= 50;
            lastWithdrawalDate = today;
            localStorage.setItem('lastWithdrawalDate', today);
        }

        // Atualiza o saldo a cada segundo
        setInterval(() => {
            document.getElementById("saldo").innerText = `Saldo: $${balance}`;
        }, 1000);
    </script>
</body>
</html>
