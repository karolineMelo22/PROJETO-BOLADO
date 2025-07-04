<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Quiz Gamer - TechBrain</title>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

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
            text-shadow: 0 0 5px rgb(32, 253, 12);
        }
        
        .overlay {
            background: rgba(0, 0, 0, 0.40);
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
        
        input[type="text"],
        select,
        button {
            font-family: 'Press Start 2P', cursive;
            margin-bottom: 20px;
            padding: 14px 25px;
            font-size: 1.1rem;
            border: 3px solid #00ff00;
            background: rgba(0, 0, 0, 0.8);
            color: #00ff00;
            outline: none;
            text-align: center;
            border-radius: 12px;
            box-shadow: 0 0 8px #00ff00, inset 0 0 10px #003300;
            transition: box-shadow 0.4s ease, color 0.4s ease, background 0.4s ease, transform 0.2s ease;
            cursor: pointer;
        }
        
        input[type="text"]:focus,
        select:focus,
        button:focus {
            box-shadow: 0 0 20px #00ff00, inset 0 0 15px #00ff00;
            color: #aaffaa;
            background: rgba(0, 30, 0, 0.95);
        }
        
        input[type="text"]:hover,
        select:hover,
        button:hover {
            box-shadow: 0 0 18px #00ff00, inset 0 0 15px #009900;
            color: #ccffcc;
            background: rgba(0, 40, 0, 0.9);
        }
        
        button:active {
            transform: scale(0.95);
            box-shadow: 0 0 5px #00cc00, inset 0 0 20px #00cc00;
            color: #99ff99;
            background: rgba(0, 50, 0, 1);
        }
        
        #how-to-play-btn {
            position: fixed;
            top: 15px;
            left: 15px;
            background: transparent;
            border: 2px solid #00ff00;
            color: #00ff00;
            padding: 10px 15px;
            border-radius: 5px;
            font-family: 'Press Start 2P', cursive;
            font-size: 0.77rem;
            cursor: pointer;
            z-index: 9999;
            transition: background 0.3s, color 0.3s;
        }
        
        #how-to-play-btn:hover {
            background: #00ff00;
            color: #000;
        }
        
        #whatsapp-share-btn {
            margin-top: 10px;
            padding: 12px 20px;
            background: transparent;
            border: 2px solid #46f340;
            color: #4dfd07;
            font-family: 'Press Start 2P', cursive;
            font-size: 0.7rem;
            cursor: pointer;
            border-radius: 5px;
            transition: background 0.3s, color 0.3s;
        }
        
        #whatsapp-share-btn:hover {
            background: #51ff1c;
            color: #000;
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
        
        #congratsScreen {
            z-index: 9999;
        }
        
        #congratsScreen h1 {
            color: #00ff00;
            font-size: 2rem;
            margin-bottom: 20px;
            text-align: center;
        }
        
        #congratsScreen p {
            font-size: 1rem;
            color: #fff;
            text-align: center;
            margin-bottom: 20px;
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
            background: #27e427;
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
            background: #47f741;
            color: #000;
        }
        
        select {
            width: 280px;
            padding: 14px 25px;
            font-size: 1.1rem;
            font-family: 'Press Start 2P', cursive;
            background: rgba(0, 0, 0, 0.8);
            color: #41ff28;
            border: 3px solid #38f831;
            border-radius: 12px;
            outline: none;
            box-shadow: 0 0 8px #690d65, inset 0 0 10px #003300;
            text-align-last: center;
            appearance: none;
            -webkit-appearance: none;
            -moz-appearance: none;
            cursor: pointer;
            transition: box-shadow 0.4s ease, color 0.4s ease, background 0.4s ease, transform 0.2s ease;
        }
        
        #quiz-container {
            max-width: 700px;
            width: 90%;
            margin: 40px auto;
            padding: 30px;
            background: #050505e3;
            color: #0f0;
            border-radius: 15px;
            box-shadow: 0 0 15px #520855;
            font-family: 'Press Start 2P', cursive;
            text-align: center;
        }
        
        #ranking-list {
            list-style: none;
            padding: 0;
            margin: 30px 0;
            max-height: 400px;
            overflow-y: auto;
        }
        
        #ranking-list li {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 20px;
            margin-bottom: 10px;
            background: #000;
            border-radius: 8px;
            animation: fadeIn 0.4s ease;
            border: 1px solid #0f0;
        }
        
        #ranking-list li.highlight {
            background: rgba(82, 4, 82, 0.849);
            box-shadow: 0 0 12px #8ffa15;
            font-weight: bold;
        }
        
        #ranking-list span.pos {
            width: 50px;
            text-align: left;
        }
        
        #ranking-list span.name {
            flex-grow: 1;
            text-align: center;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
        
        #ranking-list span.score {
            width: 100px;
            text-align: right;
        }
        
        .ranking-buttons {
            display: flex;
            justify-content: center;
            gap: 15px;
        }
        
        .ranking-buttons button {
            padding: 10px 20px;
            font-weight: bold;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            background: #b60ce0;
            color: #000;
            transition: 0.2s;
        }
        
        .ranking-buttons button:hover {
            background: rgb(99, 241, 71);
        }
        
        .medal {
            font-size: 1.2em;
            margin-right: 6px;
        }
        
        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
    </style>
