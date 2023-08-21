
<html>
<head>
    <title>Piccolo Drinkgame</title>
    <style>
        /* Voeg de nieuwe CSS-regels voor full screen kaarten toe */
        .fullscreen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: #ecf0f1;
            z-index: 9999;
            transform: scale(1.1); /* Inzoomen */
            margin-top: -20px; /* Verschuiven naar boven */
            margin-left: -20px; /* Verschuiven naar links */
        }
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
            overflow: hidden;
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
            padding: 40px; /* Grotere padding voor afstand tot de rand */
            transition: background-color 0.3s;
        }
        /* Pas de achtergrondkleur aan voor voldoende contrast met zwarte tekst */
        .card {
            background-color: #34495e;
            color: #fff;
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
    <!-- Voeg de Full Screen knop toe -->
    <button id="fullscreenButton">Full Screen</button>

    <div id="nameInput">
        <h2>Enter Player Names:</h2>
        <input type="text" id="playerName" placeholder="Player Name">
        <button id="addPlayerButton">Add Player</button>
        <button id="startGameButton">Start Game</button>
        <div class="playerNameContainer" id="enteredPlayers"></div>
    </div>

    <div id="gamePage" style="display: none;">
        <!-- Voeg de kaartcontainer met volledig scherm toe -->
        <div id="drinkInstruction" class="fullscreen card">
            <button id="backButton" style="position: absolute; top: 10px; left: 10px; display: none;">Back</button>
        </div>
    </div>

    <script>
        const cards = [
            "Drink het aantal slokken van het maandnummer waarin je bent geboren maandnummer",
            "wat was je laatste aanraking met politie, xxx begint",
            "wie in deze groep heeft het meeste relaties gehad",
            "we tellen af en wijzen een random persoon aan, de persoon met het meeste stemmen drinkt zzz slokken.",
            "123 kijk zo nie, als je in iemand zijn ogen kijkt drink yyy slokken",
            // Voeg meer kaarten toe hier
        ];

        const gamePage = document.getElementById("gamePage");
        const drinkInstruction = document.getElementById("drinkInstruction");
        const fullscreenButton = document.getElementById("fullscreenButton");
        const playerNameInput = document.getElementById("playerName");
        const addPlayerButton = document.getElementById("addPlayerButton");
        const startGameButton = document.getElementById("startGameButton");
        const enteredPlayers = document.getElementById("enteredPlayers");

        let players = [];

        addPlayerButton.addEventListener("click", addPlayer);
        startGameButton.addEventListener("click", startGame);

        let cardIndex = 0;
        let isShowingCard = false;

        drinkInstruction.addEventListener("click", showNextCard);
        fullscreenButton.addEventListener("click", toggleFullScreen);

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
                document.getElementById("nameInput").style.display = "none";
                gamePage.style.display = "block";
                showNextCard(); // Toon de eerste kaart
            }
        }

        function updateEnteredPlayers() {
            enteredPlayers.innerHTML = players.map(player => `<div class="playerName">${player}</div>`).join("");
        }

        function toggleFullScreen() {
            const element = document.documentElement;
            if (!document.fullscreenElement) {
                if (element.requestFullscreen) {
                    element.requestFullscreen();
                } else if (element.webkitRequestFullscreen) { // Voor Apple
                    element.webkitRequestFullscreen();
                }
            } else {
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                } else if (document.webkitExitFullscreen) { // Voor Apple
                    document.webkitExitFullscreen();
                }
            }
        }

        // Voeg een eventlistener toe om de "Back" knop te tonen bij het bekijken van de kaarten
        drinkInstruction.addEventListener("click", function() {
            document.getElementById("backButton").style.display = "block";
        });

        // Voeg een eventlistener toe aan de "Back" knop om terug te gaan naar het invoerscherm
        document.getElementById("backButton").addEventListener("click", function() {
            document.getElementById("gamePage").style.display = "none";
            document.getElementById("nameInput").style.display = "block";
            document.getElementById("backButton").style.display = "none";
        });
        
        // Voeg een eventlistener toe voor het draaien van de gsm
        window.addEventListener("orientationchange", function() {
            if (document.fullscreenElement) {
                // Vernieuw de full-screen modus om de tekst op het scherm te houden na draaien
                document.exitFullscreen().then(function() {
                    toggleFullScreen();
                });
            }
        });
    </script>
</body>
</html>

