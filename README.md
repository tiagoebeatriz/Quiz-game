<!-- ... mesma parte do HTML até o corpo ... -->
<body>
  <div class="container" id="bloqueioAnuncios">
    <h1>Desafio do Quiz</h1>
    <p>Antes de jogar, assista aos 5 anúncios obrigatórios!</p>
    
    <div class="ad-container" id="ads"></div>
    <p id="contadorAds">Anúncios assistidos: 0/5</p>
  </div>

  <div class="container" id="jogo" style="display: none;">
    <h1>Desafio do Quiz</h1>
    <p>Responda corretamente e ganhe dinheiro real!</p>
    <div id="pergunta">Carregando pergunta...</div>
    <div id="respostas"></div>
    <div id="saldo">Saldo: $0.00</div>
    <button class="share-btn" onclick="shareGame()">Compartilhar com Amigos</button>
  </div>

  <script>
    const perguntas = [
      { pergunta: "Qual é a fórmula da equação de Schrödinger?", respostas: ["Ψ(x,t) = A e^(iωt)", "Ψ(x,t) = iħ∂/∂t", "Ψ(x,t) = -iħ∇²ψ(x,t)", "Ψ(x,t) = (∇²ψ)/ħ"], correta: 2 },
      { pergunta: "Qual é o nome do composto que contém um átomo de carbono central ligado a três grupos amina?", respostas: ["Aminoácido", "Amina terciária", "Amina primária", "Amina secundária"], correta: 1 },
      // Adicione mais perguntas aqui...
    ];

    let indiceAtual = 0;
    let saldo = 0;
    let vitorias = 0;
    let anunciosAssistidos = 0;

    function embaralharArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    }

    function mostrarPergunta() {
      if (vitorias >= 10) {
        alert("Você atingiu o limite de 10 vitórias. Volte amanhã.");
        return;
      }

      embaralharArray(perguntas);
      const p = perguntas[indiceAtual];

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
        document.getElementById("saldo").innerText = "Saldo: $" + saldo.toFixed(2);
        alert("Parabéns! Você ganhou $1.");
        if (vitorias < 10) {
          alert("Agora você precisa assistir 5 anúncios novamente.");
          iniciarAnuncios();
        }
      } else {
        alert("Errou! Você perdeu tudo.");
        saldo = 0;
        document.getElementById("saldo").innerText = "Saldo: $0.00";
        iniciarAnuncios();
      }
    }

    function iniciarAnuncios() {
      document.getElementById("jogo").style.display = "none";
      document.getElementById("bloqueioAnuncios").style.display = "block";
      anunciosAssistidos = 0;
      document.getElementById("contadorAds").innerText = "Anúncios assistidos: 0/5";
      const adsDiv = document.getElementById("ads");
      adsDiv.innerHTML = "";

      for (let i = 1; i <= 5; i++) {
        const ad = document.createElement("iframe");
        ad.src = "https://www.profitableratecpm.com/a9jbed7jy4?key=c2a7916e8712767463a0c7e7fae86ec1";
        ad.width = 300;
        ad.height = 250;
        ad.style.marginBottom = "10px";
        ad.onload = () => {
          anunciosAssistidos++;
          document.getElementById("contadorAds").innerText = `Anúncios assistidos: ${anunciosAssistidos}/5`;
          if (anunciosAssistidos >= 5) {
            document.getElementById("bloqueioAnuncios").style.display = "none";
            document.getElementById("jogo").style.display = "block";
            indiceAtual = 0;
            mostrarPergunta();
          }
        };
        adsDiv.appendChild(ad);
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

    // Inicia o jogo com anúncios obrigatórios
    iniciarAnuncios();
  </script>
</body>
