# happy-birthday
happybirthday.web
[happybirthdayweb.html](https://github.com/user-attachments/files/29531530/happybirthdayweb.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Birthday Mom — Gram Hub</title>
    <style>
        :root {
            --primary: #2c4a6e;
            --bg: #e7e5e1;
            --dark: #3c3b38;
            --white: #ffffff;
        }
        * { box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg);
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }
        h1, h2 { color: var(--dark); text-align: center; }

        .container {
            background: var(--white);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 500px;
            text-align: center;
            margin-bottom: 20px;
        }
        input, textarea, select {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border: 2px solid #ddd;
            border-radius: 10px;
            font-size: 16px;
            font-family: inherit;
        }
        .color-group {
            display: flex;
            align-items: center;
            gap: 15px;
            margin: 10px 0;
        }
        .color-group label { font-weight: bold; color: var(--dark); }
        input[type="color"] {
            width: 60px;
            height: 40px;
            padding: 2px;
            cursor: pointer;
        }
        button {
            background: var(--primary);
            color: var(--white);
            border: none;
            padding: 12px 25px;
            border-radius: 10px;
            font-size: 18px;
            cursor: pointer;
            transition: 0.2s;
            width: 100%;
        }
        button:hover { background: #1e3550; transform: scale(1.02); }
        button:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }

        .grams-grid {
            display: flex;
            flex-direction: column;
            gap: 15px;
            width: 100%;
            max-width: 500px;
        }
        .empty-state {
            text-align: center;
            color: #888;
            padding: 20px;
        }
        .gram-card {
            background: var(--white);
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
            cursor: pointer;
            position: relative;
            transition: 0.2s;
            border-left: 8px solid var(--primary);
            display: flex;
            gap: 15px;
            align-items: center;
        }
        .gram-card:hover { transform: translateY(-3px); }
        .gram-card-content { flex-grow: 1; }
        .gram-title { font-weight: bold; font-size: 20px; color: var(--dark); }
        .gram-sender { font-size: 14px; color: #666; margin-top: 5px; }
        .gram-countdown { font-size: 13px; color: var(--primary); font-weight: bold; margin-top: 8px; }

        .thumb-preview {
            width: 60px;
            height: 60px;
            border-radius: 10px;
            object-fit: cover;
            border: 2px solid #ddd;
            flex-shrink: 0;
        }
        .thumb-sticker {
            width: 60px;
            height: 60px;
            border-radius: 10px;
            border: 2px solid #ddd;
            background: #f1f2f6;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
        }

        .card-actions {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }
        .share-btn, .delete-btn {
            font-size: 12px;
            padding: 6px 12px;
            width: auto;
        }
        .share-btn { background: #7a7873; }
        .share-btn:hover { background: #625f5b; }
        .delete-btn { background: #5a453e; }
        .delete-btn:hover { background: #45332d; }

        /* Fullscreen Gram Overlay */
        #gram-view {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100vw; height: 100vh;
            background: var(--bg);
            z-index: 100;
            overflow-y: auto;
            text-align: center;
            padding: 30px 20px 60px;
        }
        .cake { font-size: 90px; margin: 10px 0; cursor: pointer; user-select: none; display: inline-block; }
        .cake:hover { transform: scale(1.05); }
        .candle-flame { display: inline-block; animation: flicker 0.5s infinite alternate; }
        @keyframes flicker { 0% { transform: translateY(0); } 100% { transform: translateY(-5px); } }
        .cake-hint { font-size: 13px; color: #888; margin-top: -5px; margin-bottom: 10px; }

        #confetti-canvas {
            position: fixed;
            top: 0; left: 0; pointer-events: none;
            z-index: 200;
            width: 100%; height: 100%;
        }
        .nav-btn { width: auto; display: inline-block; margin: 0 0 20px 0; background: var(--dark); }
        .nav-btn:hover { background: #232220; }

        .live-timer {
            font-size: 22px;
            font-weight: bold;
            color: var(--dark);
            background: #fff;
            padding: 15px;
            border-radius: 12px;
            display: inline-block;
            margin-top: 15px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
        }

        .canvas-wrapper {
            position: relative;
            width: 100%;
            max-width: 400px;
            margin: 0 auto 15px auto;
            border-radius: 12px;
            overflow: hidden;
            border: 3px solid #ddd;
            background: #f1f2f6;
            aspect-ratio: 4 / 3;
            display: none;
        }
        .gram-photo-bg {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
        }
        .movable-sticker {
            position: absolute;
            font-size: 65px;
            cursor: grab;
            user-select: none;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            touch-action: none;
        }
        .movable-sticker:active { cursor: grabbing; }

        .sticker-options-grid {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin: 10px 0;
            background: #f1f2f6;
            padding: 10px;
            border-radius: 10px;
        }
        .sticker-btn {
            font-size: 32px;
            background: none;
            border: 2px solid transparent;
            cursor: pointer;
            width: auto;
            padding: 5px;
            border-radius: 8px;
            transition: 0.2s;
        }
        .sticker-btn:hover { transform: scale(1.15); }
        .sticker-btn.selected { border-color: var(--primary); background: #fff; }

        .field-label {
            display: block;
            text-align: left;
            font-weight: bold;
            color: var(--dark);
            font-size: 14px;
            margin-top: 5px;
        }
        .status-msg {
            font-size: 14px;
            color: var(--primary);
            font-weight: bold;
            min-height: 18px;
        }
        .photo-preview-small {
            width: 100%;
            max-height: 150px;
            object-fit: cover;
            border-radius: 10px;
            margin-top: 8px;
            display: none;
        }
    </style>
</head>
<body>

    <canvas id="confetti-canvas"></canvas>

    <!-- Main Dashboard View -->
    <div id="dashboard-view" style="width: 100%; display: flex; flex-direction: column; align-items: center;">
        <div class="container">
            <h1>Happy Birthday Mom! 🎂</h1>
            <p>Leave a card for this very special July 3rd!</p>

            <form id="gram-form">
                <label class="field-label" for="recipient">What do you call her?</label>
                <input type="text" id="recipient" placeholder="e.g. Mom, Suzie, etc." required>

                <label class="field-label" for="sender">Your name</label>
                <input type="text" id="sender" placeholder="Your name" required>

                <label class="field-label" for="message">Your birthday message</label>
                <textarea id="message" placeholder="Write something from the heart..." rows="4" required></textarea>

                <label class="field-label" for="photo-input">Add a photo (optional)</label>
                <input type="file" id="photo-input" accept="image/*">
                <img id="photo-preview" class="photo-preview-small" alt="Preview">

                <div class="color-group">
                    <label for="card-color">Card color:</label>
                    <input type="color" id="card-color" value="#2c4a6e">
                </div>

                <label class="field-label">Pick a sticker</label>
                <div class="sticker-options-grid">
                    <button type="button" class="sticker-btn selected" data-sticker="🦈">🦈</button>
                    <button type="button" class="sticker-btn" data-sticker="🦖">🦖</button>
                    <button type="button" class="sticker-btn" data-sticker="🦕">🦕</button>
                    <button type="button" class="sticker-btn" data-sticker="🦴">🦴</button>
                </div>

                <button type="submit" id="submit-btn">Add my message 🎉</button>
                <p class="status-msg" id="form-status"></p>
            </form>
        </div>

        <h2>Messages so far</h2>
        <div class="grams-grid" id="grams-container">
            <p class="empty-state" id="empty-state">No messages yet — be the first!</p>
        </div>
    </div>

    <!-- Fullscreen Gram Overlay View -->
    <div id="gram-view">
        <button class="nav-btn" onclick="closeGramView()">← Back to Hub</button>

        <div class="cake" id="cake-btn" title="Click for confetti!">🎂</div>
        <p class="cake-hint">tap the cake!</p>

        <h1 id="view-title">For Someone Special</h1>
        <p id="view-meta">From someone who cares</p>

        <div class="canvas-wrapper" id="view-canvas-wrapper">
            <img class="gram-photo-bg" id="view-photo" src="" alt="Birthday photo">
            <div class="movable-sticker" id="view-sticker">🦈</div>
        </div>

        <p id="view-message" style="font-size: 20px; max-width: 600px; margin: 20px auto; line-height: 1.6; color: #2f3542; white-space: pre-wrap;"></p>

        <div id="countdown-container">
            <h3>Time until the big day:</h3>
            <div class="live-timer" id="view-timer">—</div>
        </div>
    </div>

    <script>
        // ---------- State ----------
        let selectedSticker = '🦈';
        let photoDataUrl = null;
        let countdownIntervalId = null;
        let currentGramId = null;

        // ---------- Sticker picker ----------
        document.querySelectorAll('.sticker-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.sticker-btn').forEach(b => b.classList.remove('selected'));
                btn.classList.add('selected');
                selectedSticker = btn.dataset.sticker;
            });
        });

        // ---------- Photo upload (resized to keep storage small) ----------
        document.getElementById('photo-input').addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (!file) { photoDataUrl = null; document.getElementById('photo-preview').style.display = 'none'; return; }

            const reader = new FileReader();
            reader.onload = (ev) => {
                const img = new Image();
                img.onload = () => {
                    const maxW = 900;
                    const scale = Math.min(1, maxW / img.width);
                    const canvas = document.createElement('canvas');
                    canvas.width = img.width * scale;
                    canvas.height = img.height * scale;
                    const ctx = canvas.getContext('2d');
                    ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                    photoDataUrl = canvas.toDataURL('image/jpeg', 0.8);
                    const preview = document.getElementById('photo-preview');
                    preview.src = photoDataUrl;
                    preview.style.display = 'block';
                };
                img.src = ev.target.result;
            };
            reader.readAsDataURL(file);
        });

        // ---------- Fixed birthday: July 3rd ----------
        function getNextBirthdayISO() {
            const now = new Date();
            const year = now.getFullYear();
            let target = new Date(year, 6, 3); // July is month index 6
            if (now > target) target = new Date(year + 1, 6, 3);
            const mm = String(target.getMonth() + 1).padStart(2, '0');
            const dd = String(target.getDate()).padStart(2, '0');
            return `${target.getFullYear()}-${mm}-${dd}`;
        }

        // ---------- Form submit ----------
        document.getElementById('gram-form').addEventListener('submit', async (e) => {
            e.preventDefault();
            const submitBtn = document.getElementById('submit-btn');
            const status = document.getElementById('form-status');
            submitBtn.disabled = true;
            status.textContent = 'Saving...';

            const gram = {
                id: Date.now() + '-' + Math.random().toString(36).slice(2, 8),
                recipient: document.getElementById('recipient').value.trim(),
                sender: document.getElementById('sender').value.trim(),
                message: document.getElementById('message').value.trim(),
                birthdate: getNextBirthdayISO(),
                color: document.getElementById('card-color').value,
                sticker: selectedSticker,
                photo: photoDataUrl,
                createdAt: Date.now()
            };

            try {
                const result = await window.storage.set('gram:' + gram.id, JSON.stringify(gram), true);
                if (!result) throw new Error('Storage returned no result');
                status.textContent = 'Message saved! 🎉';
                e.target.reset();
                photoDataUrl = null;
                document.getElementById('photo-preview').style.display = 'none';
                selectedSticker = '🦈';
                document.querySelectorAll('.sticker-btn').forEach(b => b.classList.remove('selected'));
                document.querySelector('.sticker-btn[data-sticker="🦈"]').classList.add('selected');
                await loadGrams();
                setTimeout(() => { status.textContent = ''; }, 2500);
            } catch (err) {
                console.error('Save failed:', err);
                status.textContent = 'Something went wrong saving your message — please try again.';
            } finally {
                submitBtn.disabled = false;
            }
        });

        // ---------- Load & render grams ----------
        async function loadGrams() {
            const container = document.getElementById('grams-container');
            const emptyState = document.getElementById('empty-state');
            try {
                const listResult = await window.storage.list('gram:', true);
                const keys = (listResult && listResult.keys) ? listResult.keys : [];

                if (keys.length === 0) {
                    container.innerHTML = '';
                    container.appendChild(emptyState);
                    return;
                }

                const grams = [];
                for (const key of keys) {
                    try {
                        const res = await window.storage.get(key, true);
                        if (res && res.value) grams.push(JSON.parse(res.value));
                    } catch (err) {
                        console.error('Failed to load', key, err);
                    }
                }
                grams.sort((a, b) => b.createdAt - a.createdAt);

                container.innerHTML = '';
                if (grams.length === 0) {
                    container.appendChild(emptyState);
                    return;
                }

                grams.forEach(gram => {
                    const card = document.createElement('div');
                    card.className = 'gram-card';
                    card.style.borderLeftColor = gram.color || '#2c4a6e';

                    const thumbHtml = gram.photo
                        ? `<img class="thumb-preview" src="${gram.photo}" alt="">`
                        : `<div class="thumb-sticker">${gram.sticker || '🦈'}</div>`;

                    card.innerHTML = `
                        ${thumbHtml}
                        <div class="gram-card-content">
                            <div class="gram-title">${escapeHtml(gram.recipient)} 🎂</div>
                            <div class="gram-sender">From ${escapeHtml(gram.sender)}</div>
                            <div class="gram-countdown">${countdownLabel(gram.birthdate)}</div>
                        </div>
                        <div class="card-actions">
                            <button type="button" class="share-btn">Copy</button>
                            <button type="button" class="delete-btn">Delete</button>
                        </div>
                    `;

                    card.querySelector('.gram-card-content').addEventListener('click', () => openGramView(gram));
                    card.querySelector('.thumb-preview, .thumb-sticker')?.addEventListener('click', () => openGramView(gram));
                    card.querySelector('.share-btn').addEventListener('click', (ev) => {
                        ev.stopPropagation();
                        copyGramText(gram);
                    });
                    card.querySelector('.delete-btn').addEventListener('click', (ev) => {
                        ev.stopPropagation();
                        deleteGram(gram.id);
                    });

                    container.appendChild(card);
                });
            } catch (err) {
                console.error('Failed to load grams:', err);
                container.innerHTML = '<p class="empty-state">Couldn\'t load messages right now. Try refreshing.</p>';
            }
        }

        function escapeHtml(str) {
            const div = document.createElement('div');
            div.textContent = str || '';
            return div.innerHTML;
        }

        function countdownLabel(birthdate) {
            if (!birthdate) return '';
            const target = new Date(birthdate + 'T00:00:00');
            const now = new Date();
            const diffMs = target - now;
            if (diffMs <= 0) return '🎉 Today is the day!';
            const days = Math.ceil(diffMs / (1000 * 60 * 60 * 24));
            return days === 1 ? '1 day to go!' : `${days} days to go!`;
        }

        async function copyGramText(gram) {
            const text = `${gram.message}\n\n— ${gram.sender}`;
            try {
                await navigator.clipboard.writeText(text);
                alert('Message copied to clipboard!');
            } catch (err) {
                alert('Could not copy automatically. Here is the message:\n\n' + text);
            }
        }

        async function deleteGram(id) {
            if (!confirm('Delete this message? This cannot be undone.')) return;
            try {
                await window.storage.delete('gram:' + id, true);
                await loadGrams();
            } catch (err) {
                console.error('Delete failed:', err);
                alert('Could not delete this message — please try again.');
            }
        }

        // ---------- Fullscreen gram view ----------
        function openGramView(gram) {
            currentGramId = gram.id;
            document.getElementById('view-title').textContent = `For ${gram.recipient} 🎂`;
            document.getElementById('view-meta').textContent = `From ${gram.sender}`;
            document.getElementById('view-message').textContent = gram.message;
            document.getElementById('view-sticker').textContent = gram.sticker || '🦈';

            const wrapper = document.getElementById('view-canvas-wrapper');
            const photoEl = document.getElementById('view-photo');
            if (gram.photo) {
                photoEl.src = gram.photo;
                wrapper.style.display = 'block';
            } else {
                wrapper.style.display = 'none';
            }

            startCountdown(gram.birthdate);

            document.getElementById('dashboard-view').style.display = 'none';
            document.getElementById('gram-view').style.display = 'block';
            window.scrollTo(0, 0);
        }

        function closeGramView() {
            document.getElementById('gram-view').style.display = 'none';
            document.getElementById('dashboard-view').style.display = 'flex';
            if (countdownIntervalId) clearInterval(countdownIntervalId);
            currentGramId = null;
        }

        function startCountdown(birthdate) {
            if (countdownIntervalId) clearInterval(countdownIntervalId);
            const timerEl = document.getElementById('view-timer');
            function tick() {
                if (!birthdate) { timerEl.textContent = '—'; return; }
                const target = new Date(birthdate + 'T00:00:00');
                const now = new Date();
                const diffMs = target - now;
                if (diffMs <= 0) {
                    timerEl.textContent = "🎉 It's her birthday!";
                    return;
                }
                const days = Math.floor(diffMs / (1000 * 60 * 60 * 24));
                const hours = Math.floor((diffMs / (1000 * 60 * 60)) % 24);
                const mins = Math.floor((diffMs / (1000 * 60)) % 60);
                const secs = Math.floor((diffMs / 1000) % 60);
                timerEl.textContent = `${days}d ${hours}h ${mins}m ${secs}s`;
            }
            tick();
            countdownIntervalId = setInterval(tick, 1000);
        }

        // ---------- Draggable sticker ----------
        (function setupDraggableSticker() {
            const sticker = document.getElementById('view-sticker');
            const wrapper = document.getElementById('view-canvas-wrapper');
            let dragging = false;

            function toLocalPos(clientX, clientY) {
                const rect = wrapper.getBoundingClientRect();
                let x = clientX - rect.left;
                let y = clientY - rect.top;
                x = Math.max(0, Math.min(rect.width, x));
                y = Math.max(0, Math.min(rect.height, y));
                return { x, y };
            }

            function startDrag(clientX, clientY) {
                dragging = true;
                moveTo(clientX, clientY);
            }
            function moveTo(clientX, clientY) {
                if (!dragging) return;
                const { x, y } = toLocalPos(clientX, clientY);
                sticker.style.left = x + 'px';
                sticker.style.top = y + 'px';
            }
            function endDrag() { dragging = false; }

            sticker.addEventListener('mousedown', (e) => startDrag(e.clientX, e.clientY));
            window.addEventListener('mousemove', (e) => moveTo(e.clientX, e.clientY));
            window.addEventListener('mouseup', endDrag);

            sticker.addEventListener('touchstart', (e) => {
                const t = e.touches[0];
                startDrag(t.clientX, t.clientY);
            }, { passive: true });
            window.addEventListener('touchmove', (e) => {
                if (!dragging) return;
                const t = e.touches[0];
                moveTo(t.clientX, t.clientY);
            }, { passive: true });
            window.addEventListener('touchend', endDrag);
        })();

        // ---------- Confetti ----------
        const confettiCanvas = document.getElementById('confetti-canvas');
        const confettiCtx = confettiCanvas.getContext('2d');
        let confettiParticles = [];
        let confettiAnimId = null;

        function resizeConfettiCanvas() {
            confettiCanvas.width = window.innerWidth;
            confettiCanvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeConfettiCanvas);
        resizeConfettiCanvas();

        function burstConfetti() {
            const colors = ['#2c4a6e', '#5a7ca3', '#8a8681', '#c9a24a', '#3c3b38', '#a9c4de'];
            for (let i = 0; i < 120; i++) {
                confettiParticles.push({
                    x: confettiCanvas.width / 2,
                    y: confettiCanvas.height / 3,
                    vx: (Math.random() - 0.5) * 12,
                    vy: Math.random() * -10 - 4,
                    size: Math.random() * 8 + 4,
                    color: colors[Math.floor(Math.random() * colors.length)],
                    rotation: Math.random() * 360,
                    rotSpeed: (Math.random() - 0.5) * 10,
                    life: 0
                });
            }
            if (!confettiAnimId) animateConfetti();
        }

        function animateConfetti() {
            confettiCtx.clearRect(0, 0, confettiCanvas.width, confettiCanvas.height);
            confettiParticles.forEach(p => {
                p.vy += 0.3;
                p.x += p.vx;
                p.y += p.vy;
                p.rotation += p.rotSpeed;
                p.life++;
                confettiCtx.save();
                confettiCtx.translate(p.x, p.y);
                confettiCtx.rotate((p.rotation * Math.PI) / 180);
                confettiCtx.fillStyle = p.color;
                confettiCtx.fillRect(-p.size / 2, -p.size / 2, p.size, p.size);
                confettiCtx.restore();
            });
            confettiParticles = confettiParticles.filter(p => p.y < confettiCanvas.height + 50 && p.life < 400);

            if (confettiParticles.length > 0) {
                confettiAnimId = requestAnimationFrame(animateConfetti);
            } else {
                confettiCtx.clearRect(0, 0, confettiCanvas.width, confettiCanvas.height);
                confettiAnimId = null;
            }
        }

        document.getElementById('cake-btn').addEventListener('click', burstConfetti);

        // ---------- Init ----------
        loadGrams();
    </script>
</body>
</html>
