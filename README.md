<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Classroom Expectations Jeopardy</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #1a202c; /* Dark background for contrast */
            color: #e2e8f0; /* Light text color */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
            box-sizing: border-box;
        }
        .container {
            background-color: #2d3748; /* Slightly lighter dark background for the container */
            border-radius: 1rem; /* Rounded corners for the main container */
            box-shadow: 0 10px 15px rgba(0, 0, 0, 0.5); /* Stronger shadow */
            padding: 2.5rem; /* More padding */
            max-width: 90%;
            width: 1200px;
            display: flex;
            flex-direction: column;
            gap: 2rem;
            align-items: center;
        }
        .game-board {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 1rem; /* Gap between cells */
            width: 100%;
        }
        .category-header, .jeopardy-cell {
            display: flex;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 1rem;
            border-radius: 0.5rem; /* Rounded corners */
            font-weight: 600;
            color: #ffffff;
            transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
            border: 2px solid rgba(255, 255, 255, 0.1);
        }
        .category-header {
            background-color: #4a5568; /* Darker blue-gray */
            font-size: 1.25rem; /* Larger font for categories */
            min-height: 80px; /* Ensure consistent height */
        }
        .jeopardy-cell {
            background-color: #3b82f6; /* Bright blue for active cells */
            font-size: 1.5rem; /* Larger font for points */
            cursor: pointer;
            min-height: 100px; /* Consistent height for question cells */
        }
        .jeopardy-cell:hover:not(.answered) {
            transform: translateY(-5px); /* Lift effect on hover */
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.4);
        }
        .jeopardy-cell.answered {
            background-color: #64748b; /* Grey out answered questions */
            cursor: not-allowed;
            text-decoration: line-through;
            opacity: 0.7;
        }
        .question-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.85); /* Darker overlay */
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease, visibility 0.3s ease;
        }
        .question-modal.active {
            opacity: 1;
            visibility: visible;
        }
        .modal-content {
            background-color: #2d3748; /* Same as container background */
            padding: 2.5rem;
            border-radius: 1rem;
            box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
            width: 80%;
            max-width: 800px;
            text-align: center;
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            position: relative;
            transform: scale(0.9);
            transition: transform 0.3s ease;
        }
        .question-modal.active .modal-content {
            transform: scale(1);
        }
        .modal-content h2 {
            font-size: 2rem; /* Larger question title */
            font-weight: 700;
            color: #a7f3d0; /* Highlight color for question */
            margin-bottom: 1rem;
        }
        .modal-content p {
            font-size: 1.5rem; /* Larger text for questions/answers */
            margin-bottom: 1.5rem;
        }
        .answer-text {
            color: #fbd38d; /* Orange-yellow for answer */
            font-weight: 600;
        }
        .modal-buttons {
            display: flex;
            justify-content: center;
            gap: 1rem;
        }
        .btn {
            padding: 0.75rem 1.5rem;
            border-radius: 0.5rem;
            font-size: 1.125rem;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.2s ease, transform 0.1s ease;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
            border: none;
        }
        .btn-primary {
            background-color: #38a169; /* Green for reveal/mark answered */
            color: white;
        }
        .btn-primary:hover {
            background-color: #2f855a;
            transform: translateY(-2px);
        }
        .btn-secondary {
            background-color: #e53e3e; /* Red for close */
            color: white;
        }
        .btn-secondary:hover {
            background-color: #c53030;
            transform: translateY(-2px);
        }
        .btn-reset {
            background-color: #a0aec0; /* Grey for reset */
            color: #2d3748;
            margin-top: 1.5rem;
        }
        .btn-reset:hover {
            background-color: #718096;
            transform: translateY(-2px);
        }
        .score-display {
            font-size: 1.75rem;
            font-weight: 700;
            margin-bottom: 1rem;
            color: #a7f3d0;
        }

        /* Responsive adjustments */
        @media (max-width: 768px) {
            .container {
                padding: 1.5rem;
                gap: 1.5rem;
            }
            .game-board {
                grid-template-columns: repeat(3, 1fr); /* 3 columns on smaller screens */
                font-size: 0.9rem;
            }
            .category-header {
                font-size: 1rem;
                min-height: 60px;
            }
            .jeopardy-cell {
                font-size: 1.2rem;
                min-height: 80px;
            }
            .modal-content {
                padding: 1.5rem;
                width: 95%;
            }
            .modal-content h2 {
                font-size: 1.5rem;
            }
            .modal-content p {
                font-size: 1.2rem;
            }
            .btn {
                padding: 0.6rem 1.2rem;
                font-size: 1rem;
            }
            .score-display {
                font-size: 1.5rem;
            }
        }

        @media (max-width: 480px) {
            .game-board {
                grid-template-columns: repeat(2, 1fr); /* 2 columns on very small screens */
            }
            .category-header {
                font-size: 0.9rem;
                min-height: 50px;
            }
            .jeopardy-cell {
                font-size: 1rem;
                min-height: 70px;
            }
            .modal-content {
                padding: 1rem;
            }
            .modal-content h2 {
                font-size: 1.2rem;
            }
            .modal-content p {
                font-size: 1rem;
            }
            .modal-buttons {
                flex-direction: column;
            }
            .btn {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 class="text-4xl font-bold text-center text-white mb-4">Classroom Expectations Jeopardy!</h1>
        <div id="score-display" class="score-display">Score: 0</div>
        <div id="game-board" class="game-board">
            <!-- Jeopardy cells will be dynamically generated here -->
        </div>

        <button id="reset-button" class="btn btn-reset">Reset Game</button>
    </div>

    <!-- Question Modal -->
    <div id="question-modal" class="question-modal">
        <div class="modal-content">
            <h2 id="modal-category"></h2>
            <p id="modal-question"></p>
            <p id="modal-answer" class="answer-text hidden"></p>
            <div class="modal-buttons">
                <button id="reveal-button" class="btn btn-primary">Reveal Answer</button>
                <button id="mark-answered-button" class="btn btn-primary">Mark as Answered</button>
                <button id="close-modal-button" class="btn btn-secondary">Close</button>
            </div>
        </div>
    </div>

    <script>
        // Game data: Categories, questions, and answers
        const gameData = {
            "Take a Guess": [
                { points: 100, question: "What should you do immediately when you enter the classroom?", answer: "Go directly to your assigned seat and begin the 'Do Now' or bell work activity." },
                { points: 200, question: "When is it okay to pack up your belongings at the end of class?", answer: "Only when the teacher instructs you to do so, not when the bell rings." },
                { points: 300, question: "What is the first thing you should do if you want to speak or contribute to a class discussion?", answer: "Raise your hand and wait to be called upon." },
                { points: 400, question: "If you are confused about an assignment or a concept, what is the best way to get help?", answer: "Ask the teacher for clarification, raise your hand during class, or seek help during designated office hours." },
                { points: 500, question: "Who dismisses the class at the end of the period?", answer: "The teacher dismisses the class, not the bell." }
            ],
            "Favorites": [
                { points: 100, question: "What is your favorite subject to learn about (besides this one!)?", answer: "This question is open-ended! Any subject you enjoy is a great answer." },
                { points: 200, question: "Which method helps you learn best: reading, listening, or doing (hands-on activities)?", answer: "This question is open-ended! Everyone learns differently, and all methods are valuable." },
                { points: 300, question: "What's your favorite way to collaborate with classmates on a project?", answer: "This question is open-ended! Common ways include brainstorming, dividing tasks, or peer editing." },
                { points: 400, question: "Which classroom supply do you use the most (e.g., pen, pencil, notebook, ruler)?", answer: "This question is open-ended! Whatever helps you succeed the most." },
                { points: 500, question: "What's one thing you hope to learn or achieve in this class this year?", answer: "This question is open-ended! Setting goals is a great way to start the year." }
            ],
            "Fun Fact": [
                { points: 100, question: "True or False: Our classroom's 'secret handshake' involves three high-fives and a wiggle.", answer: "False! (Unless we decide to make one up! 😉)" },
                { points: 200, question: "What is the teacher's favorite color? (Hint: It's the color of a clear sky on a nice day).", answer: "Blue! (Or whatever your actual favorite color is, teacher!)" },
                { points: 300, question: "Name one unexpected (but still appropriate!) use for a common classroom item like a paperclip.", answer: "Possible answers: bookmark, temporary wire holder, key ring, makeshift screwdriver for small screws (use with care!)." },
                { points: 400, question: "What is one 'fun fact' about why keeping our classroom tidy is helpful?", answer: "A clean space helps us think clearly, find things easily, and prevents accidents or tripping hazards!" },
                { points: 500, question: "What does the Latin root of the word 'student' (studere) mean?", answer: "It means 'to be eager' or 'to devote oneself'! So you are someone eager to learn." }
            ],
            "Would You Rather": [
                { points: 100, question: "Would you rather work on an assignment in a quiet classroom or a classroom with soft background music?", answer: "This question is open-ended! Both can be good depending on personal preference." },
                { points: 200, question: "Would you rather have all homework due on Monday morning or spread out through the week?", answer: "This question is open-ended! Both have pros and cons for managing workload." },
                { points: 300, question: "Would you rather be responsible for cleaning up a messy desk area or organizing a bookshelf full of textbooks?", answer: "This question is open-ended! Both involve tidying, which is important." },
                { points: 400, question: "Would you rather forget your pencil for class or forget your notebook?", answer: "This question is open-ended! Both can be challenging, but a notebook might be harder to improvise." },
                { points: 500, question: "Would you rather present a project individually to the class or with a group of classmates?", answer: "This question is open-ended! Both require preparation and speaking skills." }
            ],
            "This or That": [
                { points: 100, question: "Notebooks or Binders?", answer: "This question is open-ended! Both are great for organization." },
                { points: 200, question: "Pens or Pencils?", answer: "This question is open-ended! Pens are permanent, pencils allow for erasing mistakes." },
                { points: 300, question: "Group work or Individual work?", answer: "This question is open-ended! Both are valuable for different learning objectives." },
                { points: 400, question: "Digital notes or Handwritten notes?", answer: "This question is open-ended! Digital notes are searchable, handwritten notes can aid memory." },
                { points: 500, question: "Morning person or Evening person?", answer: "This question is open-ended! Understanding your own energy levels can help with learning." }
            ]
        };

        let currentScore = 0;
        let currentCell = null; // To keep track of the currently selected cell
        let currentQuestionData = null; // To store current question details for modal

        // Get DOM elements
        const gameBoard = document.getElementById('game-board');
        const questionModal = document.getElementById('question-modal');
        const modalCategory = document.getElementById('modal-category');
        const modalQuestion = document.getElementById('modal-question');
        const modalAnswer = document.getElementById('modal-answer');
        const revealButton = document.getElementById('reveal-button');
        const markAnsweredButton = document.getElementById('mark-answered-button');
        const closeModalButton = document.getElementById('close-modal-button');
        const resetButton = document.getElementById('reset-button');
        const scoreDisplay = document.getElementById('score-display');

        /**
         * Initializes the game board by dynamically creating cells based on gameData.
         * Sets up event listeners for each cell.
         */
        function initializeGameBoard() {
            gameBoard.innerHTML = ''; // Clear existing board
            // Create category headers
            for (const category in gameData) {
                const header = document.createElement('div');
                header.classList.add('category-header');
                header.textContent = category;
                gameBoard.appendChild(header);
            }

            // Create question cells
            const categories = Object.keys(gameData);
            const numPoints = 5; // 100, 200, 300, 400, 500

            // Loop through point values first (row by row visually)
            for (let i = 0; i < numPoints; i++) {
                categories.forEach(category => {
                    const questionIndex = i;
                    const question = gameData[category][questionIndex];
                    if (question) {
                        const cell = document.createElement('div');
                        cell.classList.add('jeopardy-cell');
                        cell.textContent = `$${question.points}`;
                        cell.dataset.category = category;
                        cell.dataset.questionIndex = questionIndex;
                        cell.addEventListener('click', () => handleCellClick(cell, question));
                        gameBoard.appendChild(cell);
                    }
                });
            }
        }

        /**
         * Handles the click event on a Jeopardy cell.
         * Displays the question modal with the corresponding question.
         * @param {HTMLElement} cell - The clicked cell element.
         * @param {Object} questionData - The data for the question.
         */
        function handleCellClick(cell, questionData) {
            if (cell.classList.contains('answered')) {
                return; // Do nothing if already answered
            }

            currentCell = cell;
            currentQuestionData = questionData;

            modalCategory.textContent = cell.dataset.category;
            modalQuestion.textContent = questionData.question;
            modalAnswer.textContent = questionData.answer;

            // Reset modal state
            modalAnswer.classList.add('hidden');
            revealButton.classList.remove('hidden');
            markAnsweredButton.classList.remove('hidden');

            questionModal.classList.add('active');
        }

        /**
         * Reveals the answer in the question modal.
         */
        function revealAnswer() {
            modalAnswer.classList.remove('hidden');
            revealButton.classList.add('hidden'); // Hide reveal button once answer is shown
        }

        /**
         * Marks the current question as answered, updates the score,
         * and closes the modal.
         */
        function markAsAnswered() {
            if (currentCell && currentQuestionData) {
                currentCell.classList.add('answered');
                currentCell.textContent = ''; // Clear points after answering
                // Add points to score (assuming answering correctly gets points for review)
                currentScore += currentQuestionData.points;
                scoreDisplay.textContent = `Score: ${currentScore}`;
            }
            closeModal();
        }

        /**
         * Closes the question modal.
         */
        function closeModal() {
            questionModal.classList.remove('active');
            currentCell = null;
            currentQuestionData = null;
        }

        /**
         * Resets the game to its initial state.
         */
        function resetGame() {
            currentScore = 0;
            scoreDisplay.textContent = `Score: ${currentScore}`;
            initializeGameBoard(); // Re-render the board
            closeModal();
        }

        // Event Listeners for modal buttons and reset button
        revealButton.addEventListener('click', revealAnswer);
        markAnsweredButton.addEventListener('click', markAsAnswered);
        closeModalButton.addEventListener('click', closeModal);
        resetButton.addEventListener('click', resetGame);

        // Initialize the game when the window loads
        window.onload = initializeGameBoard;
    </script>
</body>
</html>
