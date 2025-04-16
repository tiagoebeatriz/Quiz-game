<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quiz Premiado</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, #8EC5FC, #E0C3FC);
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      flex-direction: column;
    }

    .container {
      background: white;
      padding: 40px;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.15);
      max-width: 400px;
      width: 90%;
      text-align: center;
      animation: fadeIn 1s ease;
    }

    h1 {
      color: #333;
      font-size: 1.8em;
      margin-bottom: 10px;
    }

    p {
      color: #555;
      font-size: 1em;
      margin-bottom: 20px;
    }

    button {
      background-color: #4CAF50;
      color: white;
      font-size: 1em;
      padding: 12px 24px;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      transition: 0.3s;
      margin-top: 10px;
      width: 100%;
    }

    button:hover {
      background-color: #388E3C;
    }

    #saldo {
      font-weight: bold;
      margin-top: 20px;
      color: #333;
    }

    .share-btn {
      background-color: #2196F3;
      margin-top: 20px;
    }

    .ad-container {
      margin-top: 20px;
      padding: 10px;
      background-color: #f1f1f1;
      border-radius: 10px;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Desafio do Quiz</h1>
    <p>Responda corretamente e ganhe dinheiro real!</p>
    <div id="pergunta">Carregando pergunta...</div>
    <div id="respostas"></div>
    <div id="saldo">Saldo: $0.00</div>
    <button class="share-btn" onclick="shareGame()">Compartilhar com Amigos</button>
    
    <div class="ad-container">
      <h3>Assista ao anúncio abaixo para continuar jogando:</h3>
      <iframe src="https://www.profitableratecpm.com/a9jbed7jy4?key=c2a7916e8712767463a0c7e7fae86ec1" width="300" height="250" frameborder="0"></iframe>
    </div>
  </div>

  <script>
    // Perguntas muito difíceis sobre temas gerais
    const perguntas = [
      { pergunta: "Qual é a fórmula da equação de Schrödinger?", respostas: ["Ψ(x,t) = A e^(iωt)", "Ψ(x,t) = iħ∂/∂t", "Ψ(x,t) = -iħ∇²ψ(x,t)", "Ψ(x,t) = (∇²ψ)/ħ"], correta: 2 },
      { pergunta: "Qual é o nome do composto que contém um átomo de carbono central ligado a três grupos amina?", respostas: ["Aminoácido", "Amina terciária", "Amina primária", "Amina secundária"], correta: 1 },
      { pergunta: "Qual é o valor da constante de Hubble em unidades do Sistema Internacional?", respostas: ["70 km/s/Mpc", "73 km/s/Mpc", "68 km/s/Mpc", "75 km/s/Mpc"], correta: 2 },
      { pergunta: "Quem foi o primeiro imperador do Japão?", respostas: ["Imperador Jimmu", "Imperador Hirohito", "Imperador Taishō", "Imperador Meiji"], correta: 0 },
      { pergunta: "O que é o Teorema de Gödel sobre incompletude?", respostas: ["Todo sistema matemático é completo", "Nenhum sistema matemático é completo", "Todo sistema é inconsistente", "As equações de Gödel são infundadas"], correta: 1 },
      // Adicione mais 995 perguntas difíceis aqui
    ];

    let indiceAtual = 0;
    let saldo = 0;
    let vitorias = 0;

    // Função para embaralhar perguntas e respostas
    function embaralharArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    }

    function mostrarPergunta() {
      if (vitorias >= 10) {
        alert("Você atingiu o limite de 10 vitórias. Volte amanhã para jogar novamente.");
        return;
      }

      if (indiceAtual >= perguntas.length) {
        indiceAtual = 0;
      }

      // Embaralhar perguntas e respostas
      embaralharArray(perguntas);
      const p = perguntas[indiceAtual];
      
      // Embaralhar respostas
      embaralharArray(p.respostas);

      document.getElementById("pergunta").innerText = p.pergunta;

      const respostasDiv = document.getElementById("respostas");
      respostasDiv.innerHTML = "";
      p.respostas.forEach((r, i) => {
        const btn = document.createElement("button");
        btn.innerText = r;
        btn.onclick = () => verificarResposta(i === p.correta);
        respostasDiv.appendChild(btn);
      });
    }

    function verificarResposta(correta) {
      if (correta) {
        saldo += 1;
        vitorias++;
        alert("Você acertou todas as perguntas! Você ganhou $1.");
        
        // Após acertar, o jogador deve assistir os 5 vídeos novamente
        alert("Você terá que assistir os 5 vídeos novamente antes de continuar.");
        
        // Reiniciar o jogo e as perguntas
        indiceAtual = 0;
        mostrarPergunta();
      } else {
        alert("Resposta errada! Você perdeu tudo e vai recomeçar.");
        saldo = 0;
        indiceAtual = 0;
        mostrarPergunta();
      }
    }

    function shareGame() {
      if (navigator.share) {
        navigator.share({
          title: 'Desafio do Quiz',
          text: 'Aposte seus conhecimentos e ganhe dinheiro respondendo perguntas!',
          url: window.location.href
        }).catch(console.error);
      } else {
        prompt("Copie o link e compartilhe:", window.location.href);
      }
    }

    mostrarPergunta();
  </script>
</body>
</html>
