<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Simples</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #ffcc00;
            color: #fff;
            text-align: center;
        }
        .question {
            font-size: 24px;
            margin: 20px;
        }
        .button {
            background-color: #00cc66;
            color: white;
            padding: 10px 20px;
            margin: 5px;
            border: none;
            cursor: pointer;
            font-size: 18px;
        }
        .button:hover {
            background-color: #00994d;
        }
        .result {
            font-size: 24px;
            margin-top: 20px;
        }
    </style>
</head>
<body>

<h1>Quiz de Perguntas Difíceis</h1>
<div id="quiz-container">
    <div class="question" id="question"></div>
    <div id="answers"></div>
    <button class="button" onclick="nextQuestion()">Próxima Pergunta</button>
</div>

<div class="result" id="result" style="display:none;"></div>

<script>
    const questions = [
        { question: "Qual é a capital da França?", answers: ["Paris", "Londres", "Roma", "Madrid"], correct: 0 },
        { question: "Qual é o maior oceano?", answers: ["Atlântico", "Índico", "Ártico", "Pacífico"], correct: 3 },
        { question: "Quem foi o primeiro homem a pisar na lua?", answers: ["Buzz Aldrin", "Neil Armstrong", "Yuri Gagarin", "Alan Shepard"], correct: 1 },
        { question: "Qual é o maior país do mundo?", answers: ["China", "EUA", "Brasil", "Rússia"], correct: 3 },
        { question: "Quantos continentes existem no mundo?", answers: ["5", "6", "7", "8"], correct: 2 },
    ];

    let currentQuestionIndex = 0;
    let correctAnswers = 0;

    function displayQuestion() {
        const question = questions[currentQuestionIndex];
        document.getElementById('question').textContent = question.question;

        let answersHTML = '';
        question.answers.forEach((answer, index) => {
            answersHTML += `<button class="button" onclick="checkAnswer(${index})">${answer}</button>`;
        });
        document.getElementById('answers').innerHTML = answersHTML;
    }

    function checkAnswer(selectedIndex) {
        const question = questions[currentQuestionIndex];
        if (selectedIndex === question.correct) {
            correctAnswers++;
        }
        currentQuestionIndex++;

        if (currentQuestionIndex < questions.length) {
            displayQuestion();
        } else {
            showResult();
        }
    }

    function showResult() {
        let resultMessage = `Você acertou ${correctAnswers} de ${questions.length} perguntas.`;
        if (correctAnswers === questions.length) {
            resultMessage += " Você ganhou 1,50 dólares!";
        }
        document.getElementById('quiz-container').style.display = 'none';
        document.getElementById('result').textContent = resultMessage;
        document.getElementById('result').style.display = 'block';
    }

    displayQuestion();
</script>

</body>
</html>
