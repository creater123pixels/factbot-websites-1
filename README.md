<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FactBot</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f9f9f9;
            margin: 0;
            padding: 0;
        }

        .chat-container {
            width: 400px;
            margin: 50px auto;
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        .chat-box {
            max-height: 300px;
            overflow-y: auto;
            margin-bottom: 20px;
            padding-right: 10px;
        }

        .input-container {
            display: flex;
            justify-content: space-between;
        }

        input[type="text"] {
            width: 80%;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }

        button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        button:hover {
            background-color: #45a049;
        }

        .message {
            padding: 10px;
            margin: 10px 0;
            border-radius: 10px;
        }

        .user-message {
            background-color: #3498db;
            color: white;
            text-align: right;
        }

        .bot-message {
            background-color: #ecf0f1;
            color: #2c3e50;
            text-align: left;
        }

        .bot-typing {
            color: #7f8c8d;
            font-style: italic;
        }

        footer {
            text-align: center;
            margin-top: 20px;
            font-size: 14px;
            color: #555;
        }
    </style>
</head>
<body>

<div class="chat-container">
    <div class="chat-box" id="chatBox">
        <div class="message bot-message">Hello! I'm FactBot. How can I help you today?</div>
    </div>
    <div class="input-container">
        <input type="text" id="userInput" placeholder="Type your message..." />
        <button onclick="sendMessage()">Send</button>
    </div>
    <button id="factButton" onclick="sendFact()">Tell me a fact</button>
    <button id="resetButton" onclick="resetChat()">Reset Chat</button>
</div>

<footer>
    <p>Created by: Your Name</p>
    <p>Powered by: ChatGPT</p>
</footer>

<script>
    const botResponses = {
        "hi": "Hello there! Would you like me to proceed with a fact?",
        "hello": "Hey there! Would you like me to proceed with a fact?",
        "how are you?": "I'm just a bot, but I'm doing great, thank you!",
        "bye": "Goodbye! Have a great day!",
        "thanks": "You're welcome!",
        "tell me a fact": getFact(),
        "default": [
            "Sorry, I didn't quite understand that.",
            "Can you please ask something else?",
            "I'm not sure what you're asking, can you clarify?"
        ]
    };

    const facts = [
        "Did you know that honey never spoils? Archaeologists have found pots of honey in ancient tombs that are over 3000 years old!",
        "The longest mountain range in the world is the Andes, stretching about 7,000 kilometers (4,350 miles).",
        "A day on Venus is longer than a year on Venus. It takes Venus about 243 Earth days to rotate once on its axis, but only 225 Earth days to orbit the Sun.",
        "The Eiffel Tower can grow by 6 inches in the summer due to thermal expansion of the metal.",
        "Sharks have been around longer than trees! Sharks have existed for about 400 million years, while trees appeared around 350 million years ago."
    ];

    let usedFacts = [];

    function getFact() {
        if (usedFacts.length === facts.length) {
            usedFacts = [];
        }
        
        let randomIndex;
        do {
            randomIndex = Math.floor(Math.random() * facts.length);
        } while (usedFacts.includes(randomIndex));

        usedFacts.push(randomIndex);
        return facts[randomIndex];
    }

    function simulateTyping(response, callback) {
        const chatBox = document.getElementById("chatBox");
        const typingIndicator = document.createElement("div");
        typingIndicator.classList.add("message", "bot-message", "bot-typing");
        typingIndicator.textContent = "FactBot is typing...";
        chatBox.appendChild(typingIndicator);

        setTimeout(() => {
            typingIndicator.remove();
            const botMessage = document.createElement("div");
            botMessage.classList.add("message", "bot-message");
            botMessage.textContent = response;
            chatBox.appendChild(botMessage);
            callback();
        }, 2000); 
    }

    function sendMessage() {
        const userInput = document.getElementById("userInput").value;
        const chatBox = document.getElementById("chatBox");

        if (userInput.trim() !== "") {
            const userMessage = document.createElement("div");
            userMessage.classList.add("message", "user-message");
            userMessage.textContent = userInput;
            chatBox.appendChild(userMessage);

            document.getElementById("userInput").value = "";

            const lowerCaseInput = userInput.toLowerCase();
            let botResponse = botResponses[lowerCaseInput] || botResponses["default"][Math.floor(Math.random() * botResponses["default"].length)];

            simulateTyping(botResponse, () => {
                chatBox.scrollTop = chatBox.scrollHeight;
            });
        }
    }

    function sendFact() {
        const factResponse = getFact();
        simulateTyping(factResponse, () => {
            const chatBox = document.getElementById("chatBox");
            chatBox.scrollTop = chatBox.scrollHeight;
        });
    }

    function resetChat() {
        const chatBox = document.getElementById("chatBox");
        chatBox.innerHTML = '';
        const welcomeMessage = document.createElement("div");
        welcomeMessage.classList.add("message", "bot-message");
        welcomeMessage.textContent = "Hello! I'm FactBot. How can I help you today?";
        chatBox.appendChild(welcomeMessage);
        document.getElementById("userInput").value = '';
    }

    document.getElementById("userInput").addEventListener("keypress", function(event) {
        if (event.key === "Enter") {
            sendMessage();
        }
    });
</script>

</body>
</html>
