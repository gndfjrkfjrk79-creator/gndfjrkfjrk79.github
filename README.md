<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>AI Chatbot</title>

<style>
body { font-family: Arial; background:#f5f5f5; }
#chatbox {
    width: 400px; height: 500px;
    margin: 40px auto; padding: 20px;
    background:white; border-radius:10px;
    overflow-y: scroll; box-shadow:0 0 10px rgba(0,0,0,0.1);
}
.msg { padding:10px; margin:10px 0; border-radius:10px; max-width:80%; }
.user { background:#d2eaff; margin-left:auto; }
.bot { background:#eee; }
#inputArea { display:flex; width:400px; margin:10px auto; }
#inputArea input { flex:1; padding:10px; }
#inputArea button { padding:10px 20px; }
</style>
</head>
<body>

<h2 style="text-align:center;">AI Chatbot</h2>

<div id="chatbox"></div>

<div id="inputArea">
    <input id="msg" placeholder="Type a message..." />
    <button onclick="sendMessage()">Send</button>
</div>

<script>
async function sendMessage() {
    const box = document.getElementById("chatbox");
    const input = document.getElementById("msg");
    const text = input.value.trim();
    if (!text) return;

    // show user message
    box.innerHTML += `<div class="msg user">${text}</div>`;
    input.value = "";
    box.scrollTop = box.scrollHeight;

    // call public AI proxy API
    const res = await fetch("https://api.deepinfra.com/v1/openai/chat/completions", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
            model: "meta-llama/Meta-Llama-3.1-8B-Instruct",
            messages: [
                { role: "system", content: "You are a helpful chatbot." },
                { role: "user", content: text }
            ]
        })
    });

    const data = await res.json();
    const reply = data.choices[0].message.content;

    // show bot message
    box.innerHTML += `<div class="msg bot">${reply}</div>`;
    box.scrollTop = box.scrollHeight;
}
</script>

</body>
</html>
theme: jekyll-theme-minimal
description: Bookmark this to keep an eye on my project updates!
