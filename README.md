# Morning-my-love
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Morning Archer - Giselle ❤️</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; touch-action: manipulation; }
        html, body { width: 100%; height: 100%; overflow: hidden; background: #000; position: fixed; font-family: -apple-system, system-ui, sans-serif; }

        #sky {
            position: fixed; inset: 0;
            background: linear-gradient(to bottom, #020111 0%, #000000 100%);
            transition: background 2s ease-in-out; z-index: -2;
        }

        #sun {
            position: absolute; bottom: -150px; left: 50%; transform: translateX(-50%);
            width: 140px; height: 140px; background: radial-gradient(circle, #fff700, #ff8c00);
            border-radius: 50%; box-shadow: 0 0 80px #ff8c00; z-index: -1;
            transition: bottom 2s ease-out;
        }

        #basket {
            position: absolute; bottom: -20px; width: 120%; left: -10%; height: 30%;
            background: #5d4037; border-top: 10px solid #3e2723;
            border-radius: 50% 50% 0 0; z-index: 10;
            box-shadow: inset 0 10px 40px rgba(0,0,0,0.6);
        }

        #bow {
            position: absolute; bottom: 5%; left: 50%; transform: translateX(-50%);
            width: 180px; height: auto; z-index: 20; pointer-events: none;
            transition: transform 0.1s;
        }

        #hud {
            position: absolute; top: 60px; width: 100%; display: flex;
            justify-content: space-around; z-index: 100;
        }
        .stat-box {
            background: rgba(255,255,255,0.15); padding: 10px 20px;
            border-radius: 20px; backdrop-filter: blur(10px);
            color: white; font-weight: bold; border: 1px solid rgba(255,255,255,0.3);
        }

        .target { position: absolute; font-size: 55px; z-index: 5; user-select: none; cursor: pointer; }

        .screen {
            position: fixed; inset: 0; z-index: 200;
            display: flex; flex-direction: column; justify-content: center;
            align-items: center; text-align: center; padding: 30px;
        }
        #start-screen { background: rgba(0,0,0,0.9); color: white; }
        #win-screen { display: none; background: rgba(255, 255, 255, 0.98); color: #333; overflow-y: auto; }

        .btn {
            padding: 15px 35px; font-size: 1.1rem; border-radius: 50px;
            border: none; background: #fbc02d; color: #333;
            font-weight: bold; margin-top: 15px; box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }

        .countdown-box {
            margin-top: 15px; padding: 12px; background: #fff8e1;
            border-radius: 15px; border: 2px dashed #fbc02d;
            font-weight: bold; color: #f57c00; width: 100%; max-width: 280px;
        }

        #secretLetter {
            display: none; margin-top: 15px; padding: 15px;
            background: #fff0f3; border-radius: 15px;
            font-style: italic; border-left: 5px solid #ff4d6d;
            font-size: 0.95rem; color: #444; line-height: 1.5;
        }
    </style>
</head>
<body>

<div id="sky"></div>
<div id="sun"></div>
<div id="basket"></div>
<div id="bow">🏹</div>

<div id="start-screen" class="screen">
    <h1 style="color: #fbc02d; font-size: 2rem;">Good Morning, Giselle! ❤️</h1>
    <p style="margin: 20px 0;">Shoot 10 sunbeams to bring out the sun!</p>
    <button class="btn" onclick="startGame()">Start Flying 🎈</button>
</div>

<div id="hud">
    <div class="stat-box">Shots: <span id="score">0</span>/10</div>
</div>

<div id="win-screen" class="screen">
    <h1 style="color: #f57c00; margin-bottom: 5px;">The Sun has Risen! ☀️</h1>
    <p style="font-size: 0.9rem; color: #666; margin-bottom: 10px;">
        Good morning, my love! I hope you have a perfect day.
    </p>
    
    <div class="countdown-box">
        🏔️ Seattle Trip In: <br>
        <span id="timerText">Calculating...</span>
    </div>

    <button class="btn" onclick="showLetter()" id="letterBtn" style="background: #ff4d6d; color: white;">Open My Letter 💌</button>

    <div id="secretLetter">
        "Giselle, you are absolutely beautiful, smart, and perfect. I can't wait to be in Seattle with you soon! I love you!"
    </div>

    <div style="font-size: 2rem; margin-top: 15px;">🏹 ❤️ ✨</div>
