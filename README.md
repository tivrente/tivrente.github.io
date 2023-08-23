<html>
<head>
    <title>Piccolo Drinkgame</title>
    <style>
        /* Voeg de nieuwe CSS-regels voor full screen kaarten toe */
        .fullscreen {
            display: flex;
            justify-content: center;
            align-items: center;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: #ecf0f1;
            z-index: 9999;
            transform: scale(1.1); /* Inzoomen */
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
            text-align: center;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100%;
            box-sizing: border-box;
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

    let isPageVisible = true;
    document.addEventListener("visibilitychange", function() {
        isPageVisible = !document.hidden;
    });

        // Definieer de kaarten in een willekeurige volgorde
        const cards = [
            // ... (existing cards) ...
        "Drink het aantal slokken van het maandnummer waarin je bent geboren",
        "wat was je laatste aanraking met politie, xxx begint",
        "wie in deze groep heeft het meeste relaties gehad",
        "we tellen af en wijzen een random persoon aan, de persoon met het meeste stemmen drinkt zzz slokken.",
        "123 kijk zo nie, als je in iemand zijn ogen kijkt drink yyy slokken",
        "Iedereen 3 slokken",
        // ... (add more cards here) ...
        "Drink het aantal slokken van het maandnummer waarin je bent geboren",
        "wat was je laatste aanraking met politie, xxx begint",
        "wie in deze groep heeft het meeste relaties gehad",
        "we tellen af en wijzen een random persoon aan, de persoon met het meeste stemmen drinkt zzz slokken.",
        "123 kijk zo nie, als je in iemand zijn ogen kijkt drink yyy slokken",
        "Iedereen 3 slokken",
        "niet met rechts drinken, anders 1 slok",
        "doe je mee met the game of life? Als je weet van waar deze naam komt mag je 3 slokken uitdelen.",
        "xxx fluistert een vraag in iemand zijn oor en deze antwoord met een naam van de groep. Als iemand uit de groep meer slokken biedt dan de persoon wordt de vraag verteld.",
        "xxx zegt een never have i ever",
        "xxx, steek je drank in de microgolf",
        "iedereen drinkt",
        "Waterfall, xxx begint",
        "xxx verbiedt een woord, als je dit woord zegt drink een slok.",
        "Persoon mag opstaan,laatste zuppe ",
        "xxx is Snake eyes",
        "persoon die langste geen seks heeft gehad mag zzz slokken uitdelen",
        "Iedereen in jeugdbeweging mag yyy slokken uitdelen",
        "Rondje mexico",
        "Rondje opus",
        "Laatste die naar de wc is gegaan drinkt yyy slokken",
        "Vingeren verliezer drinkt zzz slokken",
        "Apple/android minste stemmen drinkt yyy slokken",
        "xxx kiest getal tussen 1 en 6, aaa mag het dubbele weggeven",
        "xxx kiest getal tussen 1 en 6... je mag deze zelf opdrinken",
        "iedereen zeg een getal, laagste getal drinkt zijn nummer",
        "wie is de sportiefste vd groep, deze persoon drinkt zzz slokken.",
        "wie is de luiste vd groep, deze persoon drinkt zzz slokken.",
        "wie zijn drankje voor minder dan de helft vol zit drinkt deze op.",
        "xxx vertelt 3 korte verhalen waarvan 1 waar is. voor elke persoon die het juist raad moet de verteller 1 slok drinken, anders drink je ze zelf op.",
        "xxx, drink 4 slokken nu of het dubbele drinken van wat de volgende persoon moet drinken.",
        "De eerste die iets wit aanraakt deelt 5 slokken uit.",
        "De eerste die iets paars aanraakt deelt 5 slokken uit.",
        "De eerste die iets roze aanraakt deelt 5 slokken uit.",
        "xxx pick a mate",
        "kies samen 1 persoon die moet rechtstaan. Jij bent de president en jij mag slokken zzz uitdelen",
        "xxx hoeveel drankjes heb je deze nacht al gehad = slokken die je mag uitdelen",
        "vanaf nu spreek je xxx aan met sletje",
        "elke keer als xxx lacht moet deze persoon een slok nemen",
        "niet meer naar wc, als straf krijg je een neckslap",
        "de persoon die over xxx zit moet yyy slokken drinken",
        "iedereen heeft een geweer met 3 kogels, je kan iemand een slok laten drinken door naar hem te schieten en PANG te zeggen.",
        "xxx heeft een uno reverse kaart (fluisteren in zijn oor)",
        "xxx kiest een categorie, eerste die afvalt 8 slokken en elke keer 1 minder",
        "xxx mag een regel verzinnen",
        "niet meer vloeken, yyy slokken als straf",
        "je mag het woord drinken niet meer zeggen, anders yyy slokken",
        "xxx kiest een woord/zin dat iedereen moet zeggen voordat hij drinkt",
        "Neem yyy slokken als je nog nooit buiten europa bent geweest",
        "Neem een shot als xxx ooit een liefdesbrief heeft geschreven.",
        "Neem een slok als je een geheim hebt dat je wilt delen met de groep",
        "speel blad steen schaar met je linker persoon, als je verliest drink je yyy slokken",
        "Drink je liever een liter mayonaise of een liter ketchup?",
        "Zou je liever voor altijd jeuk hebben of voor altijd plakkerig zijn? ",
        "Zou je liever je eigen duimnagel eraf trekken met een vork of een tandenstoker onder je grote teen steken en tegen een muur schoppen?",
        "Zou je liever aan andermans korstje likken of je eigen urine drinken?",
        "Zuig je liever een minuut lang op een gebruikte tampon of doe je liever een minuut lang een openbare toiletbril binnen?",
        "Word je liever van binnenuit opgegeten door maden of van buitenaf door mieren?",
        "Lik je liever aan het condoom van een vreemde of eet je liever een handvol maden uit een dood lijk?",
        "Heb je liever seks met de lekkerste persoon die je kent en die net dood is of heb je liever seks met de lelijkste en stinkendste persoon, maar die nog wel leeft? ",
        "Zou je liever een pot sperma van je pa of menstruatiebloed van je ma drinken?",
        "Vecht je liever tegen 100 paarden ter grootte van een eend of tegen één eend ter grootte van een paard?",
        "Zou je liever op een lul zitten en cake eten of op een cake zitten en een lul eten?",
        "Zou je liever een 6 cm penis hebben of man boobs?",
        "Zou je liever stront met chocoladesmaak eten of chocolade met strontsmaak?",
        "Ben je liever een vrouwelijke man of een mannelijke vrouw?",
        "xxx, Geef een compliment aan de speler links van je. Als het niet oprecht klinkt, neem jij een slok.",
        "Kies een speler om een liedje te zingen. Als anderen meezingen, nemen ze geen slok. Zo niet, dan nemen ze een slok.",
        "Noem een categorie (bijv. auto's, landen, films) en ga om de beurt in de klok mee en noem een item uit die categorie. Wie geen antwoord heeft, neemt een slok.",
        "Kies een speler om een waarheid te vertellen of zzz slokken te nemen.",
        "xxx begint met een woord, de volgende persoon heeft 2 seconden om een woord te zeggen die hier iets mee te maken heeft. als je niet binnen de tijd kan antwoorden neem je yyy slokken",
        "iedereen mag elk om zen beurt iets zeggen over xxx, als het positief is neemt deze persoon een slok, als het negatief is neem je zelf een",
        "wedstrijd: xxx zegt het alfabet achterstevoren, intussen at aaa zijn drinken. de verliezer at een drankje",
        "push up battle, xxx tegen aaa. de verliezer drinkt het verschil in slokken.",
        "xxx pomp 10 keer of drink zzz",
        "neem yyy slokken als je afgelopen week sex hebt gehad",
        "neem yyy slokken als je maagd bent",
        "welke persoon denken jullie dat de hoogste bodycount heeft wijs deze aan. Deze persoon mag zzz slokken drinken",
        "wie zal er als eerst in de gevangenis belanden wijs deze persoon aan.Deze persoon mag yyy slokken drinken",
        "wie is de sterkste persoon wijs deze persoon aan.Deze persoon mag yyy slokken drinken",
        "wie is de zwakste persoon wijs deze persoon aan.Deze persoon mag yyy slokken drinken",
        "wie is de knapste persoon wijs deze persoon aan.Deze persoon mag yyy slokken drinken",
        "xxx zeg een woord, de eerste persoon die een liedje heeft waar dit woord in voorkomt mag yyy slokken uitdelen",
        "xxx stel een vraag, de eerste persoon die er op kan antwoorden mag yyy solkken uitdelen",
        "xxx drink yyy slokken",
        "xxx deel yyy slokken uit",
        "xxx en aaa kies een pesoon om yyy slokken aan te geven",
        "xxx drink yyy slokken",
        "xxx at je drankje",
        "drink yyy keer als je al drugs hebt gedaan ",
        "drink yyy als ze een regel opnieuw hebben moeten zeggen omdat je niet aan het luisteren was",
        "als je je neus met je tong kan raken mag je yyy sloken uitdelen",
        "xxx als je ooit hebt gelachen met een gehandicapte persoon, shame on you, drink yyy slokken",
        "beslis wie de grootste mong is in de ruimte, deze persoon drinkt 3 slokken, en mag 3 slokken uitdelen",
        "drink yyy slokken als je denkt dat je de rest van je leven van je huidige partner gaat doorbrengen",
        "xxx drink zoveel je wilt. aaa moet het dubbele drinken",
        "drink yyy slokken als je niet kan plassen onder druk of als er mensen bij staan",
        "xxx als de laatste keer dat je hebt geplast thuis was geef 5 slokken uit, anders drink er 5",
        "rank de top 3 harigste speler. 3e plaats drink 1 slok, 2de 2 en 1e 3.",
        "xxx zeg ik heb ooit..., spelers moeten raden als het juist is of niet. verliezers drinken yyy slokken",
        "xxx je mag een regel verwijderen als deze er zijn. als er nog geen regels zijn mag je 5 slokken drinken",
        "zou je russian roulette spelen voor 1 milioen",
        "elk om zen beurt moet de volgende letter van het alfabet zeggen, als xxx zijn drankje voor die tijd heeft opgedronken drinkt iedereen zijn drank op.",
        "xxx kiest het volgende liedje, als je het liedje leuk vind drink je 4 slokken. ",
        "de volgende persoon die zijn gsm gebruikt (behelave de game master) drinkt 10 slokken.",
        "als je de vorige keer uit naar de kloten bent gegaan drink yyy slokken",
        "drink 3 keer als je rost bent of blauwe ogen hebt. 6 keer voor beide.",
        "xxx mag geen klinkers meer zeggen",
        "als je ooit gearresteerd bent drink yyy slokken",
        "xxx geef 5 slokken uit aan een persoon die minder dan jou heeft gedronken, als dit niet kan drink dan zelf",
        "drink je glas leeg als je meer dan 80 kg weegt",
        "drink 3 slokken als je ooit een fiets hebt gestolen, geef 3 slokken als er ooit een fiets van je is gestolen",
        "voetbal teams, de persoon dei niets weet of in herhaling valt drink 4 slokken",
        "vanaf nu als je naar het wc moet vraag je het aan de game master met een rijm, bv :ik moet dringend naar de wc, hopelijk is het geen diaree",
        "de persoon met het minste body hair drink 5 slokken",
        "xxx en aaa at battle, de winnaar krijgt immuniteit voor 15 min",
        "als je ooit cum hebt ingeslikt dirnk 3 keer",
        "boys drink yyy keer",
        "girls drink yyy keer",
        "xxx vind een regel uit",
        "drink 2 keer als je 2de teen groter is dan je grote teen",
        "xxx als je ooit bent in elkaar geslaan drink yyy keer",
        "dehene die de vorige moest drinken drinkt het zelfde aantal opnieuw",
        "xxx is thumb king",
        "wees beleefd tegen xxx die iedereen zijn drankje vanaf nu serveert",
        "xxx is question bitch",
        "vanaf nu spreek je xxx aan met meester",
        "vanaf nu spreek je xxx aan met sletje",
        "elke keer als xxx lacht moet deze persoon een slok nemen",
        "niet meer naar wc, als straf krijg je een neckslap",
        ];

        // Schud de kaarten in een willekeurige volgorde
        function shuffleCards(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        // Roep de functie aan om de kaarten te schudden
        shuffleCards(cards);

        // De rest van je JavaScript-code
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
                const formattedCard = getRandomCard(cards[cardIndex], randomPlayer);
                drinkInstruction.innerHTML = formattedCard;
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

        function getRandomCard(cardText, playerName) {
            const yyy = Math.floor(Math.random() * 3) + 2; // Willekeurig getal tussen 2 en 4
            const zzz = Math.floor(Math.random() * 4) + 5; // Willekeurig getal tussen 5 en 8
            return cardText
                .replace(/xxx/g, playerName)
                .replace(/yyy/g, yyy)
                .replace(/zzz/g, zzz);
        }
    </script>
</body>
</html>
