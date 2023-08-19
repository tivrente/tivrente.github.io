<!DOCTYPE html>
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
        <div id="playerList"></div>
    </div>

    <div id="gamePage">
        <button id="drawCardButton">Draw Card</button>
        <p id="drinkInstruction"></p>
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
        const playerList = document.getElementById("playerList");
        const gamePage = document.getElementById("gamePage");
        const drawCardButton = document.getElementById("drawCardButton");
        const drinkInstruction = document.getElementById("drinkInstruction");

        let players = [];

        startButton.addEventListener("click", showNameInput);
        addPlayerButton.addEventListener("click", addPlayer);
        startGameButton.addEventListener("click", startGame);
        drawCardButton.addEventListener("click", drawCard);

        function showNameInput() {
            startButton.style.display = "none";
            nameInput.style.display = "block";
        }

        function updatePlayerList() {
            const playerListContent = players.map(player => `<p>${player}</p>`).join("");
            playerList.innerHTML = playerListContent;
        }

        function addPlayer() {
            const playerName = playerNameInput.value;
            if (playerName) {
                players.push(playerName);
                playerNameInput.value = "";
                updatePlayerList();
            }
        }

        function startGame() {
            if (players.length > 0) {
                nameInput.style.display = "none";
                gamePage.style.display = "block";
            }
        }

        function drawCard() {
            if (players.length === 0) {
                alert("Enter player names first!");
                return;
            }

            const randomIndex = Math.floor(Math.random() * cards.length);
            const randomCard = cards[randomIndex];
            const randomPlayer = players[Math.floor(Math.random() * players.length)];
            const formattedCard = randomCard.replace(/xxx/g, randomPlayer);
            drinkInstruction.textContent = formattedCard;
        }
    </script>
</body>
</html>
