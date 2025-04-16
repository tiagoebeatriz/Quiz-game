<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Assista aos Anúncios</title>
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

        .ad-container {
            margin-bottom: 20px;
            background-color: white;
            border-radius: 10px;
            padding: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        iframe {
            border: none;
            width: 100%;
            height: 250px;
        }
    </style>
</head>
<body>

    <h1>Antes de Jogar</h1>
    <p>Assista aos 5 anúncios abaixo para liberar o acesso ao Quiz.</p>

    <div id="adsContainer"></div>

    <button id="btnComecar" disabled>Ir para o Quiz</button>

    <script>
        const adUrl = "https://www.profitableratecpm.com/a9jbed7jy4?key=c2a7916e8712767463a0c7e7fae86ec1";
        const totalAds = 5;
        let adsAssistidos = 0;

        const adsContainer = document.getElementById('adsContainer');
        const botao = document.getElementById('btnComecar');

        // Carregar os anúncios
        for (let i = 0; i < totalAds; i++) {
            const adBox = document.createElement('div');
            adBox.classList.add('ad-container');

            const iframe = document.createElement('iframe');
            iframe.src = adUrl;

            iframe.onload = () => {
                adsAssistidos++;
                // Atualiza o botão quando todos os anúncios foram assistidos
                if (adsAssistidos === totalAds) {
                    botao.disabled = false;
                    botao.innerText = "Ir para o Quiz";
                    botao.style.cursor = "pointer";
                }
            };

            adBox.appendChild(iframe);
            adsContainer.appendChild(adBox);
        }

        // Quando o botão é clicado, redireciona para o quiz
        botao.addEventListener('click', () => {
            if (adsAssistidos === totalAds) {
                window.location.href = "quiz.html"; // Altere para o caminho do seu quiz
            }
        });
    </script>

</body>
</html>
