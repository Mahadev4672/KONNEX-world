# KONNEX-world
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Konnex World | Advanced Robotic Quiz</title>
    <style>
        :root {
            --primary: #00d2ff;
            --secondary: #3a7bd5;
            --correct: #2ecc71;
            --wrong: #e74c3c;
            --dark-glass: rgba(15, 23, 42, 0.85);
        }

        body {
            font-family: 'Segoe UI', Roboto, sans-serif;
            color: #f8fafc;
            margin: 0;
            padding: 20px;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            /* Robotic Background Setup */
            background-color: #0f172a;
            background-image: url('https://i.imgur.com/86zV6rL.jpg'); /* Placeholder for your robot background */
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            position: relative;
        }

        /* The Faded Overlay Layer */
        body::before {
            content: "";
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: radial-gradient(circle, rgba(15, 23, 42, 0.7) 0%, rgba(15, 23, 42, 0.95) 100%);
            z-index: -1;
        }

        .container {
            max-width: 850px;
            width: 100%;
            background: var(--dark-glass);
            backdrop-filter: blur(10px);
            padding: 40px;
            border-radius: 20px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 20px 50px rgba(0,0,0,0.5);
        }

        h1 {
            text-align: center;
            text-transform: uppercase;
            letter-spacing: 3px;
            color: var(--primary);
            margin-bottom: 10px;
            text-shadow: 0 0 15px rgba(0, 210, 255, 0.5);
        }

        .subtitle {
            text-align: center;
            color: #94a3b8;
            margin-bottom: 40px;
        }

        .question-card {
            background: rgba(255, 255, 255, 0.03);
            margin-bottom: 30px;
            padding: 25px;
            border-radius: 12px;
            border-left: 5px solid var(--secondary);
            transition: all 0.3s ease;
        }

        .question-text {
            font-size: 1.2rem;
            font-weight: 500;
            margin-bottom: 20px;
            display: block;
        }

        .options-grid {
            display: grid;
            gap: 12px;
        }

        .option-label {
            display: flex;
            align-items: center;
            padding: 14px;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            cursor: pointer;
            transition: 0.2s;
        }

        .option-label:hover {
            background: rgba(255, 255, 255, 0.1);
            border-color: var(--primary);
        }

        input[type="radio"] {
            margin-right: 15px;
            accent-color: var(--primary);
        }

        .btn-submit {
            display: block;
            width: 100%;
            padding: 20px;
            background: linear-gradient(90deg, var(--secondary), var(--primary));
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            text-transform: uppercase;
            transition: 0.3s;
            box-shadow: 0 10px 20px rgba(0,0,0,0.3);
        }

        .btn-submit:hover {
            transform: translateY(-2px);
            box-shadow: 0 15px 30px rgba(0, 210, 255, 0.4);
        }

        /* Result Styles */
        .answer-key {
            margin-top: 15px;
            padding: 10px;
            border-radius: 5px;
            background: rgba(0, 210, 255, 0.1);
            color: var(--primary);
            font-weight: bold;
            display: none; /* Hidden until finished */
        }

        #final-score-box {
            display: none;
            text-align: center;
            padding: 30px;
            margin-top: 30px;
            border-radius: 15px;
            font-size: 1.5rem;
            animation: fadeIn 1s;
        }

        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    </style>
</head>
<body>

<div class="container">
    <h1>Konnex World Interface</h1>
    <p class="subtitle">AI & Robotics Autonomous Systems Quiz | 40 Modules</p>
    
    <div id="quiz-anchor"></div>

    <button class="btn-submit" id="submit-btn" onclick="processResults()">Initiate Scoring Protocol</button>

    <div id="final-score-box"></div>
</div>

