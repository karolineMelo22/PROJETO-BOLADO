<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Quiz Gamer - TechBrain</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');
        * {
            box-sizing: border-box;
        }
        
        body,
        html {
            margin: 0;
            padding: 0;
            height: 100%;
            font-family: 'Press Start 2P', cursive;
            background: url('https://images.unsplash.com/photo-1511512578047-dfb367046420?auto=format&fit=crop&w=1470&q=80') no-repeat center center fixed;
            background-size: cover;
            color: #00ff00;
            text-shadow: 0 0 5px #0f0;
        }
        
        .overlay {
            background: rgba(0, 0, 0, 0.85);
            position: fixed;
            inset: 0;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 20px;
            animation: fadeIn 1s ease forwards;
        }
        
        @keyframes fadeIn {
            from {
                opacity: 0;
            }
            to {
                opacity: 1;
            }
        }
        
        .hidden {
            display: none;
        }
        
        h1,
        h2 {
            text-align: center;
        }
        
        input,
        select,
        button {
            font-family: inherit;
            margin-bottom: 20px;
        }
        
        input[type=text],
        select {
            padding: 10px;
            width: 280px;
            font-size: 1rem;
            border: 2px solid #00ff00;
            background: transparent;
            color: #00ff00;
            outline: none;
            text-align: center;
        }
        
        button {
            padding: 12px 25px;
            border: 2px solid #00ff00;
            background: transparent;
            color: #00ff00;
            cursor: pointer;
            transition: .3s;
            border-radius: 5px;
        }
        
        button:hover {
            background: #00ff00;
            color: #000;
        }
        
        #quiz-container {
            max-width: 700px;
            width: 100%;
            background: rgba(0, 0, 0, 0.75);
            padding: 20px;
            border-radius: 8px;
            animation: slideIn 0.7s ease forwards;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        @keyframes slideIn {
            from {
                transform: translateY(50px);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }
        
        .question,
        #question {
            font-size: 1.25rem;
            margin-bottom: 15px;
            text-align: center;
            min-height: 60px;
        }
        
        .options,
        #options {
            display: flex;
            flex-direction: column;
            width: 100%;
            max-width: 500px;
        }
        
        .option-btn {
            margin: 8px 0;
            padding: 10px;
            font-size: 1rem;
            background: transparent;
            border: 2px solid #00ff00;
            color: #00ff00;
            cursor: pointer;
            border-radius: 5px;
            transition: .3s;
            text-align: center;
        }
        
        .option-btn:hover {
            background: #00ff00;
            color: #000;
        }
        
        .option-btn.correct {
            background: #0f0;
            color: #000;
            border-color: #0f0;
            box-shadow: 0 0 10px #0f0;
        }
        
        .option-btn.wrong {
            background: #f00;
            color: #fff;
            border-color: #f00;
        }
        
        #scoreboard,
        #lives,
        #timer-container {
            margin-top: 20px;
            font-size: 1.1rem;
            width: 100%;
            max-width: 500px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        #lives span {
            font-size: 1.5rem;
            margin-right: 5px;
        }
        
        #timer-container {
            flex-direction: column;
            align-items: stretch;
        }
        
        #timer-bar {
            height: 12px;
            background: #0f0;
            width: 100%;
            border-radius: 6px;
            margin-top: 5px;
            transition: width 1s linear, background 1s linear;
        }
        
        #next-btn {
            margin-top: 20px;
            padding: 12px 30px;
            font-size: 1rem;
            border: 2px solid #00ff00;
            background: transparent;
            color: #00ff00;
            cursor: pointer;
            border-radius: 5px;
            transition: 0.3s;
            align-self: center;
            min-width: 120px;
        }
        
        #next-btn:hover:not(:disabled) {
            background: #00ff00;
            color: #000;
        }
        
        #next-btn:disabled {
            opacity: 0.5;
            cursor: default;
        }
        
        #quit-btn {
            margin-top: 10px;
            padding: 12px 25px;
            font-size: 1rem;
            border: 2px solid #f00;
            background: transparent;
            color: #f00;
            cursor: pointer;
            border-radius: 5px;
            transition: .3s;
            align-self: center;
            min-width: 120px;
        }
        
        #quit-btn:hover {
            background: #f00;
            color: #000;
        }
        
        #ranking-back-btn {
            padding: 12px 25px;
            border: 2px solid #00ff00;
            background: transparent;
            color: #00ff00;
            cursor: pointer;
            border-radius: 5px;
            transition: .3s;
        }
        
        #ranking-back-btn:hover {
            background: #00ff00;
            color: #000;
        }
        
        #ranking-fixed-btn {
            position: fixed;
            top: 15px;
            right: 15px;
            background: transparent;
            border: 2px solid #00ff00;
            color: #00ff00;
            padding: 10px 15px;
            border-radius: 5px;
            font-family: 'Press Start 2P', cursive;
            font-size: 0.75rem;
            cursor: pointer;
            z-index: 9999;
            transition: background 0.3s, color 0.3s;
        }
        
        #ranking-fixed-btn:hover {
            background: #00ff00;
            color: #000;
        }
    </style>
