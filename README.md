<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Secret zone | Анонимный P2P-обмен</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background-color: #000000;
            color: #ffffff;
            /* СТАНДАРТНЫЙ ШРИФТ WINDOWS 10: Segoe UI */
            font-family: 'Segoe UI', 'Tahoma', 'Geneva', 'Verdana', sans-serif;
            line-height: 1.5;
            padding: 1.5rem;
            min-height: 100vh;
            position: relative;
        }

        /* ЛЕВЫЙ ВЕРХНИЙ УГОЛ — название Secret zone */
        .corner-title {
            position: fixed;
            top: 1.2rem;
            left: 1.5rem;
            z-index: 1000;
            font-size: 1.7rem;
            font-weight: 700;
            letter-spacing: -0.3px;
            background: linear-gradient(135deg, #22c55e, #b0ffb0);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            text-shadow: 0 0 8px rgba(34,197,94,0.2);
            pointer-events: none;
            font-family: 'Segoe UI', 'Tahoma', 'Geneva', 'Verdana', sans-serif;
        }

        /* ЛЕВЫЙ НИЖНИЙ УГОЛ — панель создания обсуждения / peer-сервер */
        .peer-panel {
            position: fixed;
            bottom: 1.5rem;
            left: 1.5rem;
            z-index: 1000;
            background: #0a0a0a;
            backdrop-filter: blur(8px);
            border: 1px solid #22c55e33;
            border-radius: 1.5rem;
            padding: 0.9rem 1.2rem;
            width: 280px;
            box-shadow: 0 8px 20px rgba(0,0,0,0.6);
            transition: all 0.2s;
            font-family: 'Segoe UI', 'Tahoma', 'Geneva', 'Verdana', sans-serif;
        }

        .peer-panel h3 {
            font-size: 0.9rem;
            font-weight: 600;
            margin-bottom: 0.6rem;
            color: #22c55e;
            display: flex;
            align-items: center;
            gap: 0.4rem;
            font-family: inherit;
        }

        .peer-panel p {
            font-size: 0.7rem;
            color: #aaa;
            margin-bottom: 0.8rem;
            font-family: inherit;
        }

        .peer-buttons {
            display: flex;
            gap: 0.8rem;
            margin: 0.7rem 0;
        }

        .peer-btn {
            background: #1f1f1f;
            border: none;
            padding: 0.4rem 0.8rem;
            border-radius: 2rem;
            font-size: 0.75rem;
            font-weight: 600;
            cursor: pointer;
            transition: 0.15s;
            color: white;
            flex: 1;
            font-family: inherit;
        }

        .peer-btn.accept {
            background: #22c55e;
            color: black;
        }
        .peer-btn.accept:hover {
            background: #2ecc71;
            transform: scale(0.97);
        }
        .peer-btn.reject {
            background: #3a2a2a;
            border: 1px solid #aa3333;
            color: #ff8888;
        }
        .peer-btn.reject:hover {
            background: #5a2a2a;
        }

        .status-text {
            font-size: 0.7rem;
            margin-top: 0.5rem;
            padding-top: 0.4rem;
            border-top: 1px solid #222;
            color: #aaa;
            word-break: break-word;
            font-family: inherit;
        }

        .main-container {
            max-width: 1100px;
            margin: 0 auto;
            padding-top: 1rem;
            padding-bottom: 5rem;
        }

        .card {
            background: #0a0a0a;
            border-radius: 1.5rem;
            padding: 1.8rem;
            margin-bottom: 2rem;
            border: 1px solid #1f1f1f;
            transition: all 0.2s ease;
        }

        .card:hover {
            border-color: #2a2a2a;
            background: #0c0c0c;
        }

        .upload-area {
            border: 2px dashed #2a2a2a;
            border-radius: 1.2rem;
            padding: 1.8rem;
            text-align: center;
            background: #050505;
            cursor: pointer;
            transition: border 0.2s, background 0.2s;
        }

        .upload-area:hover {
            border-color: #22c55e;
            background: #0c0f0c;
        }

        input[type="file"] {
            display: none;
        }

        .file-label {
            display: inline-block;
            background: #1f1f1f;
            color: white;
            padding: 0.7rem 1.8rem;
            border-radius: 3rem;
            font-weight: 500;
            margin: 1rem 0 0.5rem 0;
            cursor: pointer;
            transition: 0.2s;
            font-family: inherit;
        }

        .file-label:hover {
            background: #2c2c2c;
            color: #22c55e;
        }

        .selected-files {
            margin: 1rem 0 0.8rem;
            font-size: 0.85rem;
            color: #aaa;
            font-family: inherit;
        }

        textarea {
            width: 100%;
            background: #111111;
            border: 1px solid #2a2a2a;
            color: white;
            padding: 1rem;
            border-radius: 1rem;
            font-family: 'Segoe UI', 'Tahoma', 'Geneva', 'Verdana', sans-serif;
            font-size: 0.95rem;
            resize: vertical;
            margin-bottom: 1rem;
        }

        textarea:focus {
            outline: none;
            border-color: #22c55e;
        }

        button {
            background: #22c55e;
            color: black;
            border: none;
            padding: 0.8rem 2rem;
            font-weight: 700;
            border-radius: 3rem;
            font-size: 1rem;
            cursor: pointer;
            transition: 0.2s;
            font-family: inherit;
            width: 100%;
        }

        button:hover {
            background: #1eb154;
            transform: scale(0.98);
        }

        .posts-grid {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
        }

        .post-item {
            background: #0b0b0b;
            border-radius: 1.2rem;
            padding: 1.4rem;
            border-left: 3px solid #22c55e;
            word-break: break-word;
        }

        .post-meta {
            font-size: 0.7rem;
            color: #777;
            margin-bottom: 0.75rem;
            display: flex;
            gap: 1rem;
            flex-wrap: wrap;
            border-bottom: 1px dashed #242424;
            padding-bottom: 0.5rem;
            font-family: inherit;
        }

        .post-text {
            white-space: pre-wrap;
            font-size: 0.95rem;
            background: #050505;
            padding: 0.8rem;
            border-radius: 0.8rem;
            margin-bottom: 0.5rem;
            font-family: inherit;
        }

        .file-link {
            display: inline-flex;
            align-items: center;
            gap: 0.4rem;
            background: #121212;
            padding: 0.5rem 1rem;
            border-radius: 2rem;
            text-decoration: none;
            color: #22c55e;
            font-weight: 500;
            font-size: 0.85rem;
            margin: 0.3rem 0.3rem 0 0;
            transition: 0.15s;
            border: 1px solid #2a2a2a;
            font-family: inherit;
        }

        .file-link:hover {
            background: #1a2a1a;
            color: #4ade80;
        }

        audio, video {
            width: 100%;
            border-radius: 0.8rem;
            margin-top: 0.5rem;
        }

        .badge {
            font-size: 0.7rem;
            background: #1f1f1f;
            padding: 0.2rem 0.6rem;
            border-radius: 2rem;
            color: #ccc;
            font-family: inherit;
        }

        .empty-state {
            text-align: center;
            padding: 2rem;
            color: #6b6b6b;
            font-style: italic;
            font-family: inherit;
        }

        footer {
            text-align: center;
            margin-top: 3rem;
            font-size: 0.7rem;
            color: #444;
            font-family: inherit;
        }

        .cleanup-notice {
            font-size: 0.7rem;
            color: #22c55e;
            background: #0a1a0a;
            display: inline-block;
            padding: 0.2rem 0.6rem;
            border-radius: 2rem;
            margin-left: 0.5rem;
        }

        @media (max-width: 640px) {
            body {
                padding: 1rem;
            }
            .corner-title {
                font-size: 1.2rem;
                top: 0.8rem;
                left: 1rem;
            }
            .peer-panel {
                width: 240px;
                padding: 0.7rem 1rem;
                bottom: 1rem;
                left: 1rem;
            }
        }
    </style>
</head>
<body>

<!-- НАЗВАНИЕ В ЛЕВОМ ВЕРХНЕМ УГЛУ (Secret zone) -->
<div class="corner-title">🔒 Secret zone</div>

<!-- ЛЕВЫЙ НИЖНИЙ УГОЛ: создание обсуждения + роль сервера -->
<div class="peer-panel" id="peerPanel">
    <h3>🌀 P2P-обсуждение</h3>
    <p>Стань сервером — твой ПК сможет принимать участников. При подключении гостя вы сможете анонимно обмениваться сообщениями <span style="color:#22c55e">(демо-симуляция)</span></p>
    <div class="peer-buttons">
        <button id="acceptServerBtn" class="peer-btn accept">✅ Принять</button>
        <button id="rejectServerBtn" class="peer-btn reject">❌ Отказать</button>
    </div>
    <div class="status-text" id="peerStatusText">⚪ Режим: клиент. Нажмите «Принять», чтобы активировать P2P-сервер.</div>
    <div id="incomingRequestDiv" style="display: none; margin-top: 8px; background:#111; border-radius: 14px; padding: 6px;">
        <span style="font-size:0.7rem;">📡 Входящий запрос на обсуждение!</span>
        <div style="display:flex; gap:8px; margin-top:6px;">
            <button id="approveRequestBtn" class="peer-btn accept" style="padding:0.2rem 0.5rem;">Принять чат</button>
            <button id="denyRequestBtn" class="peer-btn reject" style="padding:0.2rem 0.5rem;">Отклонить</button>
        </div>
    </div>
</div>

<div class="main-container">
    <!-- карточка публикации -->
    <div class="card">
        <div style="font-weight: 600; margin-bottom: 1rem;">📝 Анонимная запись <span class="cleanup-notice">🗑️ лента очищается каждые 72 часа</span></div>
        <textarea id="postText" rows="3" placeholder="Напишите что угодно, ссылки становятся зелёными..."></textarea>

        <div class="upload-area" id="uploadArea">
            <div>📎 🎥 🎵 🖼️ — перетащите или нажмите</div>
            <input type="file" id="fileInput" multiple>
            <div class="file-label" onclick="document.getElementById('fileInput').click()">Выбрать файлы</div>
            <div id="fileListPreview" class="selected-files"></div>
            <div style="font-size:0.7rem; color:#555;">до 50 МБ на файл (локально в IndexedDB)</div>
        </div>

        <button id="publishBtn">🌱 Опубликовать анонимно</button>
    </div>

    <!-- лента публикаций -->
    <div style="display: flex; justify-content: space-between; margin-bottom: 1rem;">
        <h2 style="font-size: 1.4rem; font-weight: 500; font-family: 'Segoe UI', 'Tahoma', 'Geneva', 'Verdana', sans-serif;">📰 Анонимная лента</h2>
        <span class="badge" id="postsCount">0 записей</span>
    </div>
    <div id="postsFeed" class="posts-grid">
        <div class="empty-state">✨ Пока пусто. Опубликуйте что-нибудь или создайте обсуждение в левом углу.</div>
    </div>
    <footer>
        🔒 Полная анонимность — данные только в вашем браузере. Secret zone — файлы, текст, аудио, видео.<br>
        🧩 P2P-режим: симуляция серверной роли с диалогом «принять/отказать».<br>
        ⏱️ Автоочистка: все записи старше 72 часов удаляются автоматически.
    </footer>
</div>

<script>
    // ========== 1. IndexedDB для анонимных постов ==========
    const DB_NAME = 'SecretZoneDB';
    const DB_VERSION = 2;          // повысили версию для обновления схемы (очистка старая логика)
    const STORE_NAME = 'anon_posts';
    const CLEANUP_INTERVAL_MS = 72 * 60 * 60 * 1000; // 72 часа в миллисекундах

    let db = null;

    function openDatabase() {
        return new Promise((resolve, reject) => {
            const request = indexedDB.open(DB_NAME, DB_VERSION);
            request.onerror = () => reject('Ошибка открытия БД');
            request.onsuccess = (e) => {
                db = e.target.result;
                resolve(db);
            };
            request.onupgradeneeded = (e) => {
                const dbRef = e.target.result;
                if (!dbRef.objectStoreNames.contains(STORE_NAME)) {
                    const store = dbRef.createObjectStore(STORE_NAME, { keyPath: 'id', autoIncrement: true });
                    store.createIndex('timestamp', 'timestamp');
                } else {
                    // если store уже существует, но нужно убедиться что индекс есть
                    const transaction = e.target.transaction;
                    const store = transaction.objectStore(STORE_NAME);
                    if (!store.indexNames.contains('timestamp')) {
                        store.createIndex('timestamp', 'timestamp');
                    }
                }
            };
        });
    }

    // ФУНКЦИЯ ОЧИСТКИ СТАРЫХ ПОСТОВ (возраст > 72 часов)
    async function deleteOldPosts() {
        if (!db) await openDatabase();
        const now = Date.now();
        const expiryTime = now - CLEANUP_INTERVAL_MS; // все, что старше этой метки — удаляем

        return new Promise((resolve, reject) => {
            const transaction = db.transaction([STORE_NAME], 'readwrite');
            const store = transaction.objectStore(STORE_NAME);
            const index = store.index('timestamp');
            const range = IDBKeyRange.upperBound(expiryTime); // все записи с timestamp <= expiryTime
            const request = index.openCursor(range);

            let deletedCount = 0;
            request.onsuccess = (event) => {
                const cursor = event.target.result;
                if (cursor) {
                    const post = cursor.value;
                    store.delete(post.id);
                    deletedCount++;
                    cursor.continue();
                } else {
                    if (deletedCount > 0) {
                        console.log(`🧹 Очистка: удалено ${deletedCount} старых записей (старше 72 ч)`);
                    }
                    resolve(deletedCount);
                }
            };
            request.onerror = (err) => {
                console.error('Ошибка при очистке', err);
                reject(err);
            };
        });
    }

    async function getAllPosts() {
        if (!db) await openDatabase();
        // сначала удаляем устаревшие (при каждом получении постов - гарантия, что лента свежая)
        await deleteOldPosts();

        return new Promise((resolve) => {
            const tx = db.transaction(STORE_NAME, 'readonly');
            const index = tx.objectStore(STORE_NAME).index('timestamp');
            const cursorReq = index.openCursor(null, 'prev');
            const posts = [];
            cursorReq.onsuccess = (e) => {
                const cursor = e.target.result;
                if (cursor) {
                    posts.push(cursor.value);
                    cursor.continue();
                } else resolve(posts);
            };
        });
    }

    async function savePost(text, fileItems) {
        if (!db) await openDatabase();
        const post = {
            id: Date.now() + Math.random() * 10000,
            text: text || '',
            files: fileItems,
            timestamp: Date.now()
        };
        return new Promise((resolve, reject) => {
            const tx = db.transaction(STORE_NAME, 'readwrite');
            const req = tx.objectStore(STORE_NAME).add(post);
            req.onsuccess = () => resolve(post);
            req.onerror = (err) => reject(err);
        });
    }

    // конвертация файлов в base64
    const MAX_FILE_SIZE_BYTES = 50 * 1024 * 1024;
    function fileToDataURL(file) {
        return new Promise((resolve, reject) => {
            if (file.size > MAX_FILE_SIZE_BYTES) reject(new Error(`Файл ${file.name} > 50МБ`));
            const reader = new FileReader();
            reader.onload = (e) => resolve({ name: file.name, type: file.type, dataURL: e.target.result, size: file.size });
            reader.onerror = reject;
            reader.readAsDataURL(file);
        });
    }

    // ---- UI элементы файлов ----
    const fileInput = document.getElementById('fileInput');
    const uploadArea = document.getElementById('uploadArea');
    const fileListPreview = document.getElementById('fileListPreview');
    const publishBtn = document.getElementById('publishBtn');
    const postTextarea = document.getElementById('postText');
    const postsFeed = document.getElementById('postsFeed');
    const postsCountSpan = document.getElementById('postsCount');
    let selectedFiles = [];

    function updateFilePreview() {
        if (!selectedFiles.length) {
            fileListPreview.innerHTML = '<span style="color:#555;">📭 Файлы не выбраны</span>';
            return;
        }
        const names = selectedFiles.map(f => `${f.name} (${(f.size/1024).toFixed(1)} KB)`).join(', ');
        fileListPreview.innerHTML = `✅ Выбрано: ${names}`;
    }

    fileInput.addEventListener('change', (e) => {
        selectedFiles.push(...Array.from(e.target.files));
        updateFilePreview();
        fileInput.value = '';
    });
    uploadArea.addEventListener('dragover', (e) => e.preventDefault());
    uploadArea.addEventListener('drop', (e) => {
        e.preventDefault();
        selectedFiles.push(...Array.from(e.dataTransfer.files));
        updateFilePreview();
    });
    uploadArea.addEventListener('click', (e) => {
        if (e.target === uploadArea || e.target.parentElement === uploadArea || (e.target.tagName === 'DIV' && e.target.innerText.includes('перетащите'))) 
            fileInput.click();
    });

    function escapeHtml(str) { return str?.replace(/[&<>]/g, function(m){if(m==='&') return '&amp;'; if(m==='<') return '&lt;'; if(m==='>') return '&gt;'; return m;}) || ''; }
    function formatBytes(b){ if(b===0) return '0 B'; const k=1024; const sizes=['B','KB','MB','GB']; const i=Math.floor(Math.log(b)/Math.log(k)); return parseFloat((b/Math.pow(k,i)).toFixed(1))+' '+sizes[i]; }

    async function renderFeed() {
        try {
            const posts = await getAllPosts(); // внутри происходит очистка старых записей
            const count = posts.length;
            postsCountSpan.innerText = `${count} ${count === 1 ? 'запись' : (count < 5 ? 'записи' : 'записей')}`;
            if (!count) { 
                postsFeed.innerHTML = '<div class="empty-state">🌱 Нет публикаций. Будьте первым! (очистка каждые 72ч)</div>'; 
                return; 
            }
            let html = '';
            for (const p of posts) {
                const date = new Date(p.timestamp).toLocaleString();
                let textHtml = escapeHtml(p.text || '');
                const linkedText = textHtml.replace(/(https?:\/\/[^\s]+)/g, '<a href="$1" target="_blank" class="file-link" style="background: none; display: inline; text-decoration: underline; padding:0;">$1</a>');
                let filesHtml = '';
                if (p.files && p.files.length) {
                    for (const f of p.files) {
                        const isImage = f.type?.startsWith('image/');
                        const isAudio = f.type?.startsWith('audio/');
                        const isVideo = f.type?.startsWith('video/');
                        const fname = escapeHtml(f.name);
                        if (isImage) {
                            filesHtml += `<div><a href="${f.dataURL}" class="file-link" download="${fname}" target="_blank">🖼️ ${fname}</a><br><img src="${f.dataURL}" style="max-width:100%; max-height:200px; border-radius:12px; margin-top:6px;"></div>`;
                        } else if (isAudio) {
                            filesHtml += `<div class="file-link">🎵 ${fname}</div><audio controls src="${f.dataURL}"></audio>`;
                        } else if (isVideo) {
                            filesHtml += `<div class="file-link">🎬 ${fname}</div><video controls src="${f.dataURL}" style="max-width:100%; border-radius:12px;"></video>`;
                        } else {
                            filesHtml += `<div><a href="${f.dataURL}" class="file-link" download="${fname}" target="_blank">📎 ${fname} (${formatBytes(f.size || 0)})</a></div>`;
                        }
                    }
                }
                html += `<div class="post-item"><div class="post-meta"><span>🕒 ${date}</span><span>🔒 Аноним</span>${p.files?.length ? `<span>📎 ${p.files.length} файл(ов)</span>` : ''}</div><div class="post-content">${p.text ? `<div class="post-text">${linkedText}</div>` : ''}${filesHtml}</div></div>`;
            }
            postsFeed.innerHTML = html;
        } catch(e) { 
            console.error(e);
            postsFeed.innerHTML = '<div class="empty-state">⚠️ Ошибка загрузки ленты</div>'; 
        }
    }

    // Публикация с последующей очисткой и рендером
    publishBtn.addEventListener('click', async () => {
        const text = postTextarea.value.trim();
        if (!text && !selectedFiles.length) { alert('Добавьте текст или файл'); return; }
        publishBtn.disabled = true;
        publishBtn.textContent = '⏳ Публикация...';
        try {
            const filePayloads = [];
            for (const file of selectedFiles) {
                try { filePayloads.push(await fileToDataURL(file)); } catch(e) { alert(e.message); publishBtn.disabled=false; publishBtn.textContent='🌱 Опубликовать анонимно'; return; }
            }
            await savePost(text, filePayloads);
            postTextarea.value = '';
            selectedFiles = [];
            updateFilePreview();
            // после публикации вызываем рендер (очистит старые записи автоматически)
            await renderFeed();
        } catch(e) { alert('Ошибка сохранения'); } finally { publishBtn.disabled=false; publishBtn.textContent='🌱 Опубликовать анонимно'; }
    });

    // ========== 2. P2P / СИМУЛЯЦИЯ СЕРВЕРА ПК (Принять/Отказать) ==========
    let serverActive = false;
    const acceptBtn = document.getElementById('acceptServerBtn');
    const rejectBtn = document.getElementById('rejectServerBtn');
    const peerStatusSpan = document.getElementById('peerStatusText');
    const incomingDiv = document.getElementById('incomingRequestDiv');
    const approveReqBtn = document.getElementById('approveRequestBtn');
    const denyReqBtn = document.getElementById('denyRequestBtn');

    function hideIncoming() { incomingDiv.style.display = 'none'; }
    function showIncoming() { incomingDiv.style.display = 'block'; }

    function activateServerMode() {
        if (serverActive) return;
        serverActive = true;
        peerStatusSpan.innerHTML = '🟢 РЕЖИМ: СЕРВЕР (ваш ПК — хост обсуждения). Ожидание запросов...<br><span style="color:#8bc34a;">✨ Через 5-10 сек может поступить входящий запрос (симуляция).</span>';
        setTimeout(() => {
            if (serverActive) {
                showIncoming();
                peerStatusSpan.innerHTML += `<br><span style="color:#ffb347;">📨 Входящее предложение обсуждения! Нажмите принять/отказать.</span>`;
            }
        }, 5000 + Math.random() * 5000);
    }

    function deactivateServerMode() {
        serverActive = false;
        hideIncoming();
        peerStatusSpan.innerHTML = '⚫ Режим: клиент (обсуждения не активны). Нажмите «Принять», чтобы стать сервером.';
    }

    function acceptIncomingChat() {
        if (!serverActive) {
            alert('Серверный режим не активен');
            hideIncoming();
            return;
        }
        peerStatusSpan.innerHTML = '💬 P2P-обсуждение установлено! Вы общаетесь анонимно (симуляция). Чтобы завершить диалог, отключите сервер.';
        hideIncoming();
        const userMessage = prompt('💬 P2P чат открыт! Введите тестовое сообщение (симуляция обмена):');
        if (userMessage && userMessage.trim()) {
            alert(`📡 Собеседник (симуляция) получил: "${userMessage}"\nОтвет: "Анонимно принято, Secret zone!"`);
        }
    }

    function rejectIncomingChat() {
        if (!serverActive) return;
        peerStatusSpan.innerHTML = '❌ Запрос на обсуждение отклонён. Вы остаётесь в режиме сервера.';
        hideIncoming();
        setTimeout(() => {
            if(serverActive) peerStatusSpan.innerHTML = '🟢 РЕЖИМ: СЕРВЕР (ваш ПК — хост). Ожидание запросов...';
        }, 2000);
    }

    acceptBtn.addEventListener('click', () => {
        if (serverActive) {
            alert('Серверный режим уже активен. Сначала нажмите «Отказать».');
            return;
        }
        activateServerMode();
    });

    rejectBtn.addEventListener('click', () => {
        if (!serverActive) {
            alert('Серверный режим не активен.');
            return;
        }
        deactivateServerMode();
    });

    approveReqBtn.addEventListener('click', acceptIncomingChat);
    denyReqBtn.addEventListener('click', rejectIncomingChat);

    // ========== 3. Периодическая очистка каждые 72 часа (запускаем также при старте) ==========
    // Функция запускается при загрузке, очищает старые записи, затем рендерит
    async function initialCleanupAndRender() {
        try {
            await openDatabase();
            await deleteOldPosts();   // принудительная очистка
            await renderFeed();
        } catch(e) {
            console.warn(e);
            postsFeed.innerHTML = '<div class="empty-state">⚠️ Ошибка хранилища. Возможно, приватный режим.</div>';
        }
    }

    // Запускаем периодическую проверку (каждый час, чтобы удалять накопившиеся старые посты, но основная очистка внутри getAllPosts)
    setInterval(async () => {
        if (db) {
            const deleted = await deleteOldPosts();
            if (deleted > 0) {
                await renderFeed();
            }
        }
    }, 60 * 60 * 1000); // проверка каждый час, но не чаще, чем нужно

    initialCleanupAndRender();
</script>
</body>
</html>
