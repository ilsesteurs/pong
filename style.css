document.addEventListener('DOMContentLoaded', () => {
    const gameContainer = document.querySelector('.game-container');
    const leftPaddle = document.querySelector('.left-paddle');
    const rightPaddle = document.querySelector('.right-paddle');
    const ball = document.querySelector('.ball');
    const ball2 = document.querySelector('.ball2');
    const obstacle = document.querySelector('.obstacle');
    const powerUp = document.querySelector('.power-up');
    const powerDown = document.querySelector('.power-down');
    const playerScoreElement = document.getElementById('player-score');
    const computerScoreElement = document.getElementById('computer-score');
    const levelElement = document.getElementById('level');
    const timerElement = document.getElementById('timer');
    const startButton = document.getElementById('start-button');

    let ballX = 592.5;
    let ballY = 292.5;
    let ballSpeedX = 4;
    let ballSpeedY = 4;

    let ball2X = 592.5;
    let ball2Y = 292.5;
    let ball2SpeedX = -4;
    let ball2SpeedY = 4;

    let leftPaddleY = 250;
    let rightPaddleY = 250;
    const paddleSpeed = 10;

    let playerScore = 0;
    let computerScore = 0;
    let level = 1;

    let timeLeft = 60;
    let timerInterval;

    let obstacleActive = false;
    let powerUpActive = false;
    let powerDownActive = false;

    // Geluidseffecten
    const paddleSound = new Audio('sounds/633318__hewnmarrow__1950s_soviet_ping1.wav');
    const obstacleSound = new Audio('sounds/28077__klankschap__bonk.wav');
    const powerUpSound = new Audio('sounds/511385__mrthenoronha__power-up-8-bit.wav');
    const powerDownSound = new Audio('sounds/159399__noirenex__power-down.wav');

    // Start het spel
    startButton.addEventListener('click', () => {
        startTimer();
        gameLoop();
        startButton.style.display = 'none';
    });

    // Beweging van de bal
    function moveBall() {
        ballX += ballSpeedX;
        ballY += ballSpeedY;

        if (ballY <= 0 || ballY >= 585) ballSpeedY = -ballSpeedY;
        if (ballX <= 20 && ballY >= leftPaddleY && ballY <= leftPaddleY + 100) {
            ballSpeedX = -ballSpeedX;
            playSound(paddleSound);
            increaseBallSpeed();
        }
        if (ballX >= 1175 && ballY >= rightPaddleY && ballY <= rightPaddleY + 100) {
            ballSpeedX = -ballSpeedX;
            playSound(paddleSound);
            increaseBallSpeed();
        }
        if (obstacleActive && ballX >= parseInt(obstacle.style.left) && ballX <= parseInt(obstacle.style.left) + 40 && ballY >= parseInt(obstacle.style.top) && ballY <= parseInt(obstacle.style.top) + 40) {
            ballSpeedX = -ballSpeedX;
            playSound(obstacleSound);
        }

        if (ballX <= 0 || ballX >= 1185) {
            if (ballX <= 0) computerScore++;
            else playerScore++;
            updateScore();
            resetBall();
        }

        ball.style.left = ballX + 'px';
        ball.style.top = ballY + 'px';
    }

    // Beweging van de tweede bal
    function moveBall2() {
        ball2X += ball2SpeedX;
        ball2Y += ball2SpeedY;

        if (ball2Y <= 0 || ball2Y >= 585) ball2SpeedY = -ball2SpeedY;
        if (ball2X <= 20 && ball2Y >= leftPaddleY && ball2Y <= leftPaddleY + 100) {
            ball2SpeedX = -ball2SpeedX;
            playSound(paddleSound);
            increaseBallSpeed();
        }
        if (ball2X >= 1175 && ball2Y >= rightPaddleY && ball2Y <= rightPaddleY + 100) {
            ball2SpeedX = -ball2SpeedX;
            playSound(paddleSound);
            increaseBallSpeed();
        }
        if (obstacleActive && ball2X >= parseInt(obstacle.style.left) && ball2X <= parseInt(obstacle.style.left) + 40 && ball2Y >= parseInt(obstacle.style.top) && ball2Y <= parseInt(obstacle.style.top) + 40) {
            ball2SpeedX = -ball2SpeedX;
            playSound(obstacleSound);
        }

        if (ball2X <= 0 || ball2X >= 1185) {
            if (ball2X <= 0) computerScore++;
            else playerScore++;
            updateScore();
            resetBall2();
        }

        ball2.style.left = ball2X + 'px';
        ball2.style.top = ball2Y + 'px';
    }

    // Reset de bal
    function resetBall() {
        ballX = 592.5;
        ballY = 292.5;
        ballSpeedX = 4 + level;
        ballSpeedY = 4 + level;
    }

    // Reset de tweede bal
    function resetBall2() {
        ball2X = 592.5;
        ball2Y = 292.5;
        ball2SpeedX = -4 - level;
        ball2SpeedY = 4 + level;
    }

    // Update de score en het level
    function updateScore() {
        playerScoreElement.textContent = playerScore;
        computerScoreElement.textContent = computerScore;
        if (playerScore % 5 === 0) {
            level++;
            levelElement.textContent = level;
        }
    }

    // Beweging van de computerpeddel
    function moveComputerPaddle() {
        if (rightPaddleY + 50 < ballY) {
            rightPaddleY += paddleSpeed / 2 + level;
        } else if (rightPaddleY + 50 > ballY) {
            rightPaddleY -= paddleSpeed / 2 + level;
        }

        rightPaddle.style.top = rightPaddleY + 'px';
    }

    // Linkerpeddel besturen met de muis
    gameContainer.addEventListener('mousemove', (e) => {
        const rect = gameContainer.getBoundingClientRect();
        const mouseY = e.clientY - rect.top - 50;
        if (mouseY >= 0 && mouseY <= 500) {
            leftPaddleY = mouseY;
            leftPaddle.style.top = leftPaddleY + 'px';
        }
    });

    // Laat een obstakel verschijnen (minder vaak)
    function spawnObstacle() {
        if (!obstacleActive && Math.random() < 0.005) {
            obstacle.style.left = Math.floor(Math.random() * 1160) + 'px';
            obstacle.style.top = Math.floor(Math.random() * 560) + 'px';
            obstacle.style.display = 'block';
            obstacleActive = true;
            setTimeout(() => {
                obstacle.style.display = 'none';
                obstacleActive = false;
            }, 5000);
        }
    }

    // Laat een power-up verschijnen (minder vaak)
    function spawnPowerUp() {
        if (!powerUpActive && Math.random() < 0.003) {
            powerUp.style.left = Math.floor(Math.random() * 1160) + 'px';
            powerUp.style.top = Math.floor(Math.random() * 560) + 'px';
            powerUp.style.display = 'block';
            powerUpActive = true;
            setTimeout(() => {
                powerUp.style.display = 'none';
                powerUpActive = false;
            }, 5000);
        }
    }

    // Laat een power-down verschijnen (minder vaak)
    function spawnPowerDown() {
        if (!powerDownActive && Math.random() < 0.003) {
            powerDown.style.left = Math.floor(Math.random() * 1160) + 'px';
            powerDown.style.top = Math.floor(Math.random() * 560) + 'px';
            powerDown.style.display = 'block';
            powerDownActive = true;
            setTimeout(() => {
                powerDown.style.display = 'none';
                powerDownActive = false;
            }, 5000);
        }
    }

    // Timer
    function startTimer() {
        timerInterval = setInterval(() => {
            timeLeft--;
            timerElement.textContent = timeLeft;
            if (timeLeft <= 0) {
                clearInterval(timerInterval);
                alert(`Spel afgelopen! Speler: ${playerScore}, Computer: ${computerScore}`);
                resetGame();
            }
        }, 1000);
    }

    // Reset het spel
    function resetGame() {
        playerScore = 0;
        computerScore = 0;
        level = 1;
        timeLeft = 60;
        updateScore();
        levelElement.textContent = level;
        timerElement.textContent = timeLeft;
        resetBall();
        resetBall2();
        clearInterval(timerInterval);
        startTimer();
    }

    // Geluidseffecten afspelen
    function playSound(sound) {
        console.log("Geluid afgespeeld:", sound.src); // Debugging
        sound.currentTime = 0;
        sound.play().catch(error => {
            console.error("Fout bij afspelen van geluid:", error);
        });
    }

    // Verhoog de snelheid van de bal geleidelijk
    function increaseBallSpeed() {
        ballSpeedX *= 1.02; // Langzamere toename
        ballSpeedY *= 1.02; // Langzamere toename
        ball2SpeedX *= 1.02; // Langzamere toename
        ball2SpeedY *= 1.02; // Langzamere toename
    }

    // Game loop
    function gameLoop() {
        moveBall();
        moveBall2();
        moveComputerPaddle();
        spawnObstacle();
        spawnPowerUp();
        spawnPowerDown();
        requestAnimationFrame(gameLoop);
    }
});