<script>
const questions = [
    { q: "What is the first main phase after Ingest in Konnex roadmap?", opts: ["Runtime Zero (R0)", "Mainnet launch", "Token airdrop", "Hardware testing"], ans: 0 },
    { q: "Innovation: Konnex stops bad validators by doing what?", opts: ["Giving warnings only", "Slashing their staked collateral", "Ignoring failures", "Banning them manually"], ans: 1 },
    { q: "Benefit: Konnex makes robot deployment feel like what?", opts: ["Buying expensive machines", "Very slow & complicated", "Installing software apps", "Building everything from scratch"], ans: 2 },
    { q: "Future: When robots hire each other on Konnex, what happens?", opts: ["Chaos and confusion", "They need a central boss", "They create self-organizing fleets", "Work stops completely"], ans: 2 },
    { q: "How can you get Creator or Early Supporter roles?", opts: ["By buying tokens early", "Being active with content & daily tasks", "Only if you are a developer", "No roles are available"], ans: 1 },
    { q: "Innovation: What is the main job of miners/validators?", opts: ["Build physical robots", "Run AI models & compete on performance", "Post on social media", "Mine crypto coins only"], ans: 1 },
    { q: "Benefit of on-chain PoPW?", opts: ["Makes robot work secret", "Slows down payments", "Turns real actions into verifiable receipts", "Requires human confirmation every time"], ans: 2 },
    { q: "What makes Konnex permissionless?", opts: ["Needs special approval", "Closed to small users", "Anyone can join & submit models/tasks", "Only big companies allowed"], ans: 2 },
    { q: "What is the goal of Preflight phase?", opts: ["Token launch", "3D simulations for multi-robot testing", "Collect only human feedback", "Sell robot parts"], ans: 1 },
    { q: "Innovation: Who can become a validator?", opts: ["Only large corporations", "Anyone who stakes & runs good models", "No one is allowed", "Only humans without AI"], ans: 1 },
    { q: "Benefit: How does Konnex reward developers?", opts: ["No rewards at all", "Pay to use the platform", "Royalties if their AI performs best", "Limit to testing only"], ans: 2 },
    { q: "Future vision: Robots on Konnex will mostly be?", opts: ["Fully manual", "Virtual simulations only", "Autonomous with trustless contracts", "One robot per job forever"], ans: 2 },
    { q: "What is the current main task for earning points?", opts: ["Posting memes", "Watching videos", "Daily RLHF feedback", "Buying tokens"], ans: 2 },
    { q: "Innovation: What unlocks automatic payouts?", opts: ["Manual invoice", "PoPW receipt + smart contract", "Human manager approval", "No payouts exist"], ans: 1 },
    { q: "Benefit for logistics industry?", opts: ["More delays", "Higher labor costs", "Scalable drone/rover fleets on-chain", "Less efficiency"], ans: 2 },
    { q: "What happens if a model performs poorly?", opts: ["Gets deleted immediately", "Lower KPI score & less royalties", "Automatic win", "No effect at all"], ans: 1 },
    { q: "What is Ingest Phase mainly about?", opts: ["Launching $KNX", "Selling hardware", "Collecting RLHF feedback", "Marketing campaigns"], ans: 2 },
    { q: "Innovation: Konnex turns physical robot work into?", opts: ["Free service", "Secret company data", "Verifiable on-chain asset", "Paper contract only"], ans: 2 },
    { q: "Benefit: Why is Konnex good for small companies?", opts: ["Only big firms win", "Very high fees", "Easy & cheap robot labor access", "Slow deployment"], ans: 2 },
    { q: "Future: What will robots do on Konnex?", opts: ["Work alone forever", "Only clean houses", "Hire each other & form teams", "Stay offline"], ans: 2 },
    { q: "How do points (KP) help in testnet?", opts: ["Buy robots now", "Trade for cash today", "Position for $KNX airdrop", "Nothing at all"], ans: 2 },
    { q: "Innovation: Universal Task Language uses what format?", opts: ["Secret codes", "Only English", "JSON-based grammar", "Images only"], ans: 2 },
    { q: "Benefit of high reputation?", opts: ["Fewer tasks", "Account gets blocked", "More jobs & lower collateral", "No real impact"], ans: 2 },
    { q: "What comes right after Preflight?", opts: ["Ingest restart", "Full mainnet only", "On-chain rollout & TGE", "Back to off-chain"], ans: 2 },
    { q: "What does RLHF stand for in Konnex?", opts: ["Robot Learning Hardware Function", "Random Learning Human Feedback", "Reinforcement Learning from Human Feedback", "Real Labor Human Force"], ans: 2 },
    { q: "Innovation: Konnex lets robots license what?", opts: ["Only old models", "No sharing allowed", "AI skills & intelligence on-chain", "Physical parts only"], ans: 2 },
    { q: "Benefit: Konnex is called 'robotics-as-a-service' because?", opts: ["You buy robots forever", "Very expensive service", "Deploy labor like installing apps", "Manual control required"], ans: 2 },
    { q: "Future: Konnex builds what kind of market?", opts: ["Human job board", "Virtual goods only", "Permissionless physical robot work", "No market at all"], ans: 2 },
    { q: "What verifies physical robot tasks on Konnex?", opts: ["Human photos only", "Proof-of-Physical-Work (PoPW) using sensors & logs", "AI self-report", "No verification needed"], ans: 1 },
    { q: "Innovation: Konnex uses stablecoins for what reason?", opts: ["Slow & traditional payments", "Fast, transparent USD-equivalent robot job payouts", "Only for gaming", "No real use"], ans: 1 },
    { q: "Benefit: Konnex unlocks value in what economy?", opts: ["Digital art NFTs", "$25 trillion physical labor market", "Stock trading only", "Virtual worlds"], ans: 1 },
    { q: "Future: Coordinated robot fleets on Konnex will?", opts: ["Stay isolated", "Need constant human input", "Self-organize & compound capabilities", "Work only in simulations"], ans: 2 },
    { q: "What is the native token after TGE?", opts: ["$ETH", "$BTC", "$KNX for governance & fees", "No token"], ans: 2 },
    { q: "Innovation: Smart contracts in Konnex auto-execute what?", opts: ["Manual tasks", "Job contracts & payments after PoPW", "Social posts", "Nothing"], ans: 1 },
    { q: "Benefit for AI models?", opts: ["Stay separate forever", "Trade intelligence & grow reputation", "Fixed low pay", "No improvement"], ans: 1 },
    { q: "What phase tests full runtime constraints?", opts: ["Ingest", "Preflight", "Runtime Zero (R0)", "TGE"], ans: 2 },
    { q: "How do users earn Konnex Points mainly?", opts: ["By paying money", "Daily RLHF tasks & quests", "Watching ads", "Posting memes only"], ans: 1 },
    { q: "Innovation: Reputation scores help validators by?", opts: ["Making it harder", "Lowering collateral needed", "Blocking tasks", "Decreasing earnings"], ans: 1 },
    { q: "Benefit of on-chain settlements?", opts: ["More delays", "Transparent & fast stablecoin payouts", "Higher fees only", "Less trust"], ans: 1 },
    { q: "Future GDP goal of Konnex?", opts: ["Virtual crypto only", "Human manual jobs", "Autonomous physical labor economy", "Gaming metaverse"], ans: 2 }
];

