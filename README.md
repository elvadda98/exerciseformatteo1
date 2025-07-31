<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fill in the Gaps Exercise</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: #333;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        
        h1 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 30px;
        }
        
        .exercise-container {
            background-color: white;
            padding: 25px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .instructions {
            background-color: #e8f4fc;
            padding: 15px;
            border-left: 4px solid #3498db;
            margin-bottom: 20px;
            border-radius: 0 4px 4px 0;
        }
        
        .question {
            margin-bottom: 15px;
            padding: 15px;
            background-color: #f9f9f9;
            border-radius: 5px;
            border-left: 3px solid #3498db;
        }
        
        input[type="text"] {
            padding: 8px 12px;
            border: 1px solid #ddd;
            border-radius: 4px;
            width: 180px;
            font-size: 16px;
            margin-left: 10px;
        }
        
        .button-container {
            display: flex;
            justify-content: space-between;
            margin-top: 25px;
        }
        
        button {
            background-color: #3498db;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s;
        }
        
        button:hover {
            background-color: #2980b9;
            transform: translateY(-2px);
        }
        
        #reset-btn {
            background-color: #6c757d;
        }
        
        #reset-btn:hover {
            background-color: #5a6268;
        }
        
        .feedback {
            margin-top: 25px;
            padding: 20px;
            border-radius: 5px;
            display: none;
        }
        
        .correct {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        
        .word-bank {
            background-color: #fff3cd;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 25px;
            font-weight: bold;
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
        }
        
        .word-bank span {
            background-color: #ffe69c;
            padding: 5px 10px;
            border-radius: 3px;
        }
        
        .score-display {
            text-align: center;
            font-size: 1.2rem;
            margin: 20px 0;
            font-weight: bold;
        }
        
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <h1>Fill in the Gaps Exercise</h1>
    
    <div class="exercise-container">
        <div class="instructions">
            <p>Fill in each gap with the correct word from the word bank. Words may be used more than once.</p>
        </div>
        
        <div class="word-bank">
            <span>motorway</span>
            <span>diverse</span>
            <span>crowded</span>
            <span>instead</span>
            <span>flat</span>
            <span>give</span>
            <span>carry</span>
        </div>
        
        <div id="questions-container">
            <!-- Questions will be inserted here by JavaScript -->
        </div>
        
        <div class="button-container">
            <button id="reset-btn" onclick="resetAnswers()">Reset Answers</button>
            <button id="check-btn" onclick="checkAnswers()">Check Answers</button>
        </div>
        
        <div class="feedback" id="feedback"></div>
    </div>
    
    <script>
        // Shuffled questions with answers
        const questions = [
            { sentence: "1. The _____ was so busy that we were stuck in traffic for two hours.", answer: "motorway" },
            { sentence: "2. Could you _____ me your opinion about the new design?", answer: "give" },
            { sentence: "3. Our office is very _____ - we have colleagues from 15 different countries.", answer: "diverse" },
            { sentence: "4. I can't _____ all these tools by myself - they're too heavy.", answer: "carry" },
            { sentence: "5. The city center gets extremely _____ on Saturday afternoons.", answer: "crowded" },
            { sentence: "6. We decided to take the train _____ of driving.", answer: "instead" },
            { sentence: "7. My _____ is on the fifth floor with a nice view of the park.", answer: "flat" },
            { sentence: "8. The A1 _____ connects London to Edinburgh.", answer: "motorway" },
            { sentence: "9. Please _____ these documents to the accounting department.", answer: "carry" },
            { sentence: "10. The museum offers _____ programs for people of all ages.", answer: "diverse" },
            { sentence: "11. The restaurant was too _____, so we went somewhere else.", answer: "crowded" },
            { sentence: "12. I'll _____ you a call when I arrive at the station.", answer: "give" },
            { sentence: "13. They moved from a house to a _____ in the city center.", answer: "flat" },
            { sentence: "14. We took a wrong turn on the _____ and got lost.", answer: "motorway" },
            { sentence: "15. The team has a _____ range of skills and experiences.", answer: "diverse" },
            { sentence: "16. Can you _____ me a minute to discuss the project?", answer: "give" },
            { sentence: "17. The elevator was broken, so we had to _____ all the boxes upstairs.", answer: "carry" },
            { sentence: "18. The beach gets _____ in July and August.", answer: "crowded" },
            { sentence: "19. We didn't go to Paris _____ we went to Lyon.", answer: "instead" },
            { sentence: "20. She rents a small _____ near the university.", answer: "flat" }
        ];

        // Shuffle the questions array
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // Display shuffled questions
        function displayQuestions() {
            const container = document.getElementById('questions-container');
            container.innerHTML = '';
            
            shuffleArray(questions).forEach((q, index) => {
                const questionDiv = document.createElement('div');
                questionDiv.className = 'question';
                questionDiv.innerHTML = `
                    <p>${q.sentence}</p>
                    <input type="text" id="answer${index}" data-answer="${q.answer.toLowerCase()}">
                `;
                container.appendChild(questionDiv);
            });
        }

        // Check answers
        function checkAnswers() {
            let score = 0;
            let feedbackHtml = '';
            const totalQuestions = questions.length;
            
            for (let i = 0; i < totalQuestions; i++) {
                const input = document.getElementById(`answer${i}`);
                const userAnswer = input.value.trim().toLowerCase();
                const correctAnswer = input.dataset.answer;
                
                if (userAnswer === correctAnswer) {
                    score++;
                    input.style.borderColor = '#28a745';
                    feedbackHtml += `<p>Question ${i+1}: <span style="color:#28a745">✓ Correct</span> - "${correctAnswer}"</p>`;
                } else {
                    input.style.borderColor = '#dc3545';
                    feedbackHtml += `<p>Question ${i+1}: <span style="color:#dc3545">✗ Incorrect</span> - The correct answer is "${correctAnswer}"</p>`;
                }
            }
            
            const percentage = Math.round((score / totalQuestions) * 100);
            let resultMessage = '';
            
            if (percentage === 100) {
                resultMessage = "Perfect score! Excellent work!";
            } else if (percentage >= 80) {
                resultMessage = "Great job! You know most of these words well.";
            } else if (percentage >= 60) {
                resultMessage = "Good effort! Review the words you missed.";
            } else {
                resultMessage = "Keep practicing! Try the exercise again.";
            }
            
            document.getElementById('feedback').innerHTML = `
                <div class="score-display">Your score: ${score}/${totalQuestions} (${percentage}%)</div>
                <p><strong>${resultMessage}</strong></p>
                <div style="max-height: 300px; overflow-y: auto; margin-top: 15px;">
                    ${feedbackHtml}
                </div>
            `;
            document.getElementById('feedback').style.display = 'block';
        }

        // Reset all answers
        function resetAnswers() {
            const inputs = document.querySelectorAll('input[type="text"]');
            inputs.forEach(input => {
                input.value = '';
                input.style.borderColor = '#ddd';
            });
            document.getElementById('feedback').style.display = 'none';
        }

        // Initialize the exercise when page loads
        window.onload = function() {
            displayQuestions();
        };
    </script>
</body>
</html>
