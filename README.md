<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulator Clicker Game!</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f5f5f5; /* Light gray background */
        }

        .game-container {
            text-align: center;
            background-color: #fff;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
        }

        h1 {
            color: #2196f3; /* Blue title */
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
        }

        #currencyDisplay {
            font-size: 1.5em;
            margin-bottom: 15px;
            color: #555;
        }

        #clickArea {
            width: 120px;
            height: 120px;
            background-color: #ffc107; /* Amber */
            border-radius: 20px;
            margin: 20px auto;
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.8em;
            color: white;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            transition: transform 0.1s ease-in-out, box-shadow 0.1s ease-in-out;
        }

        #clickArea:active {
            transform: scale(0.9);
            box-shadow: none;
        }

        .shop, .upgrades, .pets {
            margin-top: 25px;
            border-top: 2px solid #eee;
            padding-top: 25px;
        }

        .shop h2, .upgrades h2, .pets h2 {
            color: #3f51b5; /* Indigo headings */
            margin-bottom: 15px;
        }

        .shop button, .upgrades button, .pets button {
            padding: 10px 15px;
            margin: 5px;
            cursor: pointer;
            border: none;
            background-color: #4caf50; /* Green */
            color: white;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
            transition: background-color 0.3s ease;
        }

        .shop button:hover, .upgrades button:hover, .pets button:hover {
            background-color: #388e3c;
        }

        #characterPreview {
            width: 70px;
            height: 70px;
            border-radius: 10px;
            background-color: #795548; /* Brown */
            margin: 15px auto;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }

        .clicker-display {
            margin-top: 10px;
            font-style: italic;
            color: #777;
        }

        /* Floating number effect */
        .floating-number {
            position: absolute;
            pointer-events: none;
            animation: floatUp 1s forwards;
            color: #2196f3; /* Blue */
        }

        @keyframes floatUp {
            0% {
                transform: translateY(0);
                opacity: 1;
            }
            100% {
                transform: translateY(-50px);
                opacity: 0;
            }
        }

        /* Upgrade options */
        .upgrade-options {
            display: flex;
            justify-content: center;
            margin-top: 10px;
        }

        .upgrade-options button {
            padding: 8px 12px;
            margin: 5px;
            cursor: pointer;
            border: none;
            background-color: #9c27b0; /* Purple */
            color: white;
            border-radius: 5px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
            transition: background-color 0.3s ease;
        }

        .upgrade-options button:hover {
            background-color: #7b1fa2;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>Simulator Clicker Game!</h1>
        <div id="currencyDisplay">Currency: <span id="currency">0</span></div>
        <div id="clickArea">Click Me!</div>
        <div class="clicker-display">Current Clicker: <span id="currentClicker">Basic Clicker</span></div>

        <div class="shop">
            <h2>Shop</h2>
            <button onclick="buyClicker('lava')">Buy Lava Clicker (Cost: 20, Buff: +2)</button>
            <button onclick="buyClicker('water')">Buy Water Clicker (Cost: 30, Buff: +3)</button>
            <button onclick="buyClicker('ice')">Buy Ice Clicker (Cost: 40, Buff: +4)</button>
            <button onclick="buyClicker('fire')">Buy Fire Clicker (Cost: 50, Buff: +5)</button>
            <button onclick="buyClicker('earth')">Buy Earth Clicker (Cost: 60, Buff: +6)</button>
            <button onclick="buyClicker('wind')">Buy Wind Clicker (Cost: 70, Buff: +7)</button>
            <button onclick="buyClicker('electric')">Buy Electric Clicker (Cost: 80, Buff: +8)</button>
            <button onclick="buyClicker('stone')">Buy Stone Clicker (Cost: 90, Buff: +9)</button>
            <button onclick="buyClicker('iron')">Buy Iron Clicker (Cost: 100, Buff: +10)</button>
            <button onclick="buyClicker('gold')">Buy Gold Clicker (Cost: 110, Buff: +11)</button>
            <button onclick="buyClicker('diamond')">Buy Diamond Clicker (Cost: 120, Buff: +12)</button>
            <button onclick="buyClicker('ruby')">Buy Ruby Clicker (Cost: 130, Buff: +13)</button>
            <button onclick="buyClicker('emerald')">Buy Emerald Clicker (Cost: 140, Buff: +14)</button>
            <button onclick="buyClicker('sapphire')">Buy Sapphire Clicker (Cost: 150, Buff: +15)</button>
            <button onclick="buyClicker('amethyst')">Buy Amethyst Clicker (Cost: 160, Buff: +16)</button>
        </div>

        <div class="upgrades">
            <h2>Upgrades</h2>
            <div class="upgrade-options">
                <button onclick="buyUpgrade('autoClick')">Auto Click (Cost: 100, Passive: +1)</button>
                <button onclick="buyUpgrade('clickBoost')">Click Boost (Cost: 150, Buff: +2)</button>
            </div>
        </div>

        <div class="pets">
            <h2>Pets</h2>
            <button onclick="buyPet('dog')">Buy Dog (Cost: 50, Passive: +5)</button>
            <button onclick="buyPet('cat')">Buy Cat (Cost: 75, Passive: +8)</button>
        </div>
    </div>

    <script>
        let currency = 0;
        let clickValue = 1;
        let autoClickValue = 0;
        let characterColor = '#795548'; // Brown
        let currentClicker = 'Basic Clicker';
        let passiveIncome = 0;

        const currencyDisplay = document.getElementById('currency');
        const clickArea = document.getElementById('clickArea');
        const currentClickerDisplay = document.getElementById('currentClicker');
        const gameContainer = document.querySelector('.game-container');

        clickArea.addEventListener('click', (event) => {
            currency += clickValue;
            currencyDisplay.textContent = currency;

            // Create floating number
            const floatingNumber = document.createElement('div');
            floatingNumber.classList.add('floating-number');
            floatingNumber.textContent = '+' + clickValue;
            floatingNumber.style.left = (event.clientX - gameContainer.offsetLeft) + 'px';
            floatingNumber.style.top = (event.clientY - gameContainer.offsetTop - 20) + 'px';
            gameContainer.appendChild(floatingNumber);

            // Remove floating number after animation
            floatingNumber.addEventListener('animationend', () => {
                floatingNumber.remove();
            });
        });

        function buyClicker(type) {
            let cost = 0;
            let buff = 0;

            switch (type) {
                case 'lava':
                    cost = 20;
                    buff = 2;
                    break;
                case 'water':
                    cost = 30;
                    buff = 3;
                    break;
                case 'ice':
                    cost = 40;
                    buff = 4;
                    break;
                case 'fire':
                    cost = 50;
                    buff = 5;
                    break;
                case 'earth':
                    cost = 60;
                    buff = 6;
                    break;
                case 'wind':
                    cost = 70;
                    buff = 7;
                    break;
                case 'electric':
                    cost = 80;
                    buff = 8;
                    break;
                case 'stone':
                    cost = 90;
                    buff = 9;
                    break;
                case 'iron':
                    cost = 100;
                    buff = 10;
                    break;
                case 'gold':
                    cost = 110;
                    buff = 11;
                    break;
                case 'diamond':
                    cost = 120;
                    buff = 12;
                    break;
                case 'ruby':
                    cost = 130;
                    buff = 13;
                    break;
                case 'emerald':
                    cost = 140;
                    buff = 14;
                    break;
                case 'sapphire':
                    cost = 150;
                    buff = 15;
                    break;
                case 'amethyst':
                    cost = 160;
                    buff = 16;
                    break;
            }

            if (currency >= cost) {
                currency -= cost;
                clickValue = 1 + buff;
                currentClicker = type + ' Clicker';
                currencyDisplay.textContent = currency;
                currentClickerDisplay.textContent = currentClicker;
                alert('Clicker purchased! Click value increased.');
            } else {
                alert('Not enough currency!');
            }
        }

        function buyUpgrade(type) {
            let cost = 0;
            let passive = 0;
            let buff = 0;

            switch (type) {
                case 'autoClick':
                    cost = 100;
                    passive = 1;
                    break;
                case 'clickBoost':
                    cost = 150;
                    buff = 2;
                    break;
            }

            if (currency >= cost) {
                currency -= cost;
                if (type === 'autoClick') {
                    autoClickValue += passive;
                } else if (type === 'clickBoost') {
                    clickValue += buff;
                }
                currencyDisplay.textContent = currency;
                alert('Upgrade purchased!');
            } else {
                alert('Not enough currency!');
            }
        }

        function buyPet(type) {
            let cost = 0;
            let passive = 0;

            if (type === 'dog') {
                cost = 50;
                passive = 5;
            } else if (type === 'cat') {
                cost = 75;
                passive = 8;
            }

            if (currency >= cost) {
                currency -= cost;
                passiveIncome += passive;
                currencyDisplay.textContent = currency;
                alert('Pet purchased! Passive income increased.');
            } else {
                alert('Not enough currency!');
            }
        }

        // Example of passive income update (every 5 seconds)
        setInterval(() => {
            currency += passiveIncome + autoClickValue;
            currencyDisplay.textContent = currency;
        }, 5000);
    </script>
</body>
</html># max
