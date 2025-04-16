<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Assista aos Anúncios</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #f3ec78, #af4261);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
      padding: 40px 20px;
      height: 100vh;
    }
    h1 {
      color: #fff;
      margin-bottom: 10px;
    }
    p {
      color: #fff;
      margin-bottom: 30px;
      font-size: 18px;
      text-align: center;
    }
    .ad-container {
      margin-bottom: 20px;
      background: white;
      padding: 10px;
      border-radius: 12px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      position: relative;
    }
    iframe {
      border: none;
      width: 300px;
      height: 250px;
      display: block;
    }
    .contador {
      position: absolute;
      top: 8px;
      right: 8px;
      background: #000;
      color: #fff;
      padding: 2px 8px;
      border-radius: 8px;
      font-size: 14px;
    }
    button {
      padding: 12px 24px;
      font-size: 18px;
      border: none;
      border-radius: 10px;
      background-color: #4CAF50;
      color: white;
      cursor: not-allowed;
      opacity: 0.6;
    }
    button.enabled {
      cursor: pointer;
      opacity: 1;
    }
  </style>
</head>
<body>

  <h1>Antes de Jogar</h1>
  <p>Assista aos 5 anúncios abaixo (5 segundos cada) para liberar o acesso ao Quiz Premiado.</p>

  <div id="ads"></div>

  <button id="btnComecar" disabled>Ir para o Quiz</button>

  <script>
    const adUrl = "https://www.profitableratecpm.com/a9jbed7jy4?key=c2a7916e8712767463a0c7e7fae86ec1";
    const totalAds = 5;
    const tempoPorAnuncio = 5; // segundos
    let adsAssistidos = 0;

    const container = document.getElementById("ads");
    const botao = document.getElementById("btnComecar");

    for (let i = 0; i < totalAds; i++) {
      const adBox = document.createElement("div");
      adBox.classList.add("ad-container");

      const iframe = document.createElement("iframe");
      iframe.src = adUrl;

      const contador = document.createElement("div");
      contador.classList.add("contador");
      contador.textContent = `${tempoPorAnuncio}s`;

      adBox.appendChild(iframe);
      adBox.appendChild(contador);
      container.appendChild(adBox);

      let tempoRestante = tempoPorAnuncio;
      const intervalo = setInterval(() => {
        tempoRestante--;
        contador.textContent = `${tempoRestante}s`;

        if (tempoRestante <= 0) {
          clearInterval(intervalo);
          contador.textContent = "✔️";
          adsAssistidos++;

          if (adsAssistidos === totalAds) {
            botao.disabled = false;
            botao.classList.add("enabled");
            botao.innerText = "Ir para o Quiz";
            botao.style.cursor = "pointer";
          }
        }
      }, 1000);
    }

    botao.addEventListener("click", () => {
      if (adsAssistidos === totalAds) {
        window.location.href = "quiz.html"; // Altere para o seu caminho real
      }
    });
  </script>

</body>
</html>