</head>

<body>
    <!-- Botão fixo de Ranking -->
    <button id="ranking-fixed-btn" title="Ver Ranking">🏆 Ranking</button>



    <div class="overlay" id="start-screen">
        <h1>Quiz Gamer - TechBrain</h1>
        <input type="text" id="player-name" placeholder="Digite seu nome..." maxlength="15" />
        <select id="level-select">
            <option value="Fácil">Fácil</option>
            <option value="Médio">Médio</option>
            <option value="Difícil">Difícil</option>
        </select>
        <select id="category-select">
            <option value="Games">Games</option>
            <option value="Programação">Programação</option>
            <option value="Engenharia Social">Engenharia Social</option>
        </select>
        <button id="start-btn">Começar Quiz</button>
        <button id="how-to-play-btn">Como Jogar</button>
    </div>

    <div class="overlay hidden" id="how-to-play-modal">
        <div id="quiz-container">
            <h2>🎮 COMO JOGAR</h2>
            <p>1. Escolha seu nome, categoria e dificuldade.</p>
            <p>2. Você tem 3 vidas para errar perguntas.</p>
            <p>3. Responda antes do tempo acabar (15 segundos por pergunta).</p>
            <p>4. Clique na alternativa correta.</p>
            <p>5. Ganhe pontos e tente sobreviver até o final!</p>
            <button id="close-how-to-btn">Fechar</button>
        </div>
    </div>

    <div class="overlay hidden" id="quiz-screen">
        <div id="quiz-container">
            <div id="question" class="question"></div>
            <div id="options" class="options"></div>
            <div id="scoreboard">Pontuação: 0</div>
            <div id="lives">Vidas: <span id="life1">❤</span><span id="life2">❤</span><span id="life3">❤</span></div>
            <div id="timer-container">
                Tempo restante:
                <div id="timer-bar"></div>
            </div>
            <button id="next-btn" disabled>Próxima</button>
            <button id="quit-btn">Voltar para Início</button>
        </div>
    </div>

    <div class="overlay hidden" id="ranking-screen">
        <div id="quiz-container">
            <h2>🏆 Ranking - TechBrain</h2>
            <ol id="ranking-list" style="text-align:center; margin-bottom:20px; color:#0f0;"></ol>
            <button id="ranking-back-btn">Voltar</button>
            <button id="clear-ranking-btn">Limpar Ranking</button>
        </div>
    </div>


    <audio id="bg-music" loop src="techbrain.mp3"></audio>
    <audio id="sound-correct" src="correto.mp3"></audio>
    <audio id="sound-wrong" src="gameover.mp3"></audio>
    <audio id="sound-click" src="clik.mp3"></audio>

    <script>
        const startBtn = document.getElementById("start-btn");
        const howToPlayBtn = document.getElementById("how-to-play-btn");
        const closeHowToBtn = document.getElementById("close-how-to-btn");
        const quizScreen = document.getElementById("quiz-screen");
        const startScreen = document.getElementById("start-screen");
        const howToPlayModal = document.getElementById("how-to-play-modal");
        const rankingScreen = document.getElementById("ranking-screen");
        const rankingList = document.getElementById("ranking-list");
        const rankingBackBtn = document.getElementById("ranking-back-btn");
        const rankingFixedBtn = document.getElementById("ranking-fixed-btn");
        const quitBtn = document.getElementById("quit-btn");
        const bgMusic = document.getElementById("bg-music");
        const soundCorrect = document.getElementById("sound-correct");
        const soundWrong = document.getElementById("sound-wrong");
        const soundClick = document.getElementById("sound-click");
        const nextBtn = document.getElementById("next-btn");
        const questionEl = document.getElementById("question");
        const optionsEl = document.getElementById("options");
        const scoreboard = document.getElementById("scoreboard");
        const timerBar = document.getElementById("timer-bar");
        const livesEl = document.getElementById("lives");

        let playerName = "";
        let score = 0;
        let currentQuestionIndex = 0;
        let lives = 3;
        let timer;
        let timeLeft = 15;
        let selectedLevel = "easy";
        let selectedCategory = "games";
        let filteredQuestions = []; // 🌟 Adicione isso


        const questions = [
            // 🌟 GAMES - FÁCIL
            {
                level: "Fácil",
                category: "Games",
                question: "Qual o nome do criador do jogo Minecraft?",
                options: ["Markus Persson", "Notch", "Steve", "Alex"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Games",
                question: "Qual o mascote oficial da Nintendo?",
                options: ["Sonic", "Link", "Mario", "Pikachu"],
                correct: 2
            }, {
                level: "Fácil",
                category: "Games",
                question: "Qual console foi fabricado pela Sony?",
                options: ["Xbox", "PlayStation", "Switch", "Dreamcast"],
                correct: 1
            }, {
                level: "Fácil",
                category: "Games",
                question: "Qual gênero é o jogo FIFA?",
                options: ["Corrida", "Esporte", "Ação", "RPG"],
                correct: 1
            }, {
                level: "Fácil",
                category: "Games",
                question: "Sonic é mascote de qual empresa?",
                options: ["Nintendo", "Sega", "Sony", "Microsoft"],
                correct: 1
            }, {
                level: "Fácil",
                category: "Games",
                question: "Qual desses é um jogo de batalha real?",
                options: ["Fortnite", "Minecraft", "The Sims", "Tetris"],
                correct: 0
            },

            // 🌟 GAMES - MÉDIO
            {
                level: "Médio",
                category: "Games",
                question: "Qual dessas franquias é da Rockstar Games?",
                options: ["FIFA", "GTA", "Call of Duty", "Assassin's Creed"],
                correct: 1
            }, {
                level: "Médio",
                category: "Games",
                question: "Em qual console foi lançado o primeiro jogo da série Zelda?",
                options: ["SNES", "NES", "N64", "Switch"],
                correct: 1
            }, {
                level: "Médio",
                category: "Games",
                question: "Em que ano foi lançado o primeiro PlayStation?",
                options: ["1994", "1998", "2000", "1990"],
                correct: 0
            }, {
                level: "Médio",
                category: "Games",
                question: "Qual empresa desenvolveu o jogo Overwatch?",
                options: ["Blizzard", "Ubisoft", "Valve", "Epic Games"],
                correct: 0
            }, {
                level: "Médio",
                category: "Games",
                question: "Em qual jogo você encontra o personagem Master Chief?",
                options: ["Halo", "Gears of War", "Destiny", "Doom"],
                correct: 0
            }, {
                level: "Médio",
                category: "Games",
                question: "Quem é o criador do personagem Mario?",
                options: ["Hideo Kojima", "Shigeru Miyamoto", "Gabe Newell", "Phil Spencer"],
                correct: 1
            },

            // 🌟 GAMES - DIFÍCIL
            {
                level: "Difícil",
                category: "Games",
                question: "Qual linguagem é usada no desenvolvimento do Unity?",
                options: ["Java", "C#", "Python", "C++"],
                correct: 1
            }, {
                level: "Difícil",
                category: "Games",
                question: "Qual jogo popular foi criado por Markus Persson?",
                options: ["Minecraft", "Terraria", "Roblox", "Fortnite"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Games",
                question: "Qual estúdio desenvolveu The Witcher 3?",
                options: ["CD Projekt Red", "Bethesda", "BioWare", "Rockstar"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Games",
                question: "Em que ano foi lançado o primeiro GTA?",
                options: ["1997", "2001", "1999", "1995"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Games",
                question: "Qual engine é usada em Fortnite?",
                options: ["CryEngine", "Unreal Engine", "Unity", "Frostbite"],
                correct: 1
            }, {
                level: "Difícil",
                category: "Games",
                question: "Dark Souls foi criado por qual diretor?",
                options: ["Hidetaka Miyazaki", "Shigeru Miyamoto", "Gabe Newell", "Todd Howard"],
                correct: 0
            },

            // 💻 PROGRAMAÇÃO - FÁCIL
            {
                level: "Fácil",
                category: "Programação",
                question: "Qual dessas é uma linguagem de programação?",
                options: ["HTML", "CSS", "JavaScript", "Photoshop"],
                correct: 2
            }, {
                level: "Fácil",
                category: "Programação",
                question: "O que significa 'bug' na programação?",
                options: ["Inseto", "Erro", "Código", "Função"],
                correct: 1
            }, {
                level: "Fácil",
                category: "Programação",
                question: "Qual linguagem é usada para criar páginas da web?",
                options: ["HTML", "Python", "C#", "Java"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Programação",
                question: "CSS é usado para:",
                options: ["Lógica", "Estilo", "Banco de dados", "Rede"],
                correct: 1
            }, {
                level: "Fácil",
                category: "Programação",
                question: "Qual dessas é uma linguagem front-end?",
                options: ["JavaScript", "MySQL", "PHP", "Python"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Programação",
                question: "Python é conhecido por ser:",
                options: ["Difícil de aprender", "Fácil de ler", "Somente usado em web", "Somente para jogos"],
                correct: 1
            },

            // 💻 PROGRAMAÇÃO - MÉDIO
            {
                level: "Médio",
                category: "Programação",
                question: "Qual método adiciona um elemento ao final de um array em JS?",
                options: [".push()", ".pop()", ".shift()", ".unshift()"],
                correct: 0
            }, {
                level: "Médio",
                category: "Programação",
                question: "Qual palavra-chave cria uma constante em JavaScript?",
                options: ["var", "let", "const", "constant"],
                correct: 2
            }, {
                level: "Médio",
                category: "Programação",
                question: "Qual desses é um framework de JS?",
                options: ["React", "Django", "Laravel", "Flask"],
                correct: 0
            }, {
                level: "Médio",
                category: "Programação",
                question: "PHP é usado principalmente para:",
                options: ["Banco de dados", "Programação web", "Redes", "Criptografia"],
                correct: 1
            }, {
                level: "Médio",
                category: "Programação",
                question: "SQL é usado para:",
                options: ["Estilo", "Banco de dados", "Animações", "Frontend"],
                correct: 1
            }, {
                level: "Médio",
                category: "Programação",
                question: "Git serve para:",
                options: ["Controle de versão", "Estilo CSS", "Criar sites", "Desenhar gráficos"],
                correct: 0
            },

            // 💻 PROGRAMAÇÃO - DIFÍCIL
            {
                level: "Difícil",
                category: "Programação",
                question: "O que significa DOM em JavaScript?",
                options: ["Document Object Model", "Data Object Model", "Document Online Mode", "Data Online Model"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Programação",
                question: "CSS serve para:",
                options: ["Programar lógica", "Estilizar páginas", "Banco de dados", "Criptografar dados"],
                correct: 1
            }, {
                level: "Difícil",
                category: "Programação",
                question: "O que é um closure em JavaScript?",
                options: ["Função com escopo preservado", "Classe", "Array", "Objeto JSON"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Programação",
                question: "O que é recursão?",
                options: ["Função que chama a si mesma", "Laço infinito", "Classe abstrata", "Função anônima"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Programação",
                question: "Em Python, o que faz o operador // ?",
                options: ["Divisão inteira", "Módulo", "Exponenciação", "Divisão comum"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Programação",
                question: "TypeScript é uma variação de qual linguagem?",
                options: ["Python", "JavaScript", "C#", "PHP"],
                correct: 1
            },

            // 🕵 ENGENHARIA SOCIAL - FÁCIL
            {
                level: "Fácil",
                category: "Engenharia Social",
                question: "Engenharia social visa explorar:",
                options: ["Falhas humanas", "Erros de software", "Erros de hardware", "Redes Wi-Fi"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Engenharia Social",
                question: "O que é phishing?",
                options: ["Antivírus", "Firewall", "Roubo de dados", "Proteção de rede"],
                correct: 2
            }, {
                level: "Fácil",
                category: "Engenharia Social",
                question: "O que significa senha fraca?",
                options: ["Fácil de adivinhar", "Forte proteção", "Senha criptografada", "Senha em 2FA"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Engenharia Social",
                question: "Qual ação protege contra phishing?",
                options: ["Clicar em links", "Verificar remetente", "Compartilhar senhas", "Usar Wi-Fi público"],
                correct: 1
            }, {
                level: "Fácil",
                category: "Engenharia Social",
                question: "O que é engenharia social?",
                options: ["Manipulação psicológica", "Firewall avançado", "Antivírus", "Software de rede"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Engenharia Social",
                question: "Spam é:",
                options: ["Email não solicitado", "Antivírus", "Firewall", "Senha forte"],
                correct: 0
            },

            // 🕵 ENGENHARIA SOCIAL - MÉDIO
            {
                level: "Médio",
                category: "Engenharia Social",
                question: "O que é um ataque DoS?",
                options: ["Negação de Serviço", "Roubo de dados", "Invasão física", "Antivírus"],
                correct: 0
            }, {
                level: "Médio",
                category: "Engenharia Social",
                question: "O que significa 2FA?",
                options: ["Autenticação de dois fatores", "Firewall avançado", "Backup duplo", "Antivírus"],
                correct: 0
            }, {
                level: "Médio",
                category: "Engenharia Social",
                question: "O que significa spoofing?",
                options: ["Falsificação de identidade", "Firewall", "Backup", "Antivírus"],
                correct: 0
            }, {
                level: "Médio",
                category: "Engenharia Social",
                question: "Que prática ajuda a evitar engenharia social?",
                options: ["Compartilhar senhas", "Desconfiar de pedidos suspeitos", "Usar senha fraca", "Clicar em qualquer link"],
                correct: 1
            }, {
                level: "Médio",
                category: "Engenharia Social",
                question: "Um keylogger serve para:",
                options: ["Capturar teclas digitadas", "Proteger rede", "Criptografar dados", "Bloquear spam"],
                correct: 0
            }, {
                level: "Médio",
                category: "Engenharia Social",
                question: "O que é shoulder surfing?",
                options: ["Espiar senha", "Firewall", "Antivírus", "Wi-Fi seguro"],
                correct: 0
            },

            // 🕵 ENGENHARIA SOCIAL - DIFÍCIL
            {
                level: "Difícil",
                category: "Engenharia Social",
                question: "Senha forte deve conter:",
                options: ["Nome e data", "Só números", "Letras, números e símbolos", "Somente letras"],
                correct: 2
            }, {
                level: "Difícil",
                category: "Engenharia Social",
                question: "O que é um firewall?",
                options: ["Barreira de proteção de rede", "Tipo de vírus", "Software de edição", "Hardware para armazenamento"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Engenharia Social",
                question: "O que é engenharia reversa?",
                options: ["Analisar e replicar sistema", "Firewall", "Criar vírus", "Criptografia"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Engenharia Social",
                question: "O que é ransomware?",
                options: ["Sequestro de dados", "Firewall", "Antivírus", "Criptografia de senha"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Engenharia Social",
                question: "O que significa spear phishing?",
                options: ["Phishing direcionado", "Firewall", "Spam", "Antivírus"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Engenharia Social",
                question: "Qual conceito envolve engenharia social em empresas?",
                options: ["Espionagem corporativa", "Firewall", "Antivírus", "Backup"],
                correct: 0
            }
        ];



        startBtn.addEventListener("click", () => {
            soundClick.play();
            playerName = document.getElementById("player-name").value.trim() || "Jogador";
            selectedLevel = document.getElementById("level-select").value;
            selectedCategory = document.getElementById("category-select").value;
            score = 0;
            currentQuestionIndex = 0;
            lives = 3;
            showScreen(quizScreen);
            bgMusic.volume = 0.2;
            bgMusic.play();

            filteredQuestions = questions.filter(q =>
                q.level.toLowerCase() === selectedLevel.toLowerCase() &&
                q.category.toLowerCase() === selectedCategory.toLowerCase()
            );

            startQuiz();
        });

        howToPlayBtn.addEventListener("click", () => {
            soundClick.play();
            howToPlayModal.classList.remove("hidden");
        });

        closeHowToBtn.addEventListener("click", () => {
            soundClick.play();
            howToPlayModal.classList.add("hidden");
        });

        rankingFixedBtn.addEventListener("click", () => {
            soundClick.play();
            showRanking();
        });

        rankingBackBtn.addEventListener("click", () => {
            soundClick.play();
            showScreen(startScreen);
        });

        quitBtn.addEventListener("click", () => {
            soundClick.play();
            resetGame();
        });

        nextBtn.addEventListener("click", () => {
            soundClick.play();
            nextQuestion();
        });

        function showScreen(screen) {
            [startScreen, quizScreen, howToPlayModal, rankingScreen].forEach(s => s.classList.add("hidden"));
            screen.classList.remove("hidden");
        }

        function startQuiz() {
            showQuestion();
        }

        function showQuestion() {
            clearInterval(timer);
            timeLeft = 15;
            updateTimerBar();
            timer = setInterval(() => {
                timeLeft--;
                updateTimerBar();
                if (timeLeft <= 0) {
                    clearInterval(timer);
                    loseLife();
                }
            }, 1000);

            const q = filteredQuestions[currentQuestionIndex];
            questionEl.textContent = q.question;
            optionsEl.innerHTML = "";
            q.options.forEach((opt, idx) => {
                const btn = document.createElement("button");
                btn.textContent = opt;
                btn.className = "option-btn";
                btn.addEventListener("click", () => selectOption(idx));
                optionsEl.appendChild(btn);
            });

            nextBtn.disabled = true;
        }

        function selectOption(index) {
            clearInterval(timer);
            const q = filteredQuestions[currentQuestionIndex]; // CORRIGIDO AQUI
            const buttons = document.querySelectorAll(".option-btn");
            buttons.forEach(b => b.disabled = true);

            if (index === q.correct) {
                buttons[index].classList.add("correct");
                score += 10;
                soundCorrect.play();
            } else {
                buttons[index].classList.add("wrong");
                buttons[q.correct].classList.add("correct");
                loseLife();
                soundWrong.play();
            }

            scoreboard.textContent = `Pontuação: ${score}`;
            nextBtn.disabled = false;
        }

        function nextQuestion() {
            currentQuestionIndex++;
            if (currentQuestionIndex >= filteredQuestions.length || lives <= 0) {
                endGame();
            } else {
                showQuestion();
            }
        }

        function loseLife() {
            lives--;
            const lifeSpans = livesEl.querySelectorAll("span");
            if (lifeSpans[lives]) {
                lifeSpans[lives].textContent = "💔";
            }
            if (lives <= 0) {
                endGame();
            } else {
                nextBtn.disabled = false;
            }
        }

        function updateTimerBar() {
            const percent = (timeLeft / 15) * 100;
            timerBar.style.width = percent + "%";
            timerBar.style.background = timeLeft > 5 ? "#0f0" : "#f00";
        }

        function endGame() {
            clearInterval(timer);
            saveRanking(playerName, score);
            showRanking();
            bgMusic.pause();
            bgMusic.currentTime = 0;
        }

        function saveRanking(name, score) {
            const ranking = JSON.parse(localStorage.getItem("techbrain_ranking")) || [];
            ranking.push({
                name,
                score
            });
            ranking.sort((a, b) => b.score - a.score);
            localStorage.setItem("techbrain_ranking", JSON.stringify(ranking.slice(0, 10)));
        }

        function showRanking() {
            showScreen(rankingScreen);
            const ranking = JSON.parse(localStorage.getItem("techbrain_ranking")) || [];
            rankingList.innerHTML = ranking.map(r => `<li>${r.name}: ${r.score} pts</li>`).join("");
        }

        function resetGame() {
            clearInterval(timer);
            bgMusic.pause();
            bgMusic.currentTime = 0;
            showScreen(startScreen);
        }

        const clearRankingBtn = document.getElementById("clear-ranking-btn");

        clearRankingBtn.addEventListener("click", () => {
            soundClick.play();
            localStorage.removeItem("techbrain_ranking");
            rankingList.innerHTML = "<li>Nenhum registro</li>";
        });
    </script>
</body>

</html>
