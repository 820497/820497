<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text to Realistic Voice</title>
    <style>
        /* Basic reset */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }

        /* Container styling */
        .container {
            max-width: 500px;
            margin: 50px auto;
            padding: 20px;
            border: 1px solid #ddd;
            border-radius: 8px;
            background-color: #f9f9f9;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin: 10px 0 5px;
            font-weight: bold;
        }

        textarea {
            width: 100%;
            height: 100px;
            padding: 10px;
            font-size: 16px;
            border: 1px solid #ccc;
            border-radius: 5px;
            resize: vertical;
        }

        select, input[type="range"] {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        button {
            width: 100%;
            padding: 10px;
            margin-top: 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #45a049;
        }

        /* Responsive styling */
        @media (max-width: 600px) {
            .container {
                width: 90%;
                padding: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Text to Realistic Voice</h1>
        <label for="text-input">Enter Text:</label>
        <textarea id="text-input" placeholder="Type something to be spoken..."></textarea>

        <label for="voice-select">Choose Voice:</label>
        <select id="voice-select"></select>

        <label for="pitch">Pitch: <span id="pitch-value">1</span></label>
        <input type="range" id="pitch" min="0" max="2" value="1" step="0.1">

        <label for="rate">Rate: <span id="rate-value">1</span></label>
        <input type="range" id="rate" min="0.5" max="2" value="1" step="0.1">

        <button onclick="speak()">Speak</button>
    </div>

    <script>
        // Variables to store voices and speech settings
        let voices = [];
        const voiceSelect = document.getElementById("voice-select");
        const pitch = document.getElementById("pitch");
        const rate = document.getElementById("rate");
        const textInput = document.getElementById("text-input");
        const pitchValue = document.getElementById("pitch-value");
        const rateValue = document.getElementById("rate-value");

        // Load available voices
        function loadVoices() {
            voices = speechSynthesis.getVoices();
            voiceSelect.innerHTML = '';
            voices.forEach((voice, index) => {
                const option = document.createElement("option");
                option.textContent = `${voice.name} (${voice.lang})`;
                option.value = index;
                voiceSelect.appendChild(option);
            });
        }

        // Update displayed pitch and rate values
        pitch.addEventListener("input", () => pitchValue.textContent = pitch.value);
        rate.addEventListener("input", () => rateValue.textContent = rate.value);

        // Trigger loading of voices
        if ('speechSynthesis' in window) {
            speechSynthesis.onvoiceschanged = loadVoices;
        } else {
            alert("Sorry, your browser does not support text-to-speech.");
        }

        // Speak function
        function speak() {
            if (!textInput.value) return;
            
            const utterance = new SpeechSynthesisUtterance(textInput.value);
            utterance.voice = voices[voiceSelect.value];
            utterance.pitch = pitch.value;
            utterance.rate = rate.value;
            speechSynthesis.speak(utterance);
        }
    </script>
</body>
</html>

