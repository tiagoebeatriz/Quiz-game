<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>⚡ Liberar o Quiz - Assista aos Anúncios!</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: 'Arial', sans-serif;
            background: linear-gradient(to bottom right, #0f2027, #203a43, #2c5364);
            color: #fff;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            min-height: 100vh;
            padding-top: 40px;
        }

        h1 {
            font-size: 2.2em;
            color: #00ffcc;
            text-shadow: 1px 1px 5px #000;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .progress-text {
            font-size: 1.2em;
            margin-top: 10px;
            margin-bottom: 20px;
        }

        .progress-bar {
            width: 90%;
            height: 25px;
            background: #444;
            border-radius: 20px;
            overflow: hidden;
            margin-bottom: 30px;
            box-shadow: 0 0 10px #00ffcc;
        }

        .progress-bar-fill {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, #00ffcc, #00ff99);
            transition: width 0.4s ease-in-out;
        }

        .ad-container {
            margin: 10px 0;
            background: #fff;
            border-radius: 10px;
            overflow: hidden;
            width: 95%;
            max-width: 420px;
            height: 260px;
            box-shadow: 0 0 15px #00ffcc;
        }

        iframe {
            width: 100%;
            height: 100%;
            border: none;
        }

        button {
            margin-top: 20px;
            padding: 15px 30px;
            background: #00ffcc;
            color: #000;
            font-size: 1.2em;
            font-weight: bold;
            border: none;
            border-radius: 30px;
            cursor: not-allowed;
            transition: background 0.3s, transform 0.2s;
            box-shadow: 0 0 10px #00ffcc;
        }

        button.enabled {
            cursor: pointer;
            background: #00ff99;
        }

        button.enabled:hover {
            transform: scale(1.05);
        }

        audio {
            display: none;
        }
    </style>
</head>
<body>

    <h1>🎁 Assista aos 5 Anúncios e Ganhe!</h1>
    <p class="progress-text" id="progressoTexto">0 de 5 anúncios assistidos</p>
    <div class="progress-bar">
        <div class="progress-bar-fill" id="barra"></div>
    </div>

    <div id="adsContainer"></div>

    <button id="btnComecar">Liberar o Quiz</button>

    <audio id="somAnuncio" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_933b6a4de8.mp3?filename=game-notification-123672.mp3"></audio>

    <script>
        const adUrl = "https://www.profitableratecpm.com/a9jbed7jy4?key=c2a7916e8712767463a0c7e7fae86ec1";
        const totalAds = 5;
        let adsAssistidos = 0;

        const adsContainer = document.getElementById('adsContainer');
        const barra = document.getElementById('barra');
        const progressoTexto = document.getElementById('progressoTexto');
        const botao = document.getElementById('btnComecar');
        const som = document.getElementById('somAnuncio');

        // Criação dos anúncios com iframes
        for (let i = 0; i < totalAds; i++) {
            const adBox = document.createElement('div');
            adBox.classList.add('ad-container');

            const iframe = document.createElement('iframe');
            iframe.src = adUrl;

            iframe.onload = () => {
                adsAssistidos++;
                som.play(); // Toca o som de notificação
                progressoTexto.innerText = `${adsAssistidos} de ${totalAds} anúncios assistidos`;
                barra.style.width = `${(adsAssistidos / totalAds) * 100}%`;

                if (adsAssistidos === totalAds) {
                    botao.disabled = false;
                    botao.classList.add('enabled');
                    botao.innerText = "✅ Começar Quiz Agora!";
                }
            };

            adBox.appendChild(iframe);
            adsContainer.appendChild(adBox);
        }

        // Ação do botão
        botao.addEventListener('click', () => {
            if (adsAssistidos === totalAds) {
                window.location.href = "quiz.html"; // Altere para o link real do seu quiz
            }
        });
    </script>

</body>
</html>
