<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Birthday & Love!</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Tone.js CDN for music generation -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.49/Tone.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&family=Pacifico&display=swap');

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(to bottom right, #ffdde1, #ee9ca7); /* Soft pink gradient */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            overflow: hidden; /* Prevent scrollbar from animations */
            margin: 0;
            padding: 20px; /* Add some padding for smaller screens */
            box-sizing: border-box;
            position: relative; /* For footer positioning */
        }

        .page-section {
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: absolute;
            top: 0;
            left: 0;
            transition: opacity 1s ease-in-out, transform 1s ease-in-out;
        }

        .birthday-section {
            opacity: 1;
            transform: translateY(0);
            z-index: 20; /* Ensure birthday content is on top initially */
        }

        .love-section {
            opacity: 0;
            transform: translateY(100vh); /* Start off-screen below */
            z-index: 10; /* Lower z-index initially */
            background: linear-gradient(to bottom right, #ffdde1, #f0e68c); /* Soft pink to khaki gradient for love page */
            color: #4a4a4a;
            text-align: center;
            padding: 20px;
            box-sizing: border-box;
        }

        .love-section.active {
            opacity: 1;
            transform: translateY(0);
            z-index: 20; /* Bring to front when active */
        }

        .content-container {
            background-color: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.25);
            text-align: center;
            max-width: 90%;
            width: 650px;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 25px;
            position: relative;
            z-index: 10; /* Ensure container is above confetti/balloons */
        }

        /* Styles specific to the love page content container */
        .love-section .content-container {
            width: 700px; /* Slightly wider for love page */
            background-color: rgba(255, 255, 255, 0.9); /* Slightly more transparent */
        }

        h1 {
            font-family: 'Pacifico', cursive;
            color: #d32f2f; /* Deep red */
            font-size: 4rem;
            text-shadow: 3px 3px 5px rgba(0, 0, 0, 0.15);
            margin-bottom: 15px;
        }

        .love-section h1 {
            font-size: 4.5rem; /* Larger for love page */
        }

        p {
            color: #4a4a4a;
            font-size: 1.3rem;
            line-height: 1.7;
            margin-bottom: 25px;
        }

        .love-section p {
            font-size: 1.4rem;
            line-height: 1.8;
            margin-bottom: 20px;
            max-width: 80%;
        }

        .love-you-text {
            font-family: 'Pacifico', cursive;
            color: #e74c3c; /* Stronger red */
            font-size: 3rem;
            text-shadow: 2px 2px 3px rgba(0, 0, 0, 0.1);
            margin-top: 30px;
        }

        /* Cake Styling */
        .cake {
            position: relative;
            width: 280px;
            height: 220px;
            margin: 40px auto;
            transform-style: preserve-3d;
            animation: bounce 2s infinite alternate ease-in-out; /* Cake bounce animation */
            cursor: pointer; /* Indicate clickable cake */
            perspective: 1000px; /* For 3D effect */
        }

        .cake-layer {
            position: absolute;
            width: 100%;
            left: 0;
            /* Using elliptical border-radius for cylindrical look */
            border-radius: 50% / 15%; /* Adjust vertical radius for less round top */
            background: linear-gradient(to bottom, #a0522d, #8b4513); /* Sienna to chocolate brown gradient */
            box-shadow: inset 0 -8px 15px rgba(0, 0, 0, 0.3), 0 5px 10px rgba(0,0,0,0.1);
            transform-origin: bottom;
            border-bottom: 2px solid rgba(0,0,0,0.1); /* Subtle layer separation */
        }

        .cake-layer.bottom {
            height: 70px;
            top: 150px;
            transform: scaleX(1);
            z-index: 3;
        }

        .cake-layer.middle {
            height: 60px;
            top: 90px;
            transform: scaleX(0.85);
            z-index: 2;
        }

        .cake-layer.top {
            height: 50px;
            top: 40px;
            transform: scaleX(0.7);
            z-index: 1;
        }

        .frosting {
            position: absolute;
            width: 100%;
            height: 25px;
            left: 0;
            /* Using elliptical border-radius for cylindrical look */
            border-radius: 50% / 15%;
            background: linear-gradient(to bottom, #ffffff, #f0f0f0); /* White to light grey gradient */
            box-shadow: inset 0 -5px 8px rgba(0, 0, 0, 0.15), 0 3px 5px rgba(0,0,0,0.05);
            z-index: 4;
            transform-origin: bottom;
            border-top: 3px solid rgba(0,0,0,0.1);
            border-left: 3px solid rgba(0,0,0,0.05);
        }

        .frosting.bottom {
            top: 140px;
            transform: scaleX(1.05);
        }

        .frosting.middle {
            top: 80px;
            transform: scaleX(0.9);
        }

        .frosting.top {
            top: 30px;
            transform: scaleX(0.75);
        }

        .candle {
            position: absolute;
            width: 10px;
            height: 40px;
            background-color: #ffccbc; /* Light orange candle */
            border-radius: 3px;
            z-index: 5;
            transform-origin: bottom;
            box-shadow: inset 0 -3px 5px rgba(0, 0, 0, 0.15);
        }

        .flame {
            position: absolute;
            width: 12px;
            height: 18px;
            background-color: #ffeb3b; /* Yellow flame */
            border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
            box-shadow: 0 0 10px #ffeb3b, 0 0 20px #ff9800, 0 0 25px #f44336;
            animation: flicker 0.8s infinite alternate;
            transform-origin: bottom;
            top: -12px;
            left: -1px;
            cursor: pointer; /* Make flame clickable */
        }

        /* Candle positions */
        .candle-1 { top: 25px; left: 25%; }
        .candle-2 { top: 25px; left: 45%; }
        .candle-3 { top: 25px; left: 65%; }

        /* Animations */
        @keyframes flicker {
            0% { transform: scaleY(1) rotate(-1deg); opacity: 1; }
            50% { transform: scaleY(0.9) rotate(1deg); opacity: 0.9; }
            100% { transform: scaleY(1) rotate(-1deg); opacity: 1; }
        }

        @keyframes bounce {
            0% { transform: translateY(0); }
            100% { transform: translateY(-15px); }
        }

        /* Confetti animation */
        .confetti {
            position: absolute;
            width: 10px;
            height: 10px;
            background-color: #ffcc00; /* Yellow */
            opacity: 0;
            animation: fall 5s infinite;
            border-radius: 50%;
            z-index: 0;
        }

        .confetti:nth-child(2n) { background-color: #ff6699; /* Pink */ }
        .confetti:nth-child(3n) { background-color: #66ccff; /* Blue */ }
        .confetti:nth-child(4n) { background-color: #99ff66; /* Green */ }

        @keyframes fall {
            0% { transform: translateY(-100vh) rotate(0deg); opacity: 1; }
            100% { transform: translateY(100vh) rotate(720deg); opacity: 0; }
        }

        /* Candle blow out animation */
        @keyframes blowOut {
            0% { transform: scale(1); opacity: 1; }
            50% { transform: scale(0.5); opacity: 0.5; }
            100% { transform: scale(0); opacity: 0; }
        }

        .flame.blown-out {
            animation: blowOut 0.5s forwards;
        }

        .smoke {
            position: absolute;
            background-color: rgba(128, 128, 128, 0.5);
            border-radius: 50%;
            opacity: 0;
            animation: puff 1s forwards;
            z-index: 6;
        }

        @keyframes puff {
            0% { transform: scale(0) translateY(0); opacity: 1; }
            100% { transform: scale(1.5) translateY(-30px); opacity: 0; }
        }

        /* Cake explosion animation */
        @keyframes cakeExplode {
            0% { transform: scale(1) rotateY(0deg); opacity: 1; }
            25% { transform: scale(1.1) rotateY(10deg); opacity: 0.8; }
            50% { transform: scale(0.8) rotateY(-10deg); opacity: 0.5; }
            100% { transform: scale(0) rotateY(360deg); opacity: 0; }
        }

        .cake.exploding {
            animation: cakeExplode 1.5s forwards;
            pointer-events: none; /* Disable clicks during explosion */
        }

        /* Balloon Styling */
        .balloon {
            position: absolute;
            width: 80px;
            height: 100px;
            background-color: #ff6699; /* Pink balloon */
            border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
            box-shadow: inset -5px -5px 10px rgba(0,0,0,0.2);
            z-index: 5;
            animation: float 10s infinite ease-in-out alternate;
            opacity: 0.8;
        }

        .balloon::after {
            content: '';
            position: absolute;
            bottom: -15px;
            left: 50%;
            transform: translateX(-50%);
            width: 0;
            height: 0;
            border-left: 8px solid transparent;
            border-right: 8px solid transparent;
            border-top: 15px solid #ff6699;
        }

        .balloon-string {
            position: absolute;
            bottom: -35px;
            left: 50%;
            transform: translateX(-50%);
            width: 2px;
            height: 40px;
            background-color: rgba(0,0,0,0.3);
            z-index: 4;
        }

        .balloon-1 { top: 10%; left: 5%; background-color: #66ccff; }
        .balloon-1 .balloon-string { background-color: rgba(0,0,0,0.3); }
        .balloon-1::after { border-top-color: #66ccff; }

        .balloon-2 { top: 20%; right: 8%; animation-delay: 2s; background-color: #99ff66; }
        .balloon-2 .balloon-string { background-color: rgba(0,0,0,0.3); }
        .balloon-2::after { border-top-color: #99ff66; }

        .balloon-3 { bottom: 15%; left: 15%; animation-delay: 4s; background-color: #ffeb3b; }
        .balloon-3 .balloon-string { background-color: rgba(0,0,0,0.3); }
        .balloon-3::after { border-top-color: #ffeb3b; }

        @keyframes float {
            0% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-30px) rotate(5deg); }
            100% { transform: translateY(0) rotate(0deg); }
        }

        /* Gift Box Styling */
        .gift-box {
            position: absolute;
            width: 100px;
            height: 80px;
            background-color: #e74c3c; /* Red gift */
            border-radius: 5px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            z-index: 5;
        }

        .gift-box::before { /* Ribbon vertical */
            content: '';
            position: absolute;
            width: 20px;
            height: 100%;
            background-color: #f1c40f; /* Yellow ribbon */
            left: 50%;
            transform: translateX(-50%);
        }

        .gift-box::after { /* Ribbon horizontal */
            content: '';
            position: absolute;
            width: 100%;
            height: 20px;
            background-color: #f1c40f; /* Yellow ribbon */
            top: 50%;
            transform: translateY(-50%);
        }

        .gift-box-1 { bottom: 50px; left: 50px; background-color: #8e44ad; } /* Purple gift */
        .gift-box-2 { bottom: 30px; right: 70px; background-color: #27ae60; } /* Green gift */

        /* Footer */
        .footer-text {
            position: absolute;
            bottom: 15px;
            left: 20px;
            color: rgba(0, 0, 0, 0.4);
            font-size: 0.8rem;
            z-index: 10;
        }

        /* Heart Styling (for love section) */
        .heart {
            position: relative;
            width: 150px;
            height: 135px;
            background-color: #e74c3c; /* Red heart */
            transform: rotate(-45deg);
            animation: pulse 1.5s infinite alternate, float-heart 5s infinite ease-in-out;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
            margin-top: 30px;
            z-index: 1; /* Ensure heart is visible */
            opacity: 0; /* Initially hidden for creation animation */
            transform: scale(0) rotate(-45deg); /* Start small for creation */
        }

        .heart::before,
        .heart::after {
            content: '';
            position: absolute;
            width: 150px;
            height: 135px;
            background-color: #e74c3c;
            border-radius: 50%;
        }

        .heart::before {
            top: -75px;
            left: 0;
        }

        .heart::after {
            top: 0;
            left: 75px;
        }

        /* Heart Creation Animation */
        @keyframes createHeart {
            0% { opacity: 0; transform: scale(0) rotate(-45deg); }
            50% { opacity: 1; transform: scale(1.2) rotate(-45deg); }
            100% { opacity: 1; transform: scale(1) rotate(-45deg); }
        }

        .heart.show {
            animation: createHeart 1s ease-out forwards, pulse 1.5s 1s infinite alternate, float-heart 5s 1s infinite ease-in-out;
        }

        /* Small Floating Hearts */
        .small-heart {
            position: absolute;
            background-color: #ff6699; /* Pink heart */
            transform: rotate(-45deg);
            opacity: 0;
            animation: float-small-heart 8s infinite ease-in-out forwards;
            z-index: 5; /* Above background, below main content */
        }

        .small-heart::before,
        .small-heart::after {
            content: '';
            position: absolute;
            background-color: #ff6699;
            border-radius: 50%;
        }

        /* Different colors for small hearts */
        .small-heart:nth-child(2n) { background-color: #66ccff; } /* Blue */
        .small-heart:nth-child(2n)::before, .small-heart:nth-child(2n)::after { background-color: #66ccff; }
        .small-heart:nth-child(3n) { background-color: #99ff66; } /* Green */
        .small-heart:nth-child(3n)::before, .small-heart:nth-child(3n)::after { background-color: #99ff66; }
        .small-heart:nth-child(4n) { background-color: #ffeb3b; } /* Yellow */
        .small-heart:nth-child(4n)::before, .small-heart:nth-child(4n)::after { background-color: #ffeb3b; }


        @keyframes float-small-heart {
            0% { transform: translateY(100vh) rotate(-45deg) scale(0); opacity: 0; }
            20% { opacity: 0.8; transform: translateY(calc(100vh - 200px)) rotate(-45deg) scale(1); }
            100% { transform: translateY(-50vh) rotate(315deg) scale(0.5); opacity: 0; }
        }


        /* Responsive adjustments */
        @media (max-width: 768px) {
            h1 { font-size: 3rem; }
            p { font-size: 1.1rem; }
            .love-section h1 { font-size: 3.5rem; }
            .love-section p { font-size: 1.2rem; }
            .love-you-text { font-size: 2.5rem; }
            .cake { width: 220px; height: 180px; }
            .cake-layer.bottom { height: 60px; top: 120px; }
            .cake-layer.middle { height: 50px; top: 70px; }
            .cake-layer.top { height: 40px; top: 30px; }
            .frosting.bottom { top: 110px; }
            .frosting.middle { top: 60px; }
            .frosting.top { top: 20px; }
            .candle { height: 30px; width: 8px;}
            .flame { width: 10px; height: 15px; top: -10px; }
            .balloon { width: 60px; height: 80px; }
            .balloon::after { bottom: -12px; border-left: 6px solid transparent; border-right: 6px solid transparent; border-top: 12px solid; }
            .balloon-string { height: 30px; }
            .gift-box { width: 80px; height: 60px; }
            .gift-box::before, .gift-box::after { width: 15px; height: 15px; } /* Adjust ribbon size */
            .footer-text { font-size: 0.7rem; bottom: 10px; left: 15px; }
            .heart { width: 120px; height: 108px; }
            .heart::before, .heart::after { width: 120px; height: 108px; }
            .heart::before { top: -60px; }
            .heart::after { left: 60px; }
            .small-heart { width: 25px; height: 22.5px; }
            .small-heart::before, .small-heart::after { width: 25px; height: 22.5px; }
            .small-heart::before { top: -12.5px; }
            .small-heart::after { left: 12.5px; }
        }

        @media (max-width: 480px) {
            .content-container { padding: 20px; gap: 15px; }
            h1 { font-size: 2.5rem; }
            p { font-size: 1rem; }
            .love-section h1 { font-size: 2.8rem; }
            .love-section p { font-size: 1rem; }
            .love-you-text { font-size: 2rem; }
            .cake { width: 180px; height: 150px; }
            .cake-layer.bottom { height: 50px; top: 100px; }
            .cake-layer.middle { height: 40px; top: 60px; }
            .cake-layer.top { height: 30px; top: 25px; }
            .frosting.bottom { top: 90px; }
            .frosting.middle { top: 50px; }
            .frosting.top { top: 15px; }
            .candle { height: 25px; width: 6px;}
            .flame { width: 8px; height: 12px; top: -8px; }
            .balloon { width: 50px; height: 70px; }
            .balloon::after { bottom: -10px; border-left: 5px solid transparent; border-right: 5px solid transparent; border-top: 10px solid; }
            .balloon-string { height: 25px; }
            .gift-box { width: 60px; height: 50px; }
            .gift-box::before, .gift-box::after { width: 12px; height: 12px; } /* Adjust ribbon size */
            .footer-text { font-size: 0.6rem; bottom: 5px; left: 10px; }
            .heart { width: 90px; height: 81px; }
            .heart::before, .heart::after { width: 90px; height: 81px; }
            .heart::before { top: -45px; }
            .heart::after { left: 45px; }
            .small-heart { width: 20px; height: 18px; }
            .small-heart::before, .small-heart::after { width: 20px; height: 18px; }
            .small-heart::before { top: -10px; }
            .small-heart::after { left: 10px; }
        }
    </style>
</head>
<body>
    <div id="birthdaySection" class="page-section birthday-section">
        <div class="content-container">
            <h1>Happy Birthday!</h1>
            <p>Wishing you a day filled with joy, laughter, and all the happiness you deserve. May your year ahead be as sweet as this cake!</p>

            <div class="cake" id="birthdayCake">
                <!-- Bottom Layer -->
                <div class="cake-layer bottom"></div>
                <div class="frosting bottom"></div>

                <!-- Middle Layer -->
                <div class="cake-layer middle"></div>
                <div class="frosting middle"></div>

                <!-- Top Layer -->
                <div class="cake-layer top"></div>
                <div class="frosting top"></div>

                <!-- Candles -->
                <div class="candle candle-1">
                    <div class="flame"></div>
                </div>
                <div class="candle candle-2">
                    <div class="flame"></div>
                </div>
                <div class="candle candle-3">
                    <div class="flame"></div>
                </div>
            </div>
        </div>

        <!-- Balloons -->
        <div class="balloon balloon-1">
            <div class="balloon-string"></div>
        </div>
        <div class="balloon balloon-2">
            <div class="balloon-string"></div>
        </div>
        <div class="balloon balloon-3">
            <div class="balloon-string"></div>
        </div>

        <!-- Gift Boxes -->
        <div class="gift-box gift-box-1"></div>
        <div class="gift-box gift-box-2"></div>

        <div class="footer-text">Created by Hunter on 24 July 2025</div>
    </div>

    <div id="loveSection" class="page-section love-section">
        <div class="content-container">
            <h1>For My Dearest!</h1>
            <p>
                Every moment with you is a precious gift, and today, on this special occasion,
                my heart overflows with gratitude and joy. You bring so much light and happiness
                into my life, more than words can ever express.
            </p>
            <p>
                May your journey ahead be filled with endless adventures, beautiful dreams,
                and all the love you truly deserve. Thank you for being you.
            </p>
            <div class="heart" id="animatedHeart"></div>
            <div class="love-you-text">I love you forever!</div>
        </div>
    </div>

    <script>
        // JavaScript for confetti animation
        function createConfetti() {
            const confettiCount = 50;
            const body = document.body;

            for (let i = 0; i < confettiCount; i++) {
                const confetti = document.createElement('div');
                confetti.classList.add('confetti');
                confetti.style.left = Math.random() * 100 + 'vw';
                confetti.style.animationDelay = Math.random() * 5 + 's';
                confetti.style.animationDuration = (Math.random() * 3 + 4) + 's'; // Randomize duration
                confetti.style.transform = `rotate(${Math.random() * 360}deg)`; // Initial random rotation
                body.appendChild(confetti);
            }
        }

        // Function to create small floating hearts
        function createSmallHearts() {
            const smallHeartCount = 30; // Number of small hearts
            const loveSectionElement = document.getElementById('loveSection');

            for (let i = 0; i < smallHeartCount; i++) {
                const heart = document.createElement('div');
                heart.classList.add('small-heart');
                
                // Randomize size
                const size = Math.random() * (35 - 15) + 15; // Between 15px and 35px
                heart.style.width = size + 'px';
                heart.style.height = (size * 0.9) + 'px'; // Maintain aspect ratio for heart shape
                heart.style.left = Math.random() * 100 + 'vw';
                heart.style.animationDelay = Math.random() * 6 + 's'; // Stagger animation
                heart.style.animationDuration = (Math.random() * 5 + 8) + 's'; // Randomize duration

                // Position the pseudo-elements for the heart shape
                heart.style.setProperty('--before-top', `-${size / 2}px`);
                heart.style.setProperty('--after-left', `${size / 2}px`);
                heart.style.setProperty('--pseudo-width', `${size}px`);
                heart.style.setProperty('--pseudo-height', `${size * 0.9}px`);

                // Apply styles to pseudo-elements (needs to be done via JS for dynamic sizing)
                // This is a common limitation; for dynamic sizes, it's often easier to create two divs
                // or use CSS variables and set them on the parent. Let's use CSS variables.
                heart.style.setProperty('--heart-color', getRandomHeartColor());

                loveSectionElement.appendChild(heart);

                // Dynamically set pseudo-element styles for the heart shape
                const styleSheet = document.styleSheets[0];
                const ruleIndex = styleSheet.insertRule(`
                    .small-heart[data-index="${i}"]::before,
                    .small-heart[data-index="${i}"]::after {
                        background-color: var(--heart-color);
                        width: ${size}px;
                        height: ${size * 0.9}px;
                    }
                    .small-heart[data-index="${i}"]::before {
                        top: -${size / 2}px;
                        left: 0;
                    }
                    .small-heart[data-index="${i}"]::after {
                        top: 0;
                        left: ${size / 2}px;
                    }
                `, styleSheet.cssRules.length);
                heart.setAttribute('data-index', i); // Use data-index to target specific rules

            }
        }

        function getRandomHeartColor() {
            const colors = ['#ff6699', '#66ccff', '#99ff66', '#ffeb3b', '#e74c3c', '#8e44ad'];
            return colors[Math.floor(Math.random() * colors.length)];
        }


        let blownOutCandlesCount = 0;
        const totalCandles = document.querySelectorAll('.flame').length;
        const birthdayCake = document.getElementById('birthdayCake');
        const birthdaySection = document.getElementById('birthdaySection');
        const loveSection = document.getElementById('loveSection');
        const animatedHeart = document.getElementById('animatedHeart');

        // Candle blow out animation when flame is clicked
        document.querySelectorAll('.flame').forEach(flame => {
            flame.addEventListener('click', () => {
                if (!flame.classList.contains('blown-out')) {
                    flame.classList.add('blown-out');
                    blownOutCandlesCount++;

                    // Create smoke effect
                    const smoke = document.createElement('div');
                    smoke.classList.add('smoke');
                    // Position smoke relative to the flame's parent candle
                    const candle = flame.closest('.candle');
                    const candleRect = candle.getBoundingClientRect();
                    const cakeRect = birthdayCake.getBoundingClientRect();

                    // Calculate position relative to the cake container
                    smoke.style.left = (candleRect.left - cakeRect.left + candleRect.width / 2 - 15) + 'px'; // Center smoke
                    smoke.style.top = (candleRect.top - cakeRect.top - 10) + 'px'; // Above flame
                    smoke.style.width = '30px';
                    smoke.style.height = '30px';
                    birthdayCake.appendChild(smoke);

                    // Remove smoke after animation
                    smoke.addEventListener('animationend', () => {
                        smoke.remove();
                    });
                }
            });
        });

        // Cake click event for explosion and section transition
        birthdayCake.addEventListener('click', () => {
            if (blownOutCandlesCount === totalCandles && !birthdayCake.classList.contains('exploding')) {
                birthdayCake.classList.add('exploding');
                
                // Hide birthday section and show love section after explosion animation
                setTimeout(() => {
                    birthdaySection.style.opacity = '0';
                    birthdaySection.style.transform = 'translateY(-100vh)'; // Slide up and out
                    
                    // After birthday section is hidden, reveal love section
                    setTimeout(() => {
                        birthdaySection.style.display = 'none'; // Completely hide after transition
                        loveSection.classList.add('active'); // Activate love section
                        animatedHeart.classList.add('show'); // Trigger heart creation animation
                        createSmallHearts(); // Create and animate small hearts
                    }, 1000); // Wait for birthday section to slide out
                    
                }, 1500); // Match this duration to the cakeExplode animation duration
            }
        });

        // Happy Birthday Song using Tone.js - plays automatically
        let synth;
        let melody;

        async function initializeAndPlaySong() {
            // Check if Tone.js context is already running (e.g., from a previous page load in Canvas)
            if (Tone.context.state !== 'running') {
                await Tone.start();
            }
            
            synth = new Tone.Synth().toDestination();

            // Notes for "Happy Birthday" song
            const notes = [
                ['C4', '8n'], ['C4', '8n'], ['D4', '4n'], ['C4', '4n'], ['F4', '4n'], ['E4', '2n'],
                ['C4', '8n'], ['C4', '8n'], ['D4', '4n'], ['C4', '4n'], ['G4', '4n'], ['F4', '2n'],
                ['C4', '8n'], ['C4', '8n'], ['C5', '4n'], ['A4', '4n'], ['F4', '4n'], ['E4', '8n'], ['D4', '2n'],
                ['Bb4', '8n'], ['Bb4', '8n'], ['A4', '4n'], ['F4', '4n'], ['G4', '4n'], ['F4', '2n']
            ];

            melody = new Tone.Sequence((time, note) => {
                synth.triggerAttackRelease(note[0], note[1], time);
            }, notes, '4n');

            melody.loop = true; // Loop the song
            melody.start();
            Tone.Transport.start();
        }

        // Call all initialization functions when the page loads
        window.onload = () => {
            createConfetti();
            initializeAndPlaySong();
        };
    </script>
</body>
</html>