</head>

<body>

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
        <button id="how-to-play-btn">🎮 Como Jogar</button>

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
            <ul id="ranking-list"></ul>
            <p id="empty-ranking" style="display: none;">Nenhum registro ainda. Jogue e conquiste seu lugar!</p>

            <div class="ranking-buttons">
                <button id="ranking-back-btn">Voltar</button>
                <button id="clear-ranking-btn">Limpar Ranking</button>
            </div>
        </div>
    </div>

    <div id="congratsScreen" class="overlay hidden">
        <h1>🎉 Parabéns, <span id="congrats-player-name">Jogador</span>!</h1>
        <p>Você acertou todas as perguntas e venceu o desafio!</p>
        <button onclick="resetGame()">Voltar ao início</button>
        <button id="whatsapp-share-btn">📤 Compartilhar no WhatsApp</button>
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
        let filteredQuestions = [];


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
            }, {
                level: "Fácil",
                category: "Games",
                question: "Qual jogo é conhecido pelo personagem Lara Croft?",
                options: ["Tomb Raider", "Assassin's Creed", "Uncharted", "Halo"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Games",
                question: "Em qual jogo você pode construir com blocos em um mundo aberto?",
                options: ["Minecraft", "Overwatch", "FIFA", "Call of Duty"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Games",
                question: "Qual desses jogos é de estratégia em tempo real?",
                options: ["Age of Empires", "Super Mario Bros", "Fortnite", "Pac-Man"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Games",
                question: "Quem é o personagem principal de 'Sonic the Hedgehog'?",
                options: ["Sonic", "Tails", "Knuckles", "Shadow"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Games",
                question: "Qual console é fabricado pela Microsoft?",
                options: ["Xbox", "PlayStation", "Switch", "GameCube"],
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
            }, {
                level: "Médio",
                category: "Games",
                question: "Qual jogo da série 'The Elder Scrolls' se passa na província de Skyrim?",
                options: ["Skyrim", "Oblivion", "Morrowind", "Daggerfall"],
                correct: 0
            }, {
                level: "Médio",
                category: "Games",
                question: "Qual jogo famoso tem uma moeda chamada 'V-Bucks'?",
                options: ["Fortnite", "Minecraft", "League of Legends", "Overwatch"],
                correct: 0
            }, {
                level: "Médio",
                category: "Games",
                question: "Qual estúdio é responsável pela franquia 'Mass Effect'?",
                options: ["BioWare", "Valve", "Blizzard", "Rockstar"],
                correct: 0
            }, {
                level: "Médio",
                category: "Games",
                question: "Em que ano foi lançado o jogo 'Half-Life 2'?",
                options: ["2004", "2001", "2006", "2000"],
                correct: 0
            }, {
                level: "Médio",
                category: "Games",
                question: "Qual desses jogos é um MOBA (Multiplayer Online Battle Arena)?",
                options: ["League of Legends", "Call of Duty", "FIFA", "Minecraft"],
                correct: 0
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
            }, {
                level: "Difícil",
                category: "Games",
                question: "Qual foi o primeiro jogo da série 'Dark Souls' a ser lançado?",
                options: ["Demon’s Souls", "Dark Souls", "Bloodborne", "Sekiro"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Games",
                question: "Qual linguagem de programação é usada para criar mods no jogo 'Skyrim'?",
                options: ["Papyrus", "Lua", "Python", "JavaScript"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Games",
                question: "Qual é o nome da principal cidade em 'The Witcher 3'?",
                options: ["Novigrad", "Oxenfurt", "Kaer Morhen", "Vizima"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Games",
                question: "Qual engine foi usada para desenvolver 'The Last of Us Part II'?",
                options: ["Naughty Dog Engine", "Unreal Engine", "Frostbite", "Unity"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Games",
                question: "Qual o nome do diretor de arte da série 'Metroid'?",
                options: ["Kenji Yamamoto", "Yoshio Sakamoto", "Shigeru Miyamoto", "Hidetaka Miyazaki"],
                correct: 1
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
            }, {
                level: "Fácil",
                category: "Programação",
                question: "O que significa HTML?",
                options: ["HyperText Markup Language", "HyperText Markdown Language", "Hyperlink Text Markup Language", "Hyper Transfer Markup Language"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Programação",
                question: "Qual é o símbolo para declarar comentários em CSS?",
                options: ["/* comentário */", "// comentário", "<!-- comentário -->", "# comentário"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Programação",
                question: "Qual tag HTML é usada para criar um parágrafo?",
                options: ["<p>", "<div>", "<span>", "<h1>"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Programação",
                question: "JavaScript roda no:",
                options: ["Navegador", "Servidor somente", "Banco de dados", "Editor de texto"],
                correct: 0
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
            }, {
                level: "Médio",
                category: "Programação",
                question: "Qual método converte uma string para um número em JS?",
                options: ["parseInt()", "toString()", "Number()", "parseFloat()"],
                correct: 0
            }, {
                level: "Médio",
                category: "Programação",
                question: "O que faz o método .filter() em um array JS?",
                options: ["Filtra elementos com base em condição", "Adiciona elementos", "Remove elementos", "Ordena elementos"],
                correct: 0
            }, {
                level: "Médio",
                category: "Programação",
                question: "Qual palavra-chave cria uma variável que pode ser alterada em JS?",
                options: ["let", "const", "var", "final"],
                correct: 0
            }, {
                level: "Médio",
                category: "Programação",
                question: "Qual operador lógico representa 'OU' em JS?",
                options: ["||", "&&", "!", "&"],
                correct: 0
            }, {
                level: "Médio",
                category: "Programação",
                question: "Qual comando interrompe um loop em JS?",
                options: ["break", "continue", "stop", "exit"],
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
            }, {
                level: "Difícil",
                category: "Programação",
                question: "Qual conceito descreve funções que retornam outras funções em JS?",
                options: ["Funções de ordem superior", "Closures", "Callbacks", "Promises"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Programação",
                question: "O que significa 'immutabilidade' em programação funcional?",
                options: ["Dados que não mudam", "Variáveis globais", "Funções anônimas", "Laços infinitos"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Programação",
                question: "Qual é a principal vantagem do TypeScript sobre JavaScript?",
                options: ["Tipagem estática", "Sintaxe simples", "Mais rápido", "Suporte a CSS"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Programação",
                question: "O que é o padrão 'Observer'?",
                options: ["Permite notificar objetos sobre mudanças", "Cria objetos únicos", "Controla acesso a métodos", "Faz lazy loading"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Programação",
                question: "Em programação assíncrona, o que é um 'Promise'?",
                options: ["Objeto que representa um valor futuro", "Função síncrona", "Variável global", "Loop infinito"],
                correct: 0
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
            }, {
                level: "Fácil",
                category: "Engenharia Social",
                question: "Qual é o objetivo principal da engenharia social?",
                options: ["Manipular pessoas para obter informações", "Criar vírus", "Proteger redes", "Desenvolver software"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Engenharia Social",
                question: "Qual desses é um método comum de ataque de engenharia social?",
                options: ["Phishing", "Malware", "SQL Injection", "DDoS"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Engenharia Social",
                question: "O que é um 'pretexting'?",
                options: ["Fingir ser outra pessoa para obter informações", "Tipo de malware", "Firewall", "Senha forte"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Engenharia Social",
                question: "Como evitar ataques de engenharia social?",
                options: ["Desconfiar de solicitações suspeitas", "Compartilhar senhas", "Clicar em links desconhecidos", "Usar Wi-Fi público"],
                correct: 0
            }, {
                level: "Fácil",
                category: "Engenharia Social",
                question: "O que é um 'baiting' em engenharia social?",
                options: ["Isca para atrair vítimas", "Tipo de vírus", "Firewall", "Backup de dados"],
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
            }, {
                level: "Médio",
                category: "Engenharia Social",
                question: "Qual é a definição de 'spoofing'?",
                options: ["Falsificar identidade para enganar", "Ataque físico", "Antivírus", "Firewall"],
                correct: 0
            }, {
                level: "Médio",
                category: "Engenharia Social",
                question: "O que é 'shoulder surfing'?",
                options: ["Observar informações secretas por cima do ombro", "Ataque de vírus", "Firewall", "Senha forte"],
                correct: 0
            }, {
                level: "Médio",
                category: "Engenharia Social",
                question: "O que é um 'watering hole attack'?",
                options: ["Comprometer site para infectar vítimas", "Ataque de negação de serviço", "Firewall", "Antivírus"],
                correct: 0
            }, {
                level: "Médio",
                category: "Engenharia Social",
                question: "O que é 'tailgating' em segurança física?",
                options: ["Entrar em área restrita seguindo alguém", "Roubo de dados", "Ataque de phishing", "Firewall"],
                correct: 0
            }, {
                level: "Médio",
                category: "Engenharia Social",
                question: "Como o 'spear phishing' difere do phishing tradicional?",
                options: ["É direcionado a indivíduos específicos", "É um ataque em massa", "Não é perigoso", "É usado para proteção"],
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
            }, {
                level: "Difícil",
                category: "Engenharia Social",
                question: "O que significa 'Man-in-the-Middle' em segurança digital?",
                options: ["Interceptação de comunicação entre duas partes", "Ataque físico", "Firewall", "Antivírus"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Engenharia Social",
                question: "O que é um 'honeypot' em segurança da informação?",
                options: ["Sistema para atrair e analisar invasores", "Firewall avançado", "Backup automático", "Antivírus"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Engenharia Social",
                question: "Qual é o principal propósito do 'social engineering toolkit' (SET)?",
                options: ["Ferramenta para simular ataques de engenharia social", "Criar senhas seguras", "Proteger redes", "Gerenciar backups"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Engenharia Social",
                question: "O que é 'cryptojacking'?",
                options: ["Uso não autorizado de recursos para mineração de criptomoedas", "Ataque de phishing", "Ataque físico", "Firewall"],
                correct: 0
            }, {
                level: "Difícil",
                category: "Engenharia Social",
                question: "O que caracteriza um ataque de 'zero-day'?",
                options: ["Exploração de vulnerabilidade desconhecida", "Ataque já conhecido e corrigido", "Backup automático", "Firewall atualizado"],
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
            resetLifeDisplay();
            scoreboard.textContent = "Pontuação: 0";
            showScreen(quizScreen);
            bgMusic.volume = 0.2;
            bgMusic.play();

            rankingFixedBtn.classList.add("hidden");


            filteredQuestions = shuffleQuestions(questions.filter(q =>
                q.level.toLowerCase() === selectedLevel.toLowerCase() &&
                q.category.toLowerCase() === selectedCategory.toLowerCase()
            ));

            startQuiz();

        });

        quitBtn.addEventListener("click", () => {
            soundClick.play();
            resetGame();
        });

        function resetLifeDisplay() {
            const lifeSpans = livesEl.querySelectorAll("span");
            lifeSpans.forEach(span => {
                span.textContent = "❤";
            });
        }

        function resetGame() {
            clearInterval(timer);
            bgMusic.pause();
            bgMusic.currentTime = 0;
            lives = 3;
            resetLifeDisplay();
            scoreboard.textContent = "Pontuação: 0";
            showScreen(startScreen);

        }

        function showScreen(screen) {
            [startScreen, quizScreen, howToPlayModal, rankingScreen, congratsScreen].forEach(s => s.classList.add("hidden"));
            screen.classList.remove("hidden");
        }

        function launchConfetti() {
            confetti({
                particleCount: 150,
                spread: 70,
                origin: {
                    y: 0.6
                }
            });
        }



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



        function shuffleQuestions(arr) {
            return [...arr].sort(() => Math.random() - 0.5);
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
            bgMusic.pause();
            bgMusic.currentTime = 0;

            const maxScore = filteredQuestions.length * 10;
            if (score === maxScore) {
                document.getElementById("congrats-player-name").textContent = playerName;
                showScreen(congratsScreen);
                document.getElementById("congrats-player-name").textContent = playerName;
                showScreen(congratsScreen);
                launchConfetti();

            } else {
                showRanking();
            }
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
            const rankingList = document.getElementById("ranking-list");
            const emptyMsg = document.getElementById("empty-ranking");

            rankingList.innerHTML = "";

            if (ranking.length === 0) {
                emptyMsg.style.display = "block";
                return;
            } else {
                emptyMsg.style.display = "none";
            }

            const playerAtual = window.playerName || "Jogador"; // pega jogador atual

            ranking.forEach((r, index) => {
                const li = document.createElement("li");

                const pos = document.createElement("span");
                pos.className = "pos";
                if (index === 0) pos.innerHTML = "🥇";
                else if (index === 1) pos.innerHTML = "🥈";
                else if (index === 2) pos.innerHTML = "🥉";
                else pos.textContent = index + 1;

                const name = document.createElement("span");
                name.className = "name";
                name.textContent = r.name;

                const score = document.createElement("span");
                score.className = "score";
                score.textContent = `${r.score} pts`;

                li.appendChild(pos);
                li.appendChild(name);
                li.appendChild(score);

                if (r.name === playerAtual) {
                    li.classList.add("highlight");
                }

                rankingList.appendChild(li);
            });
        }


        function resetGame() {
            clearInterval(timer);
            bgMusic.pause();
            bgMusic.currentTime = 0;
            showScreen(startScreen);

            rankingFixedBtn.classList.remove("hidden");

        }


        function shareOnWhatsApp() {
            const message = `🎉 Eu acertei tudo no Quiz Gamer - TechBrain! Jogue também! 💻🔥`;
            const url = `https://wa.me/?text=${encodeURIComponent(message)}`;
            window.open(url, '_blank');
        }


        const clearRankingBtn = document.getElementById("clear-ranking-btn");

        clearRankingBtn.addEventListener("click", () => {
            soundClick.play();
            localStorage.removeItem("techbrain_ranking");
            rankingList.innerHTML = "<li>Nenhum registro</li>";
        });

        document.getElementById("whatsapp-share-btn").addEventListener("click", () => {
            const message = `🎮 Eu acertei tudo no Quiz Gamer - TechBrain! Jogue também! https://karolinemelo22.github.io/PROJETO-BOLADO/ 💻🔥`;
            const url = `https://wa.me/?text=${encodeURIComponent(message)}`;
            window.open(url, "_blank");
        });
    </script>
</body>

</html>
