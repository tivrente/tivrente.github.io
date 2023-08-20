
<html>
<head>
    <title>Piccolo Drinkgame</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }
        h1 {
            color: #333;
            margin-top: 20px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #3498db;
            color: #fff;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #2980b9;
        }
        #nameInput, #gamePage {
            display: none;
            padding: 20px;
        }
        input[type="text"] {
            padding: 10px;
            width: 200px;
        }
        #playerList {
            font-size: 16px;
            margin-top: 20px;
            text-align: left;
        }
        #drinkInstruction {
            font-size: 24px;
            margin-top: 20px;
            padding: 20px;
            transition: background-color 0.3s;
        }
        .card {
            background-color: #ecf0f1;
            padding: 20px;
            margin: 10px;
            border-radius: 10px;
            font-size: 18px;
        }
        .playerNameContainer {
            margin-top: 10px;
            text-align: left;
            display: flex;
            align-items: center;
            flex-wrap: wrap;
            max-width: 300px;
            margin: auto;
        }
        .playerName {
            background-color: #3498db;
            color: #fff;
            padding: 5px 10px;
            border-radius: 5px;
            margin: 5px;
        }
    </style>
</head>
<body>
    <h1>Piccolo Drinkgame</h1>
    <button id="startButton">Start Game</button>

    <div id="nameInput">
        <h2>Enter Player Names:</h2>
        <input type="text" id="playerName" placeholder="Player Name">
        <button id="addPlayerButton">Add Player</button>
        <button id="startGameButton">Start Game</button>
        <div class="playerNameContainer" id="enteredPlayers"></div>
    </div>

    <div id="gamePage">
        <div id="drinkInstruction" class="card"></div>
    </div>

    <script>
        const cards = [
            "xxx takes a sip!",
            "xxx Pick someone to take a sip.",
            "xxx Take two sips.",
            "Waterfall! Everyone keeps drinking until the person to their right stops. xxx starts",
            "Rhyme time! xxx Say a word, and everyone has to say a word that rhymes.",
            // Voeg meer kaarten toe hier
        ];

        const startButton = document.getElementById("startButton");
        const nameInput = document.getElementById("nameInput");
        const playerNameInput = document.getElementById("playerName");
        const addPlayerButton = document.getElementById("addPlayerButton");
        const startGameButton = document.getElementById("startGameButton");
        const gamePage = document.getElementById("gamePage");
        const drawCardButton = document.getElementById("drawCardButton");
        const drinkInstruction = document.getElementById("drinkInstruction");
        const enteredPlayers = document.getElementById("enteredPlayers");

        let players = [];

        startButton.addEventListener("click", showNameInput);
        addPlayerButton.addEventListener("click", addPlayer);
        startGameButton.addEventListener("click", startGame);

        let cardIndex = 0;
        let isShowingCard = false;

        document.addEventListener("click", showNextCard);

        function showNextCard() {
            if (!isShowingCard) {
                isShowingCard = true;
                if (cardIndex >= cards.length) {
                    cardIndex = 0;
                }
                const randomPlayer = players[Math.floor(Math.random() * players.length)];
                const formattedCard = cards[cardIndex].replace(/xxx/g, randomPlayer);
                drinkInstruction.textContent = formattedCard;
                drinkInstruction.style.backgroundColor = getRandomColor();
                cardIndex++;
                setTimeout(() => {
                    isShowingCard = false;
                }, 500); // Voeg eventueel een kortere of langere tijd toe voor de overgang
            }
        }

        function getRandomColor() {
            const letters = "0123456789ABCDEF";
            let color = "#";
            for (let i = 0; i < 6; i++) {
                color += letters[Math.floor(Math.random() * 16)];
            }
            return color;
        }

        function showNameInput() {
            startButton.style.display = "none";
            nameInput.style.display = "block";
        }

        function addPlayer() {
            const playerName = playerNameInput.value;
            if (playerName && !players.includes(playerName)) {
                players.push(playerName);
                playerNameInput.value = "";
                updateEnteredPlayers();
            }
        }

        function startGame() {
            if (players.length > 0) {
                nameInput.style.display = "none";
                gamePage.style.display = "block";
            }
        }

        function updateEnteredPlayers() {
            enteredPlayers.innerHTML = players.map(player => `<div class="playerName">${player}</div>`).join("");
        }
    </script>
</body>
</html>