</div>

<audio id="music" loop><source src="https://www.bensound.com/bensound-music/bensound-tomorrow.mp3" type="audio/mpeg"></audio>
<audio id="hit"><source src="https://assets.mixkit.co/active_storage/sfx/2571/2571-preview.mp3" type="audio/mpeg"></audio>

<script>
    let score = 0;
    let gameActive = false;
    const sky = document.getElementById('sky');
    const sun = document.getElementById('sun');
    const scoreEl = document.getElementById('score');
    const bow = document.getElementById('bow');

    function updateCountdown() {
        const tripDate = new Date("February 14, 2026 00:00:00").getTime();
        const now = new Date().getTime();
        const diff = tripDate - now;

        if (diff <= 0) {
            document.getElementById("timerText").innerHTML = "We're going to Seattle! 🏔️";
            return;
        }

        const days = Math.floor(diff / (1000 * 60 * 60 * 24));
        const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
        const mins = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));

        document.getElementById("timerText").innerHTML = `${days}d ${hours}h ${mins}m`;
    }

    function showLetter() {
        document.getElementById('secretLetter').style.display = 'block';
        document.getElementById('letterBtn').style.display = 'none';
        if (navigator.vibrate) navigator.vibrate([100, 50, 100]);
    }

    function startGame() {
        document.getElementById('start-screen').style.display = 'none';
        gameActive = true;
        document.getElementById('music').play();
        spawnLoop();
    }

    function spawnLoop() {
        if (!gameActive) return;
        spawnItem();
        let delay = Math.max(600, 1600 - (score * 100));
        setTimeout(spawnLoop, delay);
    }

    function spawnItem() {
        const item = document.createElement('div');
        item.className = 'target';
        const isSun = Math.random() > 0.4;
        item.innerHTML = isSun ? '☀️' : '☁️';
        item.style.left = (Math.random() * 70 + 15) + '%';
        item.style.top = '-100px';

        const handleShot = (e) => {
            if (!gameActive) return;
            e.preventDefault();
            bow.style.transform = 'translateX(-50%) translateY(20px) scale(0.9)';
            setTimeout(() => bow.style.transform = 'translateX(-50%)', 100);

            if (isSun) {
                score++;
                document.getElementById('hit').currentTime = 0;
                document.getElementById('hit').play();
                updateEnvironment();
            } else {
                score = Math.max(0, score - 1);
                updateEnvironment();
            }
            scoreEl.innerText = score;
            item.remove();
            if (score >= 10) win();
        };

        item.addEventListener('touchstart', handleShot, { passive: false });
        item.addEventListener('mousedown', handleShot);
        document.body.appendChild(item);

        let pos = -100;
        const fall = setInterval(() => {
            if (!gameActive || !item.parentNode) { clearInterval(fall); return; }
            pos += (3 + (score * 0.4));
            item.style.top = pos + 'px';
            if (pos > window.innerHeight) { item.remove(); clearInterval(fall); }
        }, 20);
    }

    function updateEnvironment() {
        const skyColors = ['#020111', '#0d0d26', '#1a1a40', '#2e1f47', '#4d2c5e', '#7a3a63', '#a34d5d', '#d66d52', '#ff9a44', '#ffcf71'];
        sky.style.background = skyColors[score] || '#ffcf71';
        sun.style.bottom = (-150 + (score * 40)) + 'px';
    }

    function win() {
        gameActive = false;
        document.getElementById('hud').style.display = 'none';
        document.getElementById('win-screen').style.display = 'flex';
        bow.style.display = 'none';
        document.querySelectorAll('.target').forEach(t => t.remove());
        updateCountdown();
        setInterval(updateCountdown, 60000);
    }
</script>
</body>
</html>
