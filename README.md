<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Desafio Bíblico e de Relacionamento</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 20px;
            background-color: #f4f4f4;
        }
        .question {
            margin-bottom: 15px;
            padding: 10px;
            background: #fff;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        .result {
            display: none;
            margin-top: 10px;
            padding: 5px;
            border-radius: 3px;
        }
        .result.correct {
            color: green;
            background-color: #e6ffe6;
        }
        .result.incorrect {
            color: red;
            background-color: #ffe6e6;
        }
        .button {
            margin-top: 10px;
            padding: 10px 15px;
            background-color: #007bff;
            color: #fff;
            border: none;
            border-radius: 3px;
            cursor: pointer;
        }
        .button:hover {
            background-color: #0056b3;
        }
        .score {
            margin: 20px 0;
            padding: 10px;
            background-color: #e7f3ff;
            border: 1px solid #007bff;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <h1>Desafio Bíblico e de Relacionamento</h1>
    <p>Responda às perguntas abaixo. Se errar, a resposta correta aparecerá automaticamente!</p>

    <div class="score">
        <p><strong>Minha Garota:</strong> <span id="girl-score">0</span> pontos</p>
        <p><strong>Meu Garoto:</strong> <span id="boy-score">0</span> pontos</p>
    </div>

    <div id="questions-container"></div>

    <script>
        const biblicalQuestions = [
            { question: "Quem construiu a arca?", answer: "Noé" },
            { question: "Onde nasceu Jesus?", answer: "Belém" },
            { question: "Quem foi o homem mais forte da Bíblia?", answer: "Sansão" },
            { question: "Quem foi engolido por um grande peixe?", answer: "Jonas" },
            { question: "Quantos discípulos Jesus escolheu?", answer: "12" },
            { question: "Quem recebeu os Dez Mandamentos?", answer: "Moisés" },
            { question: "Qual foi o primeiro milagre de Jesus?", answer: "Transformar água em vinho" },
            { question: "Qual era o nome do gigante derrotado por Davi?", answer: "Golias" },
            { question: "Quem traiu Jesus?", answer: "Judas Iscariotes" },
            { question: "Qual é o primeiro livro da Bíblia?", answer: "Gênesis" },
            { question: "Quem foi lançado na cova dos leões?", answer: "Daniel" },
            { question: "Quem foi a mãe de Jesus?", answer: "Maria" },
            { question: "Quem foi o primeiro homem?", answer: "Adão" },
            { question: "Quem foi o primeiro rei de Israel?", answer: "Saul" },
            { question: "Quantos dias durou o dilúvio?", answer: "40 dias" },
            { question: "Quem escreveu o livro de Apocalipse?", answer: "João" },
            { question: "Quem liderou o povo de Israel na conquista de Canaã?", answer: "Josué" },
            { question: "Qual profeta foi levado ao céu em uma carruagem de fogo?", answer: "Elias" },
            { question: "Quem foi o pai de Isaque?", answer: "Abraão" },
            { question: "Qual apóstolo foi conhecido como o 'discípulo amado'?", answer: "João" }
        ];

        const relationshipQuestions = [
            { question: "Qual foi a primeira coisa que você notou em mim?" },
            { question: "Onde foi nosso primeiro encontro?" },
            { question: "Qual é a minha comida favorita?" },
            { question: "Qual é a viagem dos nossos sonhos?" },
            { question: "Qual presente mais especial que já te dei?" },
            { question: "Qual é a nossa música favorita?" },
            { question: "Qual foi o momento mais engraçado que tivemos juntos?" },
            { question: "Qual é o meu maior medo?" },
            { question: "O que eu faço que sempre te faz sorrir?" },
            { question: "Qual foi a primeira coisa que você pensou quando me viu?" },
            { question: "Qual é a minha cor favorita?" },
            { question: "Qual é o meu filme ou série favorito?" },
            { question: "Se eu pudesse comer uma coisa para o resto da vida, o que seria?" },
            { question: "Qual foi a nossa maior aventura juntos?" },
            { question: "Qual é o meu hobby favorito?" },
            { question: "Se eu pudesse visitar qualquer lugar do mundo, onde seria?" },
            { question: "O que mais gosto em você?" },
            { question: "Qual é a minha estação do ano favorita?" },
            { question: "Qual foi a viagem mais inesquecível que fizemos juntos?" },
            { question: "O que mais admiro em você?" },
            { question: "Qual é o meu maior sonho?" },
            { question: "Qual foi o momento mais romântico que compartilhamos?" },
            { question: "Se eu pudesse mudar algo em nossa casa, o que seria?" },
            { question: "Qual é a sobremesa que mais gosto?" },
            { question: "Qual é o meu talento oculto?" }
        ];

        let girlScore = 0;
        let boyScore = 0;
        let currentPlayer = 'girl'; // Alterna entre 'girl' e 'boy'

        function updateScoreBoard() {
            document.getElementById('girl-score').textContent = girlScore;
            document.getElementById('boy-score').textContent = boyScore;
        }

        function createQuestionElement(questionObj, index, category) {
            const questionDiv = document.createElement('div');
            questionDiv.className = 'question';

            const questionText = document.createElement('p');
            questionText.innerHTML = `<strong>${index + 1}. ${questionObj.question}</strong>`;
            questionDiv.appendChild(questionText);

            const input = document.createElement('input');
            input.type = 'text';
            input.id = `q${index}-${category}`;
            input.placeholder = 'Sua resposta';
            questionDiv.appendChild(input);

            const button = document.createElement('button');
            button.className = 'button';
            button.textContent = 'Enviar';

            if (category === 'biblical') {
                button.onclick = () => checkAnswer(input.id, questionObj.answer, category);
            } else {
                button.onclick = () => relationshipAnswer(input.id);
            }

            questionDiv.appendChild(button);

            const resultDiv = document.createElement('div');
            resultDiv.className = 'result';
            resultDiv.id = `${input.id}-result`;
            questionDiv.appendChild(resultDiv);

            return questionDiv;
        }

        function loadQuestions() {
            const container = document.getElementById('questions-container');

            let questionIndex = 0;
            biblicalQuestions.forEach((q) => {
                const questionElement = createQuestionElement(q, questionIndex, 'biblical');
                container.appendChild(questionElement);
                questionIndex++;
            });

            relationshipQuestions.forEach((q) => {
                const questionElement = createQuestionElement(q, questionIndex, 'relationship');
                container.appendChild(questionElement);
                questionIndex++;
            });
        }

        function checkAnswer(questionId, correctAnswer, category) {
            const userAnswer = document.getElementById(questionId).value.trim().toLowerCase();
            const resultDiv = document.getElementById(`${questionId}-result`);

            if (userAnswer === correctAnswer.toLowerCase()) {
                resultDiv.textContent = "Correto!";
                resultDiv.className = "result correct";
                if (currentPlayer === 'girl') {
                    girlScore += 10;
                } else {
                    boyScore += 10;
                }
            } else {
                resultDiv.textContent = `Incorreto. A resposta correta é: ${correctAnswer}`;
                resultDiv.className = "result incorrect";
            }
            resultDiv.style.display = "block";
            updateScoreBoard();
            togglePlayer();
        }

        function relationshipAnswer(questionId) {
            const resultDiv = document.getElementById(`${questionId}-result`);
            resultDiv.textContent = "Resposta enviada. Avaliem juntos se está correta!";
            resultDiv.className = "result correct";
            resultDiv.style.display = "block";
            togglePlayer();
        }

        function togglePlayer() {
            currentPlayer = currentPlayer === 'girl' ? 'boy' : 'girl';
            alert(`Agora é a vez de ${currentPlayer === 'girl' ? 'Minha Garota' : 'Meu Garoto'}!`);
        }

        loadQuestions();
    </script>
</body>
</html>
