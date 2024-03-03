# WillYouBeRich.github.io
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: 'Arial', sans-serif;
      text-align: center;
      margin: 50px;
      background-color: #e6f7ff; /* Light blue background */
    }

    #wheel {
      width: 200px;
      height: 200px;
      border-radius: 50%;
      border: 2px solid #333;
      position: relative;
      margin: 20px auto;
      transition: transform 1s ease-out; /* Faster spin (adjust the duration as needed) */
      background: conic-gradient(
        #ff0000 0% 10%,     /* Red */
        #ffa500 10% 20%,    /* Orange */
        #ffff00 20% 30%,    /* Yellow */
        #008000 30% 40%,    /* Green */
        #0000ff 40% 50%,    /* Blue */
        #4b0082 50% 60%,    /* Indigo */
        #800080 60% 70%,    /* Violet */
        #ff0000 70% 80%,    /* Red */
        #ffa500 80% 90%,    /* Orange */
        #ffff00 90% 100%    /* Yellow */
      );
    }

    #wheel::before {
      content: '';
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 10px;
      height: 10px;
      background-color: #fff; /* White */
      border-radius: 50%;
    }

    #bidInput, #guessInput {
      margin-top: 10px;
      padding: 5px;
    }

    h1, p, label, #result, #virtualMoney {
      color: #333; /* Dark gray text */
    }

    button {
      background-color: #4CAF50; /* Green */
      border: none;
      color: white;
      padding: 10px 20px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
      cursor: pointer;
      border-radius: 5px;
      transition: background-color 0.3s;
    }

    button:hover {
      background-color: #45a049; /* Darker green on hover */
    }
  </style>
  <title>Colorful Wheel of Fortune</title>
</head>
<body>
  <h1 style="color: #ff6600;">Welcome to the Colorful Wheel of Fortune Game!</h1>
  <p style="color: #666666;">You start with $1,000,000 virtual money.</p>
  <label for="bidInput" style="color: #3366cc;">Enter your bidding amount:</label>
  <input type="number" id="bidInput" min="1" placeholder="Enter amount">
  <label for="guessInput" style="color: #3366cc;">Guess the number (1-10):</label>
  <input type="number" id="guessInput" min="1" max="10" placeholder="Enter guess">
  <div id="wheel" onclick="spinWheel()"></div>
  <p id="result" style="color: #cc3300;"></p>
  <p id="virtualMoney" style="color: #3366cc;"></p>
  <button onclick="spinWheel()" style="background-color: #ff6600;">Spin the Wheel</button>

  <script>
    let virtualMoney = parseInt(localStorage.getItem('virtualMoney')) || 1000000;

    function updateVirtualMoney() {
      document.getElementById('virtualMoney').innerHTML = `Virtual Money: $${virtualMoney}`;
    }

    function spinWheel() {
      const bidAmount = parseInt(document.getElementById('bidInput').value);
      const guessNumber = parseInt(document.getElementById('guessInput').value);
      const wheel = document.getElementById('wheel');
      const randomNumber = Math.floor(Math.random() * 10) + 1;

      if (bidAmount >= 1 && bidAmount <= virtualMoney && guessNumber >= 1 && guessNumber <= 10) {
        wheel.style.transform = `rotate(${360 * (randomNumber / 10)}deg)`;
        setTimeout(() => checkResult(randomNumber, bidAmount, guessNumber), 1000); // Adjust the duration as needed
      } else {
        document.getElementById('result').innerHTML = `Please enter valid bidding amount and guess within the specified ranges.`;
      }
    }

    function checkResult(randomNumber, bidAmount, guessNumber) {
      if (randomNumber === guessNumber) {
        virtualMoney += bidAmount * 2; // If the wheel stops at the guessed number, double the bidding amount.
        document.getElementById('result').innerHTML = `Congratulations! You won $${bidAmount * 2}.`;
      } else {
        virtualMoney -= bidAmount;
        document.getElementById('result').innerHTML = `Sorry, better luck next time. You lost $${bidAmount}. The correct number was ${randomNumber}.`;
      }

      localStorage.setItem('virtualMoney', virtualMoney);
      updateVirtualMoney();
    }

    // Initial setup and periodic reward
    updateVirtualMoney();
    setInterval(() => {
      virtualMoney += 1000; // Reward $1000 every hour
      localStorage.setItem('virtualMoney', virtualMoney);
      updateVirtualMoney();
    }, 3600000); // 1 hour in milliseconds
  </script>
</body>
</html>
