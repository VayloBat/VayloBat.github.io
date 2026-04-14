---
layout: page
title: Contact
icon: fas fa-satellite-dish
order: 2
---

<div style="margin-top: 20px;">

    <div style="margin-top: 10px; border: 1px solid #333; padding: 25px; border-radius: 8px; background: rgba(255,255,255,0.02);">
        <h4 style="margin-top: 0; color: #fff; text-transform: uppercase; letter-spacing: 1px; font-size: 0.9rem; border-bottom: 1px solid #333; padding-bottom: 15px;">
            [ Transmission Portal ]
        </h4>
        
        <p style="color: #888; font-size: 0.85rem; line-height: 1.5; margin-bottom: 25px;">
            If you have a message or something important to share, you can write it here. It will be transmitted directly to my secure endpoint.
        </p>
        
        <div style="margin-top: 20px;">
            <label style="color: #c0c0c0; display: block; font-size: 0.75rem; margin-bottom: 8px; font-weight: bold;">SENDER_ID</label>
            <input type="text" id="discord-name" placeholder="Name / Alias / Email..." 
                   style="width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 4px; outline: none; font-size: 0.9rem;">
        </div>

        <div style="margin-top: 20px;">
            <label style="color: #c0c0c0; display: block; font-size: 0.75rem; margin-bottom: 8px; font-weight: bold;">DATA_PACKET</label>
            <textarea id="discord-msg" placeholder="Write your secure message here..." rows="5" 
                      style="width: 100%; padding: 12px; background: #000; border: 1px solid #333; color: #fff; border-radius: 4px; outline: none; resize: vertical; font-size: 0.9rem;"></textarea>
        </div>

        <button onclick="sendToDiscord()" id="send-btn" 
                style="width: 100%; margin-top: 25px; padding: 14px; background: #fff; color: #000; border: none; border-radius: 4px; font-weight: 800; cursor: pointer; text-transform: uppercase;">
            Execute Submission
        </button>

        <p id="status-msg" style="margin-top: 15px; font-size: 0.85rem; display: none; padding: 10px; border-radius: 4px; text-align: center;"></p>
    </div>

    <div style="margin-top: 60px; text-align: center;">
        <p style="color: #555; font-size: 0.75rem; text-transform: uppercase; letter-spacing: 3px; margin-bottom: 25px;">Verified Communication Endpoints</p>
        <div style="display: flex; justify-content: center; gap: 12px; flex-wrap: wrap;">
            <a href="https://github.com/VayloBat" target="_blank" style="text-decoration: none; color: #fff; border: 1px solid #333; padding: 10px 18px; border-radius: 4px; font-size: 0.75rem;">GITHUB</a>
            <a href="https://www.linkedin.com/in/vaylobat" target="_blank" style="text-decoration: none; color: #fff; border: 1px solid #333; padding: 10px 18px; border-radius: 4px; font-size: 0.75rem;">LINKEDIN</a>
            <a href="https://t.me/vaylobat" target="_blank" style="text-decoration: none; color: #fff; border: 1px solid #333; padding: 10px 18px; border-radius: 4px; font-size: 0.75rem;">TELEGRAM</a>
        </div>
    </div>
</div>

{% raw %}
<script>
function sendToDiscord() {
    const webhookURL = "https://discord.com/api/webhooks/1491120789657751654/EBS7d6Fte20pTjEHxMI1pahHTCmJwZDbqxV_0bdxyorMbSGtT5QKqP40-7PQufXsM02K";
    const name = document.getElementById('discord-name').value || "Anonymous_Origin";
    const message = document.getElementById('discord-msg').value;
    const btn = document.getElementById('send-btn');
    const status = document.getElementById('status-msg');

    if (!message) {
        status.innerHTML = "⚠️ [ERROR]: Packet body cannot be empty.";
        status.style.display = "block";
        status.style.color = "#ff3e3e";
        status.style.background = "rgba(255, 62, 62, 0.1)";
        return;
    }

    btn.disabled = true;
    btn.innerText = "Transmitting...";

    const payload = {
        embeds: [{
            title: "🔐 New External Transmission Received",
            color: 16777215,
            fields: [
                { name: "Sender ID", value: "`" + name + "`", inline: true },
                { name: "Content", value: "```" + message + "```" }
            ],
            footer: { text: "Vaylo | Security Research" }
        }]
    };

    fetch(webhookURL, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
    }).then(response => {
        if (response.ok) {
            status.innerHTML = "✅ [SUCCESS]: Packet delivered to core.";
            status.style.display = "block";
            status.style.color = "#fff";
            status.style.background = "rgba(255, 255, 255, 0.1)";
            document.getElementById('discord-msg').value = "";
            document.getElementById('discord-name').value = "";
        } else {
            throw new Error();
        }
    }).catch(() => {
        status.innerHTML = "❌ [FAILURE]: Handshake failed.";
        status.style.display = "block";
        status.style.color = "#ff3e3e";
        status.style.background = "rgba(255, 62, 62, 0.1)";
    }).finally(() => {
        btn.disabled = false;
        btn.innerText = "Execute Submission";
    });
}
</script>
{% endraw %}
        btn.disabled = false;
        btn.innerText = "Execute Submission";
    });
}
</script>
