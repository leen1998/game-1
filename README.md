<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Case of the Misleading Metrics</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Merriweather:wght@400;700&family=Inter:wght@400;500;700&display=swap');

        :root {
            --primary-color: #2c3e50;
            --secondary-color: #f39c12;
            --accent-color: #e74c3c;
            --background-color: #ecf0f1;
            --card-background: #34495e;
            --text-color: #ecf0f1;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--background-color);
            color: var(--primary-color);
            background-image: url('data:image/svg+xml,%3Csvg width="60" height="60" viewBox="0 0 60 60" xmlns="http://www.w3.org/2000/svg"%3E%3Cg fill="none" fill-rule="evenodd"%3E%3Cg fill="%239C92AC" fill-opacity="0.12"%3E%3Cpath d="M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0 18v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zM12 34v-4h-2v4H6v2h4v4h2v-4h4v-2h-4zm0 18v-4h-2v4H6v2h4v4h2v-4h4v-2h-4zM24 0v4h2v-4h4v-2h-4V0h-2zM0 0v4h2v-4h4v-2H2V0H0zm48 0v4h2v-4h4v-2h-4V0h-2zM24 12v4h2v-4h4v-2h-4v-4h-2v4H24zM0 12v4h2v-4h4v-2H2v-4H0v4zm48 12v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm-48 0v-4h-2v4H6v2h4v4h2v-4h4v-2h-4zm0-12v4h2v-4h4v-2H2v-4H0v4zm24 12v4h2v-4h4v-2h-4v-4h-2v4H24zM0 36v4h2v-4h4v-2H2v-4H0v4zm48 12v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm24 0v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm24-24v4h2v-4h4v-2h-4V0h-2zm0 36v4h2v-4h4v-2h-4v-4h-2v4zM12 0v4h2v-4h4v-2h-4V0h-2zm-12 0v4h2v-4h4v-2H2V0H0zm24 12v4h2v-4h4v-2h-4v-4h-2v4H24zM0 12v4h2v-4h4v-2H2v-4H0v4z"%3E%3C/path%3E%3C/g%3E%3C/g%3E%3C/svg%3E');
        }

        h1,
        h2,
        h3 {
            font-family: 'Merriweather', serif;
        }

        .card {
            background-color: var(--card-background);
            color: var(--text-color);
            padding: 1.5rem;
            border-radius: 1rem;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .btn {
            background-color: var(--secondary-color);
            color: var(--primary-color);
            font-weight: bold;
            padding: 0.75rem 1.5rem;
            border-radius: 0.75rem;
            transition: all 0.2s;
            cursor: pointer;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(243, 156, 18, 0.4);
        }

        .drag-item {
            cursor: grab;
            transition: transform 0.2s;
        }

        .drag-item:active {
            cursor: grabbing;
        }

        .drop-zone {
            border: 2px dashed #95a5a6;
            transition: border-color 0.2s, background-color 0.2s;
        }

        .drop-zone.drag-over {
            border-color: var(--secondary-color);
            background-color: rgba(243, 156, 18, 0.1);
        }

        .fade-in {
            animation: fadeIn 0.5s ease-in-out;
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(10px);
            }

            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* Modal styling */
        .modal {
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 100;
        }

        .modal-content {
            background-color: var(--card-background);
        }

        .list-item-title {
            font-weight: bold;
            color: var(--secondary-color);
        }
    </style>
</head>

<body class="min-h-screen flex flex-col items-center justify-center p-4 text-center">

    <!-- Modal for Alerts -->
    <div id="modal" class="modal fixed inset-0 flex items-center justify-center hidden">
        <div class="modal-content card max-w-sm w-full p-6 text-center shadow-lg">
            <h3 id="modal-title" class="text-xl font-bold mb-4"></h3>
            <p id="modal-message" class="mb-6"></p>
            <button id="modal-close-btn" class="btn text-sm">Close</button>
        </div>
    </div>

    <!-- Main Game Container -->
    <main class="w-full max-w-4xl card p-8 fade-in">
        <h1 class="text-4xl font-bold mb-4 text-secondary-color">The Case of the Misleading Metrics</h1>
        
        <div id="user-info" class="text-sm font-mono mb-4">
            <p id="user-id" class="text-gray-400"></p>
        </div>

        <!-- Sherlock Holmes Dialogue Area -->
        <div id="sherlock-dialogue" class="card mb-8 text-left bg-gray-800 p-6 rounded-lg shadow-inner">
            <p id="dialogue-text" class="text-lg leading-relaxed text-gray-200">
                Welcome, Detective. Your mission is to investigate two CE cases. Your task: determine which case
                demonstrates effective, ACCME-compliant storytelling, and which fails. Be cautious — appearances can
                deceive.
            </p>
        </div>

        <!-- Game Stages -->
        <div id="game-stage-container">
            <!-- Stage 1: Name Entry -->
            <div id="stage-1" class="stage">
                <p class="text-xl mb-4 text-secondary-color">Enter your name, Detective.</p>
                <div class="flex flex-col items-center gap-4">
                    <input type="text" id="detective-name" placeholder="Your Name"
                        class="p-2 w-full max-w-sm rounded-md bg-gray-700 text-white placeholder-gray-400">
                    <button id="start-btn" class="btn">Begin Investigation</button>
                </div>
            </div>

            <!-- Stage 2: Directions -->
            <div id="stage-2" class="stage hidden">
                <div class="text-left leading-relaxed space-y-4">
                    <p class="text-xl text-secondary-color">The Game is afoot!</p>
                    <p><strong>Objective:</strong> Compare two CE storytelling cases — one effective, one poor.</p>
                    <p><strong>Tools Available:</strong></p>
                    <ul class="list-disc list-inside space-y-2">
                        <li><strong>Case Files</strong> (narrative evidence with numbers).</li>
                        <li><strong>Dashboards</strong> (outcomes data, charts).</li>
                        <li><strong>NPC Interrogations</strong> (program managers, clinicians, analysts).</li>
                        <li><strong>Deduction Board</strong> (organize findings into Insight → Action → Outcome → Next Step).</li>
                    </ul>
                    <p><strong>Final Task:</strong> Analyze both cases and present deductions about what worked, what failed, and how storytelling can transform CE data into actionable insights for evidence-based decision-making.</p>
                </div>
                <button id="continue-to-cases-btn" class="btn mt-8">Examine Case Files</button>
            </div>

            <!-- Stage 3: Case Files -->
            <div id="stage-3" class="stage hidden">
                <div class="grid md:grid-cols-2 gap-8">
                    <!-- Case 1 -->
                    <div class="case-file card text-left">
                        <h2 class="text-2xl font-bold mb-4 text-secondary-color">Case 1: The Brilliant Story</h2>
                        <ul class="text-sm space-y-2 text-gray-300">
                            <li><span class="list-item-title">The Challenge:</span> Oncologists lacked confidence in NSCLC second-line therapy. Only 42% reported readiness. Chart audits confirmed the gap: 54% of prescriptions followed guidelines.</li>
                            <li><span class="list-item-title">The Who:</span> Learners: Oncologists in academic and community practices. Stakeholders: Independent CE planners, NCCN experts, multidisciplinary faculty.</li>
                            <li><span class="list-item-title">The Action:</span> A bias-free CE program was built: Case-based modules with patient vignettes, faculty-led guideline discussions, reflective polling with feedback. All disclosures resolved under high-quality educational standards.</li>
                            <li><span class="list-item-title">The Results:</span> Competence: Post-test scores improved 32%. Confidence: 74% reported being better prepared. Performance: 6-month EHR audit showed 22% more guideline-concordant prescribing. Healthcare Impact: Greater alignment with quality metrics.</li>
                            <li><span class="list-item-title">The Next Step:</span> Expand to community oncology sites. Add patient-reported outcomes. Continue bridging confidence to care.</li>
                        </ul>
                    </div>
                    <!-- Case 2 -->
                    <div class="case-file card text-left">
                        <h2 class="text-2xl font-bold mb-4 text-accent-color">Case 2: The Broken Story</h2>
                        <ul class="text-sm space-y-2 text-gray-300">
                            <li><span class="list-item-title">The Challenge:</span> A symposium promised updates on NSCLC treatment, chosen because “the topic is hot.” No needs assessment. No data.</li>
                            <li><span class="list-item-title">The Who:</span> Learners: General oncology audience at a national conference. Stakeholders: Event organizers + single pharmaceutical sponsor (who picked the faculty).</li>
                            <li><span class="list-item-title">The Action:</span> Sponsor-led design: One-hour didactic lecture, company-affiliated speaker, focused mainly on a single drug, no interactivity. Sponsor controlled content → violating high-quality educational standards.</li>
                            <li><span class="list-item-title">The Results:</span> Satisfaction: 92% rated the speaker highly. Gaps: No pre/post test. No competence or performance outcomes. Claim: Organizers assumed prescribing improved, with no evidence.</li>
                            <li><span class="list-item-title">The Next Step:</span> Repeat the same lecture next year. Same sponsor. Same content. Same failures.</li>
                        </ul>
                    </div>
                </div>
                <button id="continue-to-deduction-btn" class="btn mt-8">Proceed to Deduction Board</button>
            </div>

            <!-- Stage 4: Deduction Board -->
            <div id="stage-4" class="stage hidden">
                <p class="text-lg mb-4 text-secondary-color">The Deduction Board</p>
                <p class="text-sm mb-6 text-gray-300">Drag each piece of evidence to its correct framework category.</p>

                <!-- Evidence and Deduction Board -->
                <div class="flex flex-col md:flex-row gap-8 items-center md:items-start">
                    <!-- Evidence Pool -->
                    <div id="evidence-pool" class="w-full md:w-1/2 p-4 card">
                        <h3 class="text-xl mb-4 font-bold">Evidence to Examine</h3>
                        <!-- Draggable Evidence Items -->
                        <div class="space-y-2">
                            <div id="evidence-1" class="drag-item p-3 bg-gray-700 rounded-lg shadow-md"
                                draggable="true" data-group="case1" data-framework="Insight">Oncologists lacked confidence (42% readiness)</div>
                            <div id="evidence-2" class="drag-item p-3 bg-gray-700 rounded-lg shadow-md"
                                draggable="true" data-group="case1" data-framework="Insight">Chart audits confirm prescription gap (54% follow guidelines)</div>
                            <div id="evidence-3" class="drag-item p-3 bg-gray-700 rounded-lg shadow-md"
                                draggable="true" data-group="case1" data-framework="Action">Bias-free CE program with case-based modules</div>
                            <div id="evidence-4" class="drag-item p-3 bg-gray-700 rounded-lg shadow-md"
                                draggable="true" data-group="case1" data-framework="Action">Faculty-led guideline discussions & polling</div>
                            <div id="evidence-5" class="drag-item p-3 bg-gray-700 rounded-lg shadow-md"
                                draggable="true" data-group="case1" data-framework="Outcome">Post-test scores improved by 32%</div>
                            <div id="evidence-6" class="drag-item p-3 bg-gray-700 rounded-lg shadow-md"
                                draggable="true" data-group="case1" data-framework="Outcome">EHR audit shows 22% more guideline-concordant prescribing</div>
                            <div id="evidence-7" class="drag-item p-3 bg-gray-700 rounded-lg shadow-md"
                                draggable="true" data-group="case1" data-framework="Next Step">Decision to expand to new sites</div>
                            <div id="evidence-8" class="drag-item p-3 bg-gray-700 rounded-lg shadow-md"
                                draggable="true" data-group="case2" data-framework="Weak">Symposium chosen because "topic is hot"</div>
                            <div id="evidence-9" class="drag-item p-3 bg-gray-700 rounded-lg shadow-md"
                                draggable="true" data-group="case2" data-framework="Weak">92% rated the speaker highly (satisfaction score)</div>
                            <div id="evidence-10" class="drag-item p-3 bg-gray-700 rounded-lg shadow-md"
                                draggable="true" data-group="case2" data-framework="Weak">Sponsor-controlled content & faculty</div>
                            <div id="evidence-11" class="drag-item p-3 bg-gray-700 rounded-lg shadow-md"
                                draggable="true" data-group="case2" data-framework="Weak">Organizers assumed prescribing improved</div>
                        </div>
                    </div>

                    <!-- Framework Drop Zones -->
                    <div id="deduction-board" class="w-full md:w-1/2 p-4 card">
                        <h3 class="text-xl mb-4 font-bold">The Framework</h3>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div id="drop-Insight" class="drop-zone p-4 bg-gray-700 rounded-lg shadow-inner min-h-[100px]">
                                <h4 class="text-sm font-bold text-secondary-color">Insight</h4>
                            </div>
                            <div id="drop-Action" class="drop-zone p-4 bg-gray-700 rounded-lg shadow-inner min-h-[100px]">
                                <h4 class="text-sm font-bold text-secondary-color">Action</h4>
                            </div>
                            <div id="drop-Outcome" class="drop-zone p-4 bg-gray-700 rounded-lg shadow-inner min-h-[100px]">
                                <h4 class="text-sm font-bold text-secondary-color">Outcome</h4>
                            </div>
                            <div id="drop-Next Step" class="drop-zone p-4 bg-gray-700 rounded-lg shadow-inner min-h-[100px]">
                                <h4 class="text-sm font-bold text-secondary-color">Next Step</h4>
                            </div>
                            <div id="drop-Weak" class="drop-zone p-4 bg-gray-700 rounded-lg shadow-inner col-span-2 min-h-[100px]">
                                <h4 class="text-sm font-bold text-accent-color">Weak/Misleading Evidence</h4>
                            </div>
                        </div>
                    </div>
                </div>

                <div id="hints-area" class="mt-4 p-4 card text-sm text-left hidden">
                    <p id="hint-text" class="text-red-400"></p>
                </div>
            </div>

            <!-- Stage 5: Debrief -->
            <div id="stage-5" class="stage hidden">
                <p id="debrief-text" class="text-lg leading-relaxed text-left mb-6"></p>
                <div class="card p-6 rounded-lg bg-gray-800 shadow-inner">
                    <h3 class="text-xl font-bold mb-2 text-secondary-color">Your Final Analysis</h3>
                    <div class="text-sm text-left text-gray-300 space-y-2">
                        <p><strong>What made the good case compelling?</strong></p>
                        <p><strong>What caused the poor case to fail?</strong></p>
                        <p><strong>How can Insight → Action → Outcome → Next Step transform CE storytelling?</strong></p>
                    </div>
                </div>
                <button id="show-scoreboard-btn" class="btn mt-8">See Your Detective Score</button>
            </div>

            <!-- Stage 6: Scoreboard -->
            <div id="stage-6" class="stage hidden">
                <p id="scoreboard-intro" class="text-xl italic mb-6"></p>
                <div class="w-full max-w-lg mx-auto card p-6">
                    <h3 class="text-2xl font-bold mb-4 text-secondary-color">Detective Scoreboard</h3>
                    <div id="scoreboard-list" class="space-y-4">
                        <!-- Scoreboard list will be populated here by JS -->
                        <div class="flex items-center justify-between p-3 rounded-md bg-gray-700">
                            <span class="font-bold">1. 🏆 Detective [Name]</span>
                            <span class="text-sm">Score: 100</span>
                        </div>
                        <div class="flex items-center justify-between p-3 rounded-md bg-gray-700">
                            <span class="font-bold">2. 🥈 Detective [Name]</span>
                            <span class="text-sm">Score: 90</span>
                        </div>
                        <div class="flex items-center justify-between p-3 rounded-md bg-gray-700">
                            <span class="font-bold">3. 🥉 Detective [Name]</span>
                            <span class="text-sm">Score: 80</span>
                        </div>
                    </div>
                </div>
                <div class="mt-8">
                    <h3 class="text-xl font-bold mb-4 text-secondary-color">Key Takeaways</h3>
                    <ul class="text-sm text-left space-y-2 text-gray-300">
                        <li>Numbers must be interpreted critically, not accepted at face value.</li>
                        <li><strong>Case 1</strong> demonstrates a rigorous, ethical storytelling approach.</li>
                        <li><strong>Case 2</strong> highlights the risks of poor storytelling and bias.</li>
                        <li>Collaboration, critical reasoning, and ethical alignment elevate CE practice.</li>
                    </ul>
                </div>
            </div>
        </div>
    </main>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, collection, query } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Firebase Setup
        let app;
        let db;
        let auth;
        let userId;

        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;


        const main = document.querySelector('main');
        const modal = document.getElementById('modal');
        const modalTitle = document.getElementById('modal-title');
        const modalMessage = document.getElementById('modal-message');
        const modalCloseBtn = document.getElementById('modal-close-btn');

        // Custom alert function
        function showModal(title, message) {
            modalTitle.textContent = title;
            modalMessage.textContent = message;
            modal.classList.remove('hidden');
        }

        modalCloseBtn.onclick = () => {
            modal.classList.add('hidden');
        };

        // Game State Variables
        let detectiveName = "My Dear Watson";
        let score = 0;
        let hintsUsed = 0;
        let hintsForCurrentItem = 0;
        const totalEvidence = 11;
        let evidenceCorrectlyPlaced = 0;

        const evidenceMap = {
            'evidence-1': { framework: 'Insight', hints: ["Oncologists lacked confidence (42% readiness) is a measure of a gap, which is key to a problem's 'Insight'."] },
            'evidence-2': { framework: 'Insight', hints: ["Chart audits confirm prescription gap (54% follow guidelines) is data that provides 'Insight' into a problem that needs to be solved."] },
            'evidence-3': { framework: 'Action', hints: ["A bias-free CE program with case-based modules is a deliberate 'Action' taken to address the insights."] },
            'evidence-4': { framework: 'Action', hints: ["Faculty-led guideline discussions & polling are part of the 'Action' or the design of the learning intervention."] },
            'evidence-5': { framework: 'Outcome', hints: ["Post-test scores improved by 32% is a direct 'Outcome' of the learning experience."] },
            'evidence-6': { framework: 'Outcome', hints: ["EHR audit shows 22% more guideline-concordant prescribing is a measurable 'Outcome' and a true indicator of success."] },
            'evidence-7': { framework: 'Next Step', hints: ["Decision to expand to new sites is the logical 'Next Step' after successful outcomes."] },
            'evidence-8': { framework: 'Weak', hints: ["A symposium chosen because 'topic is hot' is not an evidence-based decision, making it a 'Weak' argument."] },
            'evidence-9': { framework: 'Weak', hints: ["'Applause is not proof, Watson — does a rating measure competence?' This satisfaction score is a 'Weak' metric for measuring true learning or impact."] },
            'evidence-10': { framework: 'Weak', hints: ["Sponsor-controlled content & faculty violates standards, making the evidence 'Weak' due to bias."] },
            'evidence-11': { framework: 'Weak', hints: ["Organizers assumed prescribing improved is not supported by data, making it a 'Weak' claim."] }
        };
        
        // UI Elements
        const dialogueText = document.getElementById('dialogue-text');
        const detectiveNameInput = document.getElementById('detective-name');
        const startBtn = document.getElementById('start-btn');
        const stages = document.querySelectorAll('.stage');
        const continueToCasesBtn = document.getElementById('continue-to-cases-btn');
        const continueToDeductionBtn = document.getElementById('continue-to-deduction-btn');
        const deductionBoard = document.getElementById('deduction-board');
        const evidencePool = document.getElementById('evidence-pool');
        const hintText = document.getElementById('hint-text');
        const hintsArea = document.getElementById('hints-area');
        const showScoreboardBtn = document.getElementById('show-scoreboard-btn');
        const scoreboardIntro = document.getElementById('scoreboard-intro');
        const scoreboardList = document.getElementById('scoreboard-list');
        const userIdDisplay = document.getElementById('user-id');

        // Functions to control game flow
        function showStage(stageId) {
            stages.forEach(stage => stage.classList.add('hidden'));
            document.getElementById(stageId).classList.remove('hidden');
            document.getElementById(stageId).classList.add('fade-in');
        }

        function updateDialogue(text) {
            dialogueText.textContent = text;
        }

        // Event Listeners
        startBtn.addEventListener('click', () => {
            const name = detectiveNameInput.value.trim();
            if (name) {
                detectiveName = name;
                updateDialogue(`Welcome, Detective ${detectiveName}. Two cases await your investigation. Your task: compare them, uncover the truth behind their storytelling, and determine which aligns with high-quality educational standards — and which fails the test.`);
                showStage('stage-2');
            } else {
                showModal('Attention!', 'Please enter your name to begin your investigation.');
            }
        });

        continueToCasesBtn.addEventListener('click', () => {
            updateDialogue("Examine the case files carefully, Detective. The details contained within hold the key to our puzzle.");
            showStage('stage-3');
        });

        continueToDeductionBtn.addEventListener('click', () => {
            updateDialogue("Now for the most crucial part: the deduction. Place each piece of evidence on the board where it truly belongs.");
            showStage('stage-4');
            setupDragAndDrop();
        });

        // Drag and Drop Logic
        function setupDragAndDrop() {
            const draggables = document.querySelectorAll('.drag-item');
            const dropZones = document.querySelectorAll('.drop-zone');

            draggables.forEach(item => {
                item.addEventListener('dragstart', (e) => {
                    e.dataTransfer.setData('text/plain', e.target.id);
                    item.classList.add('opacity-50');
                    hintsForCurrentItem = 0; // Reset hint count for new drag
                    hintsArea.classList.add('hidden');
                });
                item.addEventListener('dragend', (e) => {
                    item.classList.remove('opacity-50');
                });
            });

            dropZones.forEach(zone => {
                zone.addEventListener('dragover', (e) => {
                    e.preventDefault();
                    zone.classList.add('drag-over');
                });
                zone.addEventListener('dragleave', (e) => {
                    zone.classList.remove('drag-over');
                });
                zone.addEventListener('drop', (e) => {
                    e.preventDefault();
                    zone.classList.remove('drag-over');
                    const droppedItemId = e.dataTransfer.getData('text/plain');
                    const droppedItem = document.getElementById(droppedItemId);

                    // Check if the drop is correct
                    const correctZone = evidenceMap[droppedItemId].framework;
                    const droppedZone = zone.id.replace('drop-', '');

                    if (droppedZone === correctZone) {
                        zone.appendChild(droppedItem);
                        droppedItem.setAttribute('draggable', 'false'); // Lock in place
                        droppedItem.classList.remove('drag-item', 'cursor-grab');
                        droppedItem.classList.add('bg-green-600', 'text-white');
                        evidenceCorrectlyPlaced++;
                        if (evidenceCorrectlyPlaced === totalEvidence) {
                            endGame();
                        }
                    } else {
                        // Incorrect placement
                        hintsUsed++;
                        hintsForCurrentItem++;
                        displayHint(droppedItemId);
                    }
                });
            });
        }

        function displayHint(itemId) {
            const hints = evidenceMap[itemId].hints;
            if (hintsForCurrentItem > hints.length) {
                 // Final hint
                hintsArea.classList.remove('hidden');
                hintText.innerHTML = `<strong>Holmes's Final Ruling:</strong> ${hints[hints.length - 1]}`;
            } else if (hintsForCurrentItem > 0) {
                hintsArea.classList.remove('hidden');
                hintText.textContent = hints[hintsForCurrentItem - 1];
            }
        }

        function endGame() {
            // Calculate final score
            score = Math.max(0, 100 - (hintsUsed * 5));
            updateDialogue("The investigation is complete. Case 1 demonstrated true storytelling — grounded in evidence, aligned with standards, and tied to healthcare improvement. Case 2, though dressed in applause, revealed no substance. Numbers mislead unless woven into a narrative of Insight → Action → Outcome → Next Step. And that, Watson, is the only story that counts.");
            showStage('stage-5');
            saveScore(detectiveName, score, hintsUsed);
        }

        // Firebase Firestore Functions
        async function saveScore(name, finalScore, hints) {
            if (!db || !userId) {
                console.error("Firestore not initialized or user not authenticated.");
                return;
            }
            try {
                const userDocRef = doc(db, `artifacts/${appId}/public/data/detective_scores`, userId);
                await setDoc(userDocRef, {
                    name: name,
                    score: finalScore,
                    hintsUsed: hints,
                    timestamp: new Date()
                });
                console.log("Score saved successfully!");
            } catch (e) {
                console.error("Error adding document: ", e);
            }
        }

        async function fetchScores() {
            if (!db) {
                console.error("Firestore not initialized.");
                return;
            }
            try {
                const scoreboardRef = collection(db, `artifacts/${appId}/public/data/detective_scores`);
                const q = query(scoreboardRef);
                onSnapshot(q, (querySnapshot) => {
                    const scores = [];
                    querySnapshot.forEach((doc) => {
                        const data = doc.data();
                        if (data.name && data.score) {
                            scores.push(data);
                        }
                    });
                    // Sort scores descending and take top 3
                    scores.sort((a, b) => b.score - a.score);
                    displayScoreboard(scores.slice(0, 3));
                });
            } catch (e) {
                console.error("Error fetching scores: ", e);
            }
        }

        function displayScoreboard(scores) {
            scoreboardList.innerHTML = ''; // Clear previous list
            scores.forEach((s, index) => {
                const rankText = index === 0 ? "🏆 Master of Insight" : index === 1 ? "🥈 Guardian of Evidence" : "🥉 Seeker of Truth";
                const rankHtml = `
                    <div class="flex items-center justify-between p-3 rounded-md bg-gray-700">
                        <span class="font-bold">${index + 1}. ${s.name} - ${rankText}</span>
                        <span class="text-sm">Score: ${s.score}</span>
                    </div>`;
                scoreboardList.innerHTML += rankHtml;
            });
        }

        showScoreboardBtn.addEventListener('click', () => {
            updateDialogue(`"And so, our investigation concludes. The fog of uncertainty has lifted, and clarity prevails. Here are today’s sharpest detectives:`);
            showStage('stage-6');
            fetchScores();
        });


        // Firebase Initialization and Authentication
        async function initFirebase() {
            try {
                app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);
                
                onAuthStateChanged(auth, async (user) => {
                    if (user) {
                        userId = user.uid;
                        userIdDisplay.textContent = `User ID: ${userId}`;
                        console.log("Authenticated as user:", userId);
                    } else {
                        try {
                            // Sign in anonymously if no user is authenticated
                            await signInAnonymously(auth);
                            console.log("Signed in anonymously.");
                        } catch (error) {
                            console.error("Error signing in anonymously:", error);
                        }
                    }
                });

                if (initialAuthToken) {
                    await signInWithCustomToken(auth, initialAuthToken);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (error) {
                console.error("Firebase initialization failed:", error);
            }
        }

        document.addEventListener('DOMContentLoaded', () => {
            initFirebase();
            showStage('stage-1');
        });
    </script>
</body>

</html>
