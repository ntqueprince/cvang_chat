
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Prince AI Chatbot</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Segoe UI', sans-serif;
    }
    body {
      background: linear-gradient(to right, #43cea2, #185a9d);
      height: 100vh;
      display: flex;
      flex-direction: column;
    }
    .header {
      background: #ffffff;
      padding: 15px;
      text-align: center;
      font-size: 22px;
      font-weight: bold;
      box-shadow: 0 2px 8px rgba(0,0,0,0.2);
    }
    .chat-container {
      flex: 1;
      padding: 20px;
      overflow-y: auto;
      background: #f0f4f8;
    }
    .message {
      max-width: 75%;
      padding: 12px 16px;
      margin-bottom: 12px;
      border-radius: 18px;
      font-size: 15px;
      line-height: 1.4;
      word-wrap: break-word;
      box-shadow: 0 1px 4px rgba(0,0,0,0.2);
      animation: fadeIn 0.3s ease;
    }
    .user {
      background: #43cea2;
      color: #fff;
      align-self: flex-end;
      border-bottom-right-radius: 0;
    }
    .bot {
      background: #ffffff;
      align-self: flex-start;
      border-bottom-left-radius: 0;
    }
    .input-container {
      display: flex;
      padding: 15px;
      background: #ffffff;
      box-shadow: 0 -2px 8px rgba(0,0,0,0.2);
    }
    input {
      flex: 1;
      padding: 14px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 24px;
      outline: none;
      margin-right: 10px;
    }
    button {
      padding: 14px 22px;
      background: #185a9d;
      color: #fff;
      border: none;
      border-radius: 24px;
      cursor: pointer;
      transition: 0.3s;
    }
    button:hover {
      background: #15477a;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(8px); }
      to { opacity: 1; transform: translateY(0); }
    }
  </style>
</head>
<body>

  <div class="header">Prince AI Chatbot 🤖</div>
  <div class="chat-container" id="chat"></div>

  <div class="input-container">
    <input type="text" id="userInput" placeholder="Type your message..." autofocus>
    <button onclick="sendMessage()">Send</button>
  </div>

  <script>
    const apiKey = "AIzaSyBEQ6S8wEtAKEoy1XisBpHTg7eTh-YdhU0";
    const chatContainer = document.getElementById("chat");
    const userInput = document.getElementById("userInput");

    async function sendMessage() {
      const message = userInput.value.trim();
      if (!message) return;

      addMessage("user", message);
      userInput.value = "";

      addMessage("bot", "Typing...");

      const res = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key=${apiKey}`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          contents: [{ parts: [{ text: message }] }]
        })
      });

      const data = await res.json();
      const aiReply = data.candidates?.[0]?.content?.parts?.[0]?.text || "Sorry, I couldn't respond.";

      document.querySelector(".bot:last-child").remove();
      addMessage("bot", aiReply);
    }

    function addMessage(sender, text) {
      const div = document.createElement("div");
      div.className = `message ${sender}`;
      div.textContent = text;
      chatContainer.appendChild(div);
      chatContainer.scrollTop = chatContainer.scrollHeight;
    }

    userInput.addEventListener("keydown", (e) => {
      if (e.key === "Enter") sendMessage();
    });
  </script>

</body>
</html>
