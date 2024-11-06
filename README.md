```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Website</title>
    <link rel="stylesheet" href="style.css"> </head> <link rel="stylesheet" href="style1.css">

<body>
    <div class="container" id="start-container">
        <h1>Welcome to the Quiz!</h1>
        <p>Test your knowledge with our exciting quiz. Click the button below to start!</p>
        <button class="start-btn" onclick="showTopics()">Start Quiz</button>
    </div>

    <div class="container" id="topics-container" style="display:none;">
        <h1>Select a Quiz Topic</h1>
        <div class="topics">
            <button onclick="startQuiz('economics')">Economics</button>
            <button onclick="startQuiz('history')">History</button>
            <button onclick="startQuiz('currentAffairs')">Current Affairs</button>
            <button onclick="startQuiz('generalKnowledge')">General Knowledge</button>
        </div>
    </div>

    <div class="container" id="quiz-container" style="display:none;">
        <h1>Quiz</h1>
        <div class="score">Score: <span id="score">0</span></div>
        <div id="question-container" class="question-container"></div>
        <div class="feedback"></div>
        <div class="control-buttons">
            <button onclick="backToTopics()">Back</button>
            <button onclick="nextQuestion()">Next</button>
        </div>
    </div>


    <script src="script.js"></script>
</body>

</html>
```

```css
/* style.css */
/* ... (Existing styles from index.html) ... */
#start-container, #topics-container, #quiz-container{
        background: rgba(2, 2, 2, 0.5); 
    padding: 40px 20px;
    border-radius: 10px;
    box-shadow: 0 5px 15px rgba(246, 245, 245, 0.969);
    max-width: 400px;
    width: 100%;
    text-align: center;
}
/* style1.css (New styles from index1.html) */
/* ... (Existing styles from index1.html, except body styles) ... */
```

```javascript
// script.js
const quizQuestions = { /* ... (Your quiz data) */ };

let currentTopic = "";
let currentQuestionIndex = 0;
let userScore = 0;
const startContainer = document.getElementById('start-container');
const topicsContainer = document.getElementById('topics-container');
const quizContainer = document.getElementById('quiz-container');


function showTopics() {
    startContainer.style.display = "none";
    topicsContainer.style.display = "block";
}

function startQuiz(topic) {
    currentTopic = topic;
    currentQuestionIndex = 0;
    userScore = 0;

    topicsContainer.style.display = "none";
    quizContainer.style.display = "block";
    document.getElementById('score').textContent = userScore;
    loadQuestion();

}

function loadQuestion() {
    const questions = quizQuestions[currentTopic];
    const questionObj = questions[currentQuestionIndex];

    const questionContainer = document.getElementById('question-container');
    questionContainer.innerHTML = `
        <div class="question">${questionObj.question}</div>
        <div class="options">
            ${questionObj.options.map((option) => `
                <button onclick="selectOption('${option}', '${questionObj.answer}')">${option}</button>
            `).join('')}
        </div>
    `;
    document.querySelector('.feedback').innerHTML = '';
}


function selectOption(selectedOption, correctAnswer) {
    // ... (Your existing selectOption function)
}



function nextQuestion() {
    currentQuestionIndex++;
    if (currentQuestionIndex < quizQuestions[currentTopic].length) {
        loadQuestion();
    } else {
        quizContainer.innerHTML = `
            <h1>Quiz Completed!</h1>
            <p>Your final score is ${userScore} out of ${quizQuestions[currentTopic].length}.</p>
            <button onclick="location.reload()">Try Another Quiz</button>
        `;
    }
}

function backToTopics() {
    quizContainer.style.display = "none";
    topicsContainer.style.display = "block";  // Show topics container again
    currentTopic = ""; // Reset current topic
    currentQuestionIndex = 0; // Reset question index
    userScore = 0;      // Reset the score
}



function capitalize(string) {
    // ... (Your existing capitalize function) ...
}


```

Key changes and explanations:

1.  **Consolidated HTML:** All HTML content is now in `index.html`. The different sections (start screen, topic selection, quiz) are placed within divs with unique IDs.  Initially, only the start screen (`start-container`) is visible.
2.  **CSS Organization:** Styles are split into `style.css` (for the general layout) and `style1.css` (for quiz-specific styles). This improves organization and maintainability.  Removed the `body` styles from  `style1.css` to avoid conflicts as they are already present in `style.css`.
3.  **JavaScript Logic:**
    *   The `startQuiz()` function now takes the `topic` as a parameter. It also hides the topic selection and shows the quiz container.
    *   `loadQuestion()` is simplified to load questions based on the `currentTopic`.
    *   `nextQuestion()` now checks against the length of the questions array for the `currentTopic`.
    *   A new `backToTopics()` function is introduced to handle returning to the topic selection screen, resetting the quiz state, and hiding the quiz container while showing the topics container.
    *   Global variables are used (`currentTopic`, `currentQuestionIndex`, `userScore`) to track quiz state across functions.
    *   `showTopics()` function added to display topic selection when the "Start Quiz" button is clicked.
4.  **Event Handling:**  Click handlers on buttons now call the appropriate JavaScript functions.  `window.selectOption` is no longer needed, use `selectOption` directly. The `nextQuestion` button's `onclick` is simplified to just call `nextQuestion()`.
5.  **Removed Redundant Code:**  The separate `index1.html` file and the redundant `startQuiz()` call within the initial `startQuiz()` function have been removed.

This revised structure improves the code's organization, making it easier to manage, maintain, and extend.  It follows a more standard approach to single-page web applications, using JavaScript to dynamically control content visibility and flow.