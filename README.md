<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Quiz dos 5 Anúncios</title>
</head>
<body>
  <h1>Quiz Premiado</h1>
  <button onclick="watchAd()">Assistir Anúncio (<span id="adsCount">0</span>/5)</button>
  <button id="startBtn" onclick="startGame()" disabled>Começar Quiz</button>
  <button onclick="withdraw()">Solicitar Saque</button>
  <p id="saldo">Saldo: $0</p>

  <script>
    // Variáveis
    let currentQuestion = 0;
    let score = 0;
    let dailyWins = 0;
    let balance = 0;
    const maxDailyWins = 10;
    let adsWatched = 0;
    const totalAdsRequired = 5;

    // Datas
    const today = new Date().toISOString().split('T')[0];
    let lastPlayDate = localStorage.getItem('lastPlayDate');
    if (lastPlayDate !== today) {
      dailyWins = 0;
      localStorage.setItem('lastPlayDate', today);
    }

    let lastWithdrawalDate = localStorage.getItem('lastWithdrawalDate') || null;

    // Evita sair da aba
    window.onblur = () => {
      alert("Você saiu da página! Reiniciando...");
      location.reload();
    };

    function watchAd() {
      adsWatched++;
      document.getElementById('adsCount').textContent = adsWatched;
      if (adsWatched >= totalAdsRequired) {
        document.getElementById('startBtn').disabled = false;
        alert("Você pode começar o quiz!");
      } else {
        alert(`Assista ao anúncio ${adsWatched}/${totalAdsRequired}`);
      }
    }

    function startGame() {
      if (adsWatched < totalAdsRequired) {
        alert("Assista aos 5 anúncios primeiro!");
        return;
      }

      if (dailyWins >= maxDailyWins) {
        alert("Você já ganhou 10 vezes hoje. Volte amanhã!");
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
        alert("Quiz cancelado.");
        return;
      }

      if (answer.trim().toLowerCase() === current.a.toLowerCase()) {
        currentQuestion++;
        if (currentQuestion === questions.length) {
          score++;
          dailyWins++;
          balance++;
          alert("Você ganhou $1.00! Saldo atual: $" + balance);
          adsWatched = 0;
          document.getElementById('adsCount').textContent = "0";
          document.getElementById('startBtn').disabled = true;
        } else {
          showQuestion();
        }
      } else {
        alert("Você errou! Voltando ao início. Assista os anúncios novamente.");
        adsWatched = 0;
        score = 0;
        currentQuestion = 0;
        document.getElementById('adsCount').textContent = "0";
        document.getElementById('startBtn').disabled = true;
      }
    }

    function withdraw() {
      const now = new Date();
      const currentDay = now.getDay(); // 3 = quarta-feira
      const today = now.toISOString().split('T')[0];

      if (currentDay !== 3) {
        alert("Saque disponível só às quartas-feiras!");
        return;
      }

      if (lastWithdrawalDate === today) {
        alert("Você já sacou esta semana.");
        return;
      }

      if (balance < 50) {
        alert("Você precisa de pelo menos $50 para sacar.");
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

    // Atualiza saldo em tempo real
    setInterval(() => {
      document.getElementById("saldo").innerText = `Saldo: $${balance}`;
    }, 1000);
  </script>
</body>
</html>