function buildQuiz() {
    const anchor = document.getElementById('quiz-anchor');
    let content = "";
    
    questions.forEach((item, index) => {
        content += `
            <div class="question-card" id="card-${index}">
                <span class="question-text">MODULE ${index + 1}: ${item.q}</span>
                <div class="options-grid">
                    ${item.opts.map((opt, i) => `
                        <label class="option-label">
                            <input type="radio" name="q${index}" value="${i}">
                            ${opt}
                        </label>
                    `).join('')}
                </div>
                <div id="key-${index}" class="answer-key">✅ DATA RECOVERED: ${item.opts[item.ans]}</div>
            </div>
        `;
    });
    anchor.innerHTML = content;
}

function processResults() {
    let score = 0;
    questions.forEach((item, index) => {
        const card = document.getElementById(`card-${index}`);
        const keyReveal = document.getElementById(`key-${index}`);
        const selected = document.querySelector(`input[name="q${index}"]:checked`);
        
        keyReveal.style.display = "block"; // Reveal the answer key

        if (selected && parseInt(selected.value) === item.ans) {
            score++;
            card.style.borderLeftColor = "#2ecc71";
            card.style.background = "rgba(46, 204, 113, 0.1)";
        } else {
            card.style.borderLeftColor = "#e74c3c";
            card.style.background = "rgba(231, 76, 60, 0.1)";
        }
    });

    const scoreBox = document.getElementById('final-score-box');
    scoreBox.style.display = "block";
    scoreBox.style.background = score > 30 ? "rgba(46, 204, 113, 0.2)" : "rgba(231, 76, 60, 0.2)";
    scoreBox.innerHTML = `SYSTEM ANALYSIS COMPLETE<br>Score: ${score} / ${questions.length} Modules Validated`;
    
    document.getElementById('submit-btn').innerText = "Protocol Completed";
    window.scrollTo({ top: document.body.scrollHeight, behavior: 'smooth' });
}

buildQuiz();
</script>

</body>
</html>
