# Statistics_Que
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Stats Adaptor Pro - Muji Style</title>
    
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700&display=swap" rel="stylesheet">
    
    <script src="https://cdn.tailwindcss.com"></script>
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css">
    <script src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        muji: {
                            bg: '#F7F7F5',       /* 米白色背景 */
                            card: '#FFFFFF',     /* 純白卡片 */
                            text: '#333333',     /* 深灰文字 */
                            sub: '#7F7F7F',      /* 輔助文字 */
                            accent: '#7A9E9F',   /* 低飽和墨綠/藍 */
                            accentHover: '#5F7E7F',
                            highlight: '#D67C7C', /* 柔和紅 (重點標示) */
                            border: '#E0E0E0',
                            danger: '#E06C75'    /* 刪除按鈕紅 */
                        }
                    },
                    fontFamily: {
                        sans: ['"Noto Sans TC"', 'sans-serif'],
                    },
                    boxShadow: {
                        'muji': '0 2px 8px rgba(0, 0, 0, 0.04)',
                    }
                }
            }
        }
    </script>

    <style>
        body {
            background-color: #F7F7F5;
            -webkit-font-smoothing: antialiased;
        }
        
        /* 圖片貼上區域樣式 */
        .paste-area {
            transition: all 0.3s ease;
            border: 2px dashed #E0E0E0;
        }
        .paste-area:focus, .paste-area.active {
            border-color: #7A9E9F;
            background-color: rgba(122, 158, 159, 0.05);
            outline: none;
        }

        /* KaTeX 顯示優化 */
        .katex-display {
            overflow-x: auto;
            overflow-y: hidden;
            padding: 0.5rem 0;
        }

        /* 重點標示 */
        .highlight-stat {
            color: #D67C7C;
            font-weight: 500;
            background-color: rgba(214, 124, 124, 0.1);
            padding: 0 4px;
            border-radius: 4px;
        }
        
        /* Loading 動畫 */
        .loader {
            border: 3px solid #f3f3f3;
            border-radius: 50%;
            border-top: 3px solid #7A9E9F;
            width: 24px;
            height: 24px;
            -webkit-animation: spin 1s linear infinite;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body class="text-muji-text pb-24">

    <header class="fixed top-0 left-0 right-0 bg-muji-bg/90 backdrop-blur-md z-20 border-b border-muji-border h-14 flex items-center justify-center shadow-sm">
        <h1 class="text-lg font-bold tracking-widest text-muji-text">統計學題目改編系統 Pro</h1>
    </header>

    <main class="container mx-auto px-4 mt-20 max-w-2xl">
        
        <section id="tab-question" class="tab-section transition-opacity duration-300">
            <div class="bg-muji-card rounded-xl p-6 shadow-muji mb-4">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-xl font-medium flex items-center">
                        <i class="fas fa-file-invoice mr-3 text-muji-accent"></i>題目輸入
                    </h2>
                    <button onclick="clearQuestionData()" class="text-xs text-muji-danger border border-muji-danger px-3 py-1 rounded hover:bg-muji-danger hover:text-white transition-colors">
                        <i class="fas fa-trash-alt mr-1"></i>重置/刪除
                    </button>
                </div>
                
                <div class="mb-4">
                    <label class="block text-sm text-muji-sub mb-2">上傳檔案 (圖片/PDF/TXT)</label>
                    <input type="file" id="q-file-input" class="block w-full text-sm text-muji-sub file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-muji-bg file:text-muji-accent hover:file:bg-gray-200 cursor-pointer"/>
                </div>

                <div id="q-paste-area" class="paste-area rounded-lg p-6 text-center cursor-text mb-4" contenteditable="true">
                    <p class="text-muji-sub pointer-events-none select-none">
                        <i class="fas fa-paste mb-2 text-xl"></i><br>
                        點擊此處並按下 <span class="font-bold">Ctrl+V</span> 貼上圖片
                    </p>
                </div>

                <div class="mb-4">
                    <label class="block text-sm text-muji-sub mb-2">題目敘述文字</label>
                    <textarea id="q-text-input" rows="5" class="w-full p-3 rounded-lg border border-muji-border bg-muji-bg focus:outline-none focus:border-muji-accent transition-colors" placeholder="請輸入題目敘述..."></textarea>
                </div>
                
                <div class="mb-4">
                    <label class="block text-sm font-bold text-muji-accent mb-2">改編重點/新增概念 <i class="fas fa-magic ml-1"></i></label>
                    <textarea id="q-focus-input" rows="3" class="w-full p-3 rounded-lg border border-muji-border bg-muji-bg focus:outline-none focus:border-muji-highlight transition-colors" placeholder="例如: 將單樣本 t 檢定改編為雙樣本；要求計算檢定力 (Power)；或將迴歸變數替換為其他經濟概念。"></textarea>
                </div>

                <button onclick="saveQuestion()" class="w-full bg-muji-accent hover:bg-muji-accentHover text-white font-medium py-3 rounded-lg shadow-sm transition-colors">
                    <i class="fas fa-save mr-2"></i>儲存題目與改編重點
                </button>
            </div>

            <div id="q-display-card" class="hidden bg-muji-card rounded-xl p-6 shadow-muji border-l-4 border-muji-accent">
                <div class="flex justify-between items-center mb-4 cursor-pointer" onclick="toggleVisibility('q-content-wrapper', 'q-toggle-icon')">
                    <h3 class="font-medium text-lg">原始題目預覽</h3>
                    <i id="q-toggle-icon" class="fas fa-chevron-down text-muji-sub transition-transform"></i>
                </div>
                <div id="q-content-wrapper" class="hidden">
                    <div id="q-image-preview" class="mb-4 hidden">
                        <img src="" alt="Question Image" class="rounded-lg max-h-60 mx-auto object-contain border border-muji-border">
                    </div>
                    <div id="q-text-display" class="prose prose-sm text-muji-text bg-muji-bg p-4 rounded-lg whitespace-pre-wrap leading-relaxed"></div>
                    <div id="q-focus-display" class="text-sm text-muji-highlight mt-4 border-t border-muji-border pt-2"></div>
                </div>
            </div>
        </section>

        <section id="tab-answer" class="tab-section hidden transition-opacity duration-300">
            <div class="bg-muji-card rounded-xl p-6 shadow-muji mb-4">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-xl font-medium flex items-center">
                        <i class="fas fa-pen-nib mr-3 text-muji-accent"></i>解答輸入
                    </h2>
                    <button onclick="clearAnswerData()" class="text-xs text-muji-danger border border-muji-danger px-3 py-1 rounded hover:bg-muji-danger hover:text-white transition-colors">
                        <i class="fas fa-trash-alt mr-1"></i>重置/刪除
                    </button>
                </div>
                
                <div class="mb-4">
                    <label class="block text-sm text-muji-sub mb-2">上傳解答圖片</label>
                    <input type="file" id="a-file-input" class="block w-full text-sm text-muji-sub file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-muji-bg file:text-muji-accent hover:file:bg-gray-200 cursor-pointer"/>
                </div>
                
                <div id="a-paste-area" class="paste-area rounded-lg p-6 text-center cursor-text mb-4" contenteditable="true">
                    <p class="text-muji-sub pointer-events-none select-none">
                        <i class="fas fa-paste mb-2 text-xl"></i><br>
                        點擊此處並按下 <span class="font-bold">Ctrl+V</span> 貼上圖片
                    </p>
                </div>
                
                <div class="mb-4">
                    <label class="block text-sm text-muji-sub mb-2">解答詳解 (支援 LaTeX: $$公式$$)</label>
                    <textarea id="a-text-input" rows="6" class="w-full p-3 rounded-lg border border-muji-border bg-muji-bg focus:outline-none focus:border-muji-accent transition-colors" placeholder="輸入解答... 例如: 平均數 $$\bar{X} = \frac{\sum X}{n}$$"></textarea>
                </div>

                <button onclick="saveAnswer()" class="w-full bg-muji-accent hover:bg-muji-accentHover text-white font-medium py-3 rounded-lg shadow-sm transition-colors">
                    <i class="fas fa-save mr-2"></i>儲存解答與公式
                </button>
            </div>

            <div id="a-display-card" class="hidden bg-muji-card rounded-xl p-6 shadow-muji border-l-4 border-muji-highlight">
                <div class="flex justify-between items-center mb-4 cursor-pointer" onclick="toggleVisibility('a-content-wrapper', 'a-toggle-icon')">
                    <h3 class="font-medium text-lg">原始解答預覽</h3>
                    <i id="a-toggle-icon" class="fas fa-chevron-down text-muji-sub transition-transform"></i>
                </div>
                <div id="a-content-wrapper" class="hidden">
                     <div id="a-image-preview" class="mb-4 hidden">
                        <img src="" alt="Answer Image" class="rounded-lg max-h-60 mx-auto object-contain border border-muji-border">
                    </div>
                    <div id="a-text-display" class="prose prose-sm text-muji-text bg-muji-bg p-4 rounded-lg whitespace-pre-wrap leading-relaxed"></div>
                </div>
            </div>
        </section>

        <section id="tab-adapt" class="tab-section hidden transition-opacity duration-300">
            <div class="bg-muji-card rounded-xl p-6 shadow-muji mb-6 text-center">
                <h2 class="text-xl font-medium mb-2">題目改編生成器</h2>
                <p class="text-sm text-muji-sub mb-6">系統將依據您輸入的題目內容及**改編重點**，生成 **MCQ (長敘述), Short Answer (多題), FRQ (含公式推導)** 等題型。</p>
                
                <button onclick="generateQuestions()" class="w-full bg-gray-800 hover:bg-gray-700 text-white font-medium py-4 rounded-lg shadow-lg transform transition active:scale-95 flex justify-center items-center">
                    <span id="gen-btn-text"><i class="fas fa-magic mr-2"></i>開始生成改編題目</span>
                    <div id="gen-loader" class="loader ml-3 hidden"></div>
                </button>
            </div>

            <div id="generation-status" class="hidden text-center text-muji-accent font-medium mb-4">
                <i class="fas fa-check-circle mr-2"></i>題目生成完畢！請至「清單」查看。
            </div>
        </section>

        <section id="tab-list" class="tab-section hidden transition-opacity duration-300">
            
            <div class="mb-8">
                <div class="flex justify-between items-center mb-4 border-b border-muji-border pb-2">
                    <h2 class="text-lg font-bold text-muji-text">
                        <i class="fas fa-list-ol mr-2"></i>改編題目清單
                    </h2>
                    <span class="text-xs text-muji-sub" id="question-count-badge">0 題</span>
                </div>
                
                <div id="adapted-list-container" class="space-y-6">
                    <div class="text-center text-muji-sub py-8 bg-muji-card rounded-lg border border-dashed border-muji-border">
                        尚無生成的題目
                    </div>
                </div>
            </div>

            <div class="bg-muji-card rounded-xl p-6 shadow-muji mt-8">
                <h2 class="text-lg font-bold text-muji-text mb-4 border-b border-muji-border pb-2">
                    <i class="fas fa-sticky-note mr-2"></i>個人備忘錄
                </h2>
                <textarea id="memo-input" class="w-full p-3 rounded-lg border border-muji-border bg-muji-bg mb-3 focus:outline-none focus:border-muji-accent" rows="3" placeholder="輸入備忘錄或網址..."></textarea>
                <button onclick="saveMemo()" class="bg-muji-accent text-white px-4 py-2 rounded-lg text-sm mb-4">更新備忘錄</button>
                <div id="memo-display" class="text-sm text-muji-text bg-[#FFFFF0] p-4 rounded-lg shadow-inner whitespace-pre-wrap"></div>
            </div>
        </section>

        <section id="tab-info" class="tab-section hidden transition-opacity duration-300">
            <div class="bg-muji-card rounded-xl p-6 shadow-muji mb-4">
                <h2 class="text-lg font-bold text-muji-text mb-4">
                    <i class="fas fa-info-circle mr-2"></i>來源資訊
                </h2>
                
                <label class="block text-sm text-muji-sub mb-2">出處網址</label>
                <div class="flex gap-2 mb-4">
                    <input type="text" id="source-url-input" class="flex-1 p-2 rounded-lg border border-muji-border bg-muji-bg text-sm" placeholder="https://example.com">
                    <button onclick="saveSourceUrl()" class="bg-muji-accent text-white px-4 rounded-lg text-sm">儲存</button>
                </div>
                <div id="source-url-display" class="mb-6"></div>

                <h3 class="text-md font-medium text-muji-text mb-2">原始題目參考</h3>
                <div id="info-ref-question" class="text-sm text-muji-sub bg-muji-bg p-4 rounded-lg h-40 overflow-y-auto border border-muji-border">
                    暫無資料
                </div>
            </div>
        </section>

    </main>

    <nav class="fixed bottom-0 left-0 right-0 bg-white border-t border-muji-border flex justify-around items-center h-16 z-30 shadow-[0_-2px_10px_rgba(0,0,0,0.03)] pb-safe">
        <button onclick="switchTab('question')" class="nav-btn active flex flex-col items-center justify-center w-full h-full text-muji-sub hover:text-muji-accent transition-colors">
            <i class="fas fa-file-upload text-xl mb-1"></i>
            <span class="text-[10px] font-medium">題目</span>
        </button>
        <button onclick="switchTab('answer')" class="nav-btn flex flex-col items-center justify-center w-full h-full text-muji-sub hover:text-muji-accent transition-colors">
            <i class="fas fa-check-double text-xl mb-1"></i>
            <span class="text-[10px] font-medium">解答</span>
        </button>
        <button onclick="switchTab('adapt')" class="nav-btn flex flex-col items-center justify-center w-full h-full text-muji-sub hover:text-muji-accent transition-colors">
            <i class="fas fa-sync-alt text-xl mb-1"></i>
            <span class="text-[10px] font-medium">改編</span>
        </button>
        <button onclick="switchTab('list')" class="nav-btn flex flex-col items-center justify-center w-full h-full text-muji-sub hover:text-muji-accent transition-colors">
            <i class="fas fa-th-list text-xl mb-1"></i>
            <span class="text-[10px] font-medium">清單</span>
        </button>
        <button onclick="switchTab('info')" class="nav-btn flex flex-col items-center justify-center w-full h-full text-muji-sub hover:text-muji-accent transition-colors">
            <i class="fas fa-info text-xl mb-1"></i>
            <span class="text-[10px] font-medium">資訊</span>
        </button>
    </nav>

    <script>
        // --- 1. State Management & Initialization ---
        const DB_KEY = 'stats_app_data_v4'; // Version update for focus text and copy
        let appData = {
            question: { text: '', img: null, focus: '' }, // Added 'focus'
            answer: { text: '', img: null },
            generated: [],
            memo: '',
            sourceUrl: ''
        };

        document.addEventListener('DOMContentLoaded', () => {
            loadData();
            setupPasteHandlers('q-paste-area', 'question'); // Setup question paste
            setupPasteHandlers('a-paste-area', 'answer');   // Setup answer paste
            renderAll();
        });

        function saveData() {
            try {
                localStorage.setItem(DB_KEY, JSON.stringify(appData));
            } catch (e) {
                alert('儲存失敗：資料量可能過大 (LocalStorage 限制約 5MB)。請嘗試刪除舊圖片或縮小圖片。');
            }
        }

        function loadData() {
            const raw = localStorage.getItem(DB_KEY);
            if (raw) {
                appData = JSON.parse(raw);
                // Ensure 'focus' field exists on load from older versions
                if (!appData.question.focus) appData.question.focus = '';
            }
        }

        // --- 2. Navigation & UI Logic ---
        function switchTab(tabId) {
            document.querySelectorAll('.tab-section').forEach(el => el.classList.add('hidden'));
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');
            
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('text-muji-accent', 'text-muji-sub');
                if(btn.getAttribute('onclick').includes(tabId)) {
                    btn.classList.add('text-muji-accent');
                } else {
                    btn.classList.add('text-muji-sub');
                }
            });

            if(tabId === 'list') renderList();
            if(tabId === 'info') renderInfo();
            if(['question', 'answer', 'list'].includes(tabId)) {
                setTimeout(renderMath, 100); 
            }
        }

        function toggleVisibility(contentId, iconId) {
            const content = document.getElementById(contentId);
            const icon = document.getElementById(iconId);
            if (content.classList.contains('hidden')) {
                content.classList.remove('hidden');
                icon.classList.remove('fa-chevron-down');
                icon.classList.add('fa-chevron-up');
                renderMath();
            } else {
                content.classList.add('hidden');
                icon.classList.remove('fa-chevron-up');
                icon.classList.add('fa-chevron-down');
            }
        }

        // --- 3. Input Handling (Text, Files, Paste, Reset) ---
        
        function fileToBase64(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = () => resolve(reader.result);
                reader.onerror = error => reject(error);
                reader.readAsDataURL(file);
            });
        }

        /**
         * 設置貼上處理器 (通用於題目和解答)
         * @param {string} areaId 貼上區域的 ID
         * @param {string} dataType 要更新的 appData 欄位 ('question' 或 'answer')
         */
        function setupPasteHandlers(areaId, dataType) {
            const pasteArea = document.getElementById(areaId);
            
            pasteArea.addEventListener('paste', (e) => {
                e.preventDefault();
                const items = (e.clipboardData || e.originalEvent.clipboardData).items;
                for (let index in items) {
                    const item = items[index];
                    if (item.kind === 'file' && item.type.includes('image')) {
                        const blob = item.getAsFile();
                        fileToBase64(blob).then(base64 => {
                            // 動態設置 appData 屬性
                            if (dataType === 'question') appData.question.img = base64;
                            else if (dataType === 'answer') appData.answer.img = base64;
                            
                            // Visual Feedback
                            pasteArea.innerHTML = `<p class="text-muji-accent"><i class="fas fa-check-circle mb-2"></i><br>圖片已貼上</p>`;
                            setTimeout(() => {
                                pasteArea.innerHTML = `<p class="text-muji-sub pointer-events-none select-none"><i class="fas fa-paste mb-2 text-xl"></i><br>點擊此處並按下 <span class="font-bold">Ctrl+V</span> 貼上圖片</p>`;
                            }, 2000);
                        });
                    }
                }
            });

            // Prevent typing in paste area
            pasteArea.addEventListener('keydown', (e) => {
                if((e.ctrlKey || e.metaKey) && e.key === 'v') return;
                e.preventDefault();
            });
        }


        async function saveQuestion() {
            const text = document.getElementById('q-text-input').value;
            const focus = document.getElementById('q-focus-input').value; // Get focus text
            const fileInput = document.getElementById('q-file-input');
            
            appData.question.text = text;
            appData.question.focus = focus; // Save focus text
            if (fileInput.files.length > 0) {
                appData.question.img = await fileToBase64(fileInput.files[0]);
            }
            saveData();
            renderQuestionView();
            alert('題目與改編重點已儲存');
        }

        function clearQuestionData() {
            if(confirm('確定要刪除所有「題目」資料（含圖片、文字與改編重點）並重置嗎？')) {
                appData.question = { text: '', img: null, focus: '' };
                document.getElementById('q-file-input').value = '';
                document.getElementById('q-text-input').value = '';
                document.getElementById('q-focus-input').value = ''; // Clear focus input
                saveData();
                renderQuestionView();
            }
        }

        async function saveAnswer() {
            const text = document.getElementById('a-text-input').value;
            const fileInput = document.getElementById('a-file-input');
            
            appData.answer.text = text;
            if (fileInput.files.length > 0) {
                appData.answer.img = await fileToBase64(fileInput.files[0]);
            }
            saveData();
            renderAnswerView();
            alert('解答已儲存');
        }

        function clearAnswerData() {
            if(confirm('確定要刪除所有「解答」資料（含圖片與文字）並重置嗎？')) {
                appData.answer = { text: '', img: null };
                document.getElementById('a-file-input').value = '';
                document.getElementById('a-text-input').value = '';
                saveData();
                renderAnswerView();
            }
        }
        
        // --- 4. Rendering & Highlighting ---

        function highlightKeywords(text) {
            if(!text) return '';
            const regex = /((\b(mean|sd|standard deviation|variance|sample size|alpha|p-value|hypothesis|confidence interval|empirical rule|regression|coefficient|correlation|ANOVA|F-test)\b)|(\b(H_0|H_a|H_1)\b)|([\u0370-\u03FF]))/gi;
            const numStatRegex = /\b([a-zA-Z]+\s*[=<>]\s*\d+(\.\d+)?)\b/g;

            let formatted = text
                .replace(/</g, "&lt;").replace(/>/g, "&gt;")
                .replace(regex, '<span class="highlight-stat">$1</span>')
                .replace(numStatRegex, '<span class="highlight-stat">$1</span>');
            return formatted;
        }

        function renderQuestionView() {
            const displayCard = document.getElementById('q-display-card');
            const imgPreview = document.getElementById('q-image-preview');
            const img = imgPreview.querySelector('img');
            const textDisplay = document.getElementById('q-text-display');
            const focusDisplay = document.getElementById('q-focus-display');
            
            document.getElementById('q-text-input').value = appData.question.text || '';
            document.getElementById('q-focus-input').value = appData.question.focus || '';

            if (appData.question.img || appData.question.text || appData.question.focus) {
                displayCard.classList.remove('hidden');
                
                if (appData.question.img) {
                    img.src = appData.question.img;
                    imgPreview.classList.remove('hidden');
                } else {
                    imgPreview.classList.add('hidden');
                }
                
                textDisplay.innerHTML = highlightKeywords(appData.question.text);
                
                if (appData.question.focus) {
                    focusDisplay.innerHTML = `<i class="fas fa-lightbulb mr-1"></i> **改編重點：** ${appData.question.focus}`;
                } else {
                    focusDisplay.innerHTML = '';
                }

            } else {
                displayCard.classList.add('hidden');
            }
        }

        function renderAnswerView() {
            // ... (Answer rendering logic remains the same)
            const displayCard = document.getElementById('a-display-card');
            const imgPreview = document.getElementById('a-image-preview');
            const img = imgPreview.querySelector('img');
            const textDisplay = document.getElementById('a-text-display');
            
            document.getElementById('a-text-input').value = appData.answer.text || '';

            if (appData.answer.img || appData.answer.text) {
                displayCard.classList.remove('hidden');
                if (appData.answer.img) {
                    img.src = appData.answer.img;
                    imgPreview.classList.remove('hidden');
                } else {
                    imgPreview.classList.add('hidden');
                }
                textDisplay.innerHTML = appData.answer.text; 
                renderMath();
            } else {
                displayCard.classList.add('hidden');
            }
        }

        function renderMath() {
            renderMathInElement(document.body, {
                delimiters: [
                    {left: '$$', right: '$$', display: true},
                    {left: '$', right: '$', display: false}
                ],
                throwOnError : false
            });
        }
        
        // --- 5. Copy Functionality ---

        function copyToClipboard(text) {
            navigator.clipboard.writeText(text).then(() => {
                alert('已成功複製到剪貼簿！');
            }).catch(err => {
                console.error('Copy failed: ', err);
                alert('複製失敗，請檢查瀏覽器權限。');
            });
        }

        // --- 6. Enhanced Mock AI Generation Logic ---

        function generateQuestions() {
            const loader = document.getElementById('gen-loader');
            const btnText = document.getElementById('gen-btn-text');
            const status = document.getElementById('generation-status');

            loader.classList.remove('hidden');
            btnText.textContent = "改編運算中...";
            
            // 獲取輸入和改編重點
            const originalText = appData.question.text || "A survey found the average salary of $n=50$ managers is $\\bar{x}=75,000$ with $s=12,000$.";
            const focusText = appData.question.focus || "改編為檢定比例 (Proportion) 的問題並要求計算 p 值。";
            const mockNum = (originalText.match(/\d+/) || ['60'])[0]; 

            setTimeout(() => {
                let newQuestions = [];

                // --- 1. MCQ with Long Description & Options (Focus: Proportion Testing) ---
                const mcq_en_raw = `The original problem provided a mean salary of $75,000. ${focusText.includes('比例') ? 'Adapting this to a proportion test, suppose a consulting firm claims that more than 60% of managers receive annual bonuses. In a sample of $n=${mockNum}$, only 35 received bonuses. Which statement regarding the p-value calculation is correct?' : 'Based on the original mean $\\bar{x}=75,000$, if the population standard deviation $\\sigma=12,000$ is known, which of the following statements about the test statistic is the most precise conclusion based on the Z-test for a large sample?'} `;
                
                const mcq_zh_raw = `原問題提供了平均薪資 $75,000。${focusText.includes('比例') ? '改編為比例檢定：某顧問公司聲稱超過 60% 的經理獲得年終獎金。在 $n=${mockNum}$ 個樣本中，只有 35 位獲得。關於此檢定的 p 值計算，哪個敘述是正確的？' : '基於原平均數 $\\bar{x}=75,000$，若已知母體標準差 $\\sigma=12,000$，下列關於大樣本 Z 檢定統計量的敘述中，哪項結論最為精確？'} `;
                
                newQuestions.push({
                    type: 'MCQ',
                    title: 'Multiple Choice Question (Adaptation: Proportion Test)',
                    en_raw: mcq_en_raw,
                    zh_raw: mcq_zh_raw,
                    en: mcq_en_raw, // Rendered version (same as raw here for simplicity)
                    zh: mcq_zh_raw, // Rendered version (same as raw here for simplicity)
                    options: [
                        `A. The sample proportion $\\hat{p} = 35/${mockNum} \\approx ${(35/mockNum).toFixed(2)}$, which is greater than $p_0=0.60$.`,
                        `B. The test statistic is calculated as $Z = \\frac{\\hat{p} - p_0}{\\sqrt{p_0 q_0 / n}}$, which yields a Z-value of approximately $Z=-0.12$.`,
                        `C. The p-value for the one-tailed test is $P(Z < -0.12)$, which is a large value, leading to the rejection of $H_0$.`,
                        `D. Since $n \\cdot p_0 = ${Math.round(mockNum * 0.6)} < 10$, the normal approximation for the proportion test is invalid.`
                    ],
                    ans: 'A',
                    exp_raw: `**1. Hypotheses:** $H_0: p = 0.60, H_a: p > 0.60$. \n**2. Sample Proportion:** $\\hat{p} = 35 / ${mockNum} \\approx ${(35/mockNum).toFixed(2)}$. \n**3. Test Statistic Calculation:** $Z = \\frac{\\hat{p} - p_0}{\\sqrt{p_0 q_0 / n}} = \\frac{${(35/mockNum).toFixed(2)} - 0.60}{\\sqrt{0.60 \\cdot 0.40 / ${mockNum}}} \\approx -0.12$ (using ${mockNum}=60 as base).\n**Conclusion:** Only statement A correctly identifies the sample proportion.`
                });

                // --- 2. Short Answer (Adaptation: Calculating Power) ---
                const sa1_en_raw = `Given the original problem (mean salary $75,000$, $\\sigma=12,000$, $n=50$), suppose the true population mean is $\\mu=78,000$. If $\\alpha=0.05$ (two-tailed), calculate the effect size ($d$). ${focusText.includes('功效') ? 'Then, describe how increasing the sample size $n$ would impact the power (檢定力) of the test.' : ''}`;
                const sa1_zh_raw = `基於原問題 (平均薪資 $75,000$, $\\sigma=12,000$, $n=50$)，假設真實母體平均數為 $\\mu=78,000$。若 $\\alpha=0.05$ (雙尾)，請計算效果量 ($d$)。${focusText.includes('功效') ? '接著，請描述增加樣本數 $n$ 會如何影響檢定力 (Power)。' : ''}`;
                
                newQuestions.push({
                    type: 'Short Answer',
                    title: 'Short Answer 1: Effect Size and Power (Focus: Power)',
                    en_raw: sa1_en_raw,
                    zh_raw: sa1_zh_raw,
                    ans: '0.25',
                    exp_raw: `**1. Calculation of Effect Size ($d$):**\n$$d = \\frac{|\\mu_1 - \\mu_0|}{\\sigma} = \\frac{|78,000 - 75,000|}{12,000} = \\frac{3,000}{12,000} = 0.25$$\n**2. Power Impact:**\n增加樣本數 $n$ 會減少標準誤 $SE = \\sigma / \\sqrt{n}$，從而增加檢定的**功效 (Power)**，因為這使得拒絕區和不拒絕區的分離更明顯。`
                });

                newQuestions.push({
                    type: 'Short Answer',
                    title: 'Short Answer 2: Interpretation of Regression Output',
                    en_raw: `A regression analysis shows the R-square value is $R^2=0.81$. What percentage of the total variation in the dependent variable is explained by the independent variable?`,
                    zh_raw: `一項迴歸分析顯示 $R^2=0.81$。請問自變數解釋了應變數總變異的百分之幾？`,
                    ans: '81%',
                    exp_raw: `R-square ($R^2$) represents the proportion of the variation in the dependent variable that is predictable from the independent variable(s). Thus, $0.81 \\times 100\\% = 81\\%$.`
                });
                
                // --- 3. FRQ (At least 3 questions with formulas) ---
                
                // FRQ 1: Double Sample T-Test (Focus: Two Sample)
                const frq1_en_raw = `**Adapted to Two Samples:** The original sample data comes from Company A ($n_A=50, \\bar{x}_A=75,000, s_A=12,000$). Now, we also have data from Company B ($n_B=40, \\bar{x}_B=72,000, s_B=10,000$). Conduct a pooled t-test to determine if Company A's mean salary is significantly higher than Company B's at $\\alpha=0.01$. Provide the null hypothesis and the formula for the test statistic.`;
                const frq1_zh_raw = `**改編為雙樣本檢定：** 原樣本數據來自 A 公司 ($n_A=50, \\bar{x}_A=75,000, s_A=12,000$)。現有 B 公司數據 ($n_B=40, \\bar{x}_B=72,000, s_B=10,000$)。請進行**合併 (pooled) t 檢定**，以判斷 A 公司的平均薪資是否顯著高於 B 公司 ($\\alpha=0.01$)。請提供虛無假設和檢定統計量的公式。`;

                newQuestions.push({
                    type: 'FRQ',
                    title: 'FRQ 1: Two-Sample Hypothesis Testing (Focus: Pooled t-test)',
                    en_raw: frq1_en_raw,
                    zh_raw: frq1_zh_raw,
                    ans: 'See Explanation',
                    exp_raw: `**1. Hypotheses:**\n$$H_0: \\mu_A = \\mu_B$$\n$$H_a: \\mu_A > \\mu_B$$\n\n**2. Pooled Test Statistic Formula:**\n$$t = \\frac{(\\bar{x}_A - \\bar{x}_B) - D_0}{s_p \\sqrt{\\frac{1}{n_A} + \\frac{1}{n_B}}}$$\n\n其中，合併標準差 $s_p$ 的公式為：\n$$s_p^2 = \\frac{(n_A - 1)s_A^2 + (n_B - 1)s_B^2}{n_A + n_B - 2}$$\n\n**3. Required Calculation (Example):** (Skipped for brevity, focus on formula/structure)`
                });

                // FRQ 2: Combinatorics and Listing Possibilities
                const frq2_en_raw = `A fund manager is selecting 3 stocks from a pool of 5 large-cap and 4 mid-cap stocks. List all possible combinations of selecting 2 large-cap stocks and 1 mid-cap stock. Then, calculate the total number of ways to make this selection.`;
                const frq2_zh_raw = `一位基金經理人要從 5 支大型股和 4 支中型股中選出 3 支股票。請列出選出 2 支大型股和 1 支中型股的所有可能組合（使用 L1-L5 代表大型股，M1-M4 代表中型股）。接著，計算總共有多少種不同的選股方式。`;

                newQuestions.push({
                    type: 'FRQ',
                    title: 'FRQ 2: Combinatorics and Listing Possibilities',
                    en_raw: frq2_en_raw,
                    zh_raw: frq2_zh_raw,
                    ans: 'See Explanation',
                    exp_raw: `**1. Listing Possibilities (Partial):**\n若選擇 $L_1, L_2$ 兩支大型股，則中型股可選 $M_1, M_2, M_3, M_4$。\n例如： $\{L_1, L_2, M_1\}, \{L_1, L_2, M_2\}, \\dots$\n若選擇 $L_1, L_3$ 兩支大型股，則有 $\{L_1, L_3, M_1\}, \{L_1, L_3, M_2\}, \\dots$\n(總共需列出 $C(5,2) \\times C(4,1)$ 組，共 40 組)\n\n**2. Total Number of Ways:**\n大型股的組合數: $C(5, 2) = \\frac{5!}{2!3!} = 10$\n中型股的組合數: $C(4, 1) = \\frac{4!}{1!3!} = 4$\n總選股方式: $$C(5, 2) \\times C(4, 1) = 10 \\times 4 = 40 \\text{ ways}$$`
                });

                // FRQ 3: ANOVA Interpretation
                const frq3_en_raw = `Explain the primary purpose of a one-way ANOVA F-test. Given an ANOVA table where $MST=150$ and $MSE=50$, calculate the F-statistic and state the F-test Null Hypothesis.`;
                const frq3_zh_raw = `解釋單因子變異數分析 (One-way ANOVA) F 檢定的主要目的。給定一個 ANOVA 表，其中組間均方 $MST=150$ 且組內均方 $MSE=50$，計算 F 統計量並寫出 F 檢定的虛無假設。`;

                newQuestions.push({
                    type: 'FRQ',
                    title: 'FRQ 3: ANOVA F-Test (Focus: Formulas & Interpretation)',
                    en_raw: frq3_en_raw,
                    zh_raw: frq3_zh_raw,
                    ans: 'See Explanation',
                    exp_raw: `**1. Purpose:** The ANOVA F-test determines whether the means of two or more populations are equal. (判斷兩個或兩個以上母體平均數是否相等)\n\n**2. Null Hypothesis:**\n$$H_0: \\mu_1 = \\mu_2 = \\dots = \\mu_k$$\n\n**3. F-Statistic Calculation:**\n$$F = \\frac{MST}{MSE} = \\frac{150}{50} = 3.0$$`
                });


                appData.generated = newQuestions;
                saveData();

                loader.classList.add('hidden');
                btnText.innerHTML = '<i class="fas fa-magic mr-2"></i>重新生成';
                status.classList.remove('hidden');
                
                document.getElementById('question-count-badge').textContent = `${newQuestions.length} 題`;
                
                switchTab('list');
                
            }, 1200);
        }

        // --- 7. List & Memo Logic ---

        function renderList() {
            const container = document.getElementById('adapted-list-container');
            container.innerHTML = '';

            document.getElementById('question-count-badge').textContent = `${appData.generated.length} 題`;

            if(!appData.generated || appData.generated.length === 0) {
                container.innerHTML = '<div class="text-center text-muji-sub py-8 bg-muji-card rounded-lg border border-dashed border-muji-border">尚無生成的題目</div>';
                return;
            }

            appData.generated.forEach((q, idx) => {
                let content = '';
                let typeBadgeColor = '';

                if (q.type === 'MCQ') {
                    typeBadgeColor = 'bg-blue-100 text-blue-700';
                    content = `
                        <ul class="list-none space-y-2 mb-3 ml-1 mt-2">
                            ${q.options.map(opt => `<li class="text-sm p-2 rounded hover:bg-gray-50 border border-transparent hover:border-gray-200 transition-colors">${opt}</li>`).join('')}
                        </ul>
                    `;
                } else if (q.type === 'Short Answer') {
                    typeBadgeColor = 'bg-yellow-100 text-yellow-700';
                } else if (q.type === 'FRQ') {
                    typeBadgeColor = 'bg-green-100 text-green-700';
                }

                const html = `
                <div class="bg-muji-card rounded-lg p-5 shadow-sm border border-muji-border">
                    <div class="flex justify-between items-start mb-2">
                        <span class="text-xs font-bold px-2 py-1 ${typeBadgeColor} rounded">${q.type}</span>
                        <div class="text-xs text-gray-400 flex items-center">
                            <button onclick="copyToClipboard(appData.generated[${idx}].en_raw + '\\n\\n' + appData.generated[${idx}].zh_raw)" class="text-muji-accent hover:text-muji-accentHover border border-muji-accent hover:border-muji-accentHover px-2 py-0.5 rounded-full mr-2 transition-colors">
                                <i class="fas fa-copy mr-1"></i>複製題目
                            </button>
                            <span>#${idx + 1}</span>
                        </div>
                    </div>
                    
                    <h4 class="font-medium text-muji-text mb-1 text-md">${q.title || ''}</h4>
                    <p class="text-sm text-gray-800 mb-2 leading-relaxed">${q.en}</p>
                    <p class="text-xs text-muji-sub mb-3 border-l-2 border-muji-accent pl-2 leading-relaxed">${q.zh}</p>
                    
                    ${content}

                    <div class="mt-3 pt-3 border-t border-dashed border-gray-200">
                        <details class="group">
                            <summary class="flex items-center text-xs font-medium text-muji-accent cursor-pointer list-none select-none">
                                <span class="mr-2">查看解答與詳解</span>
                                <i class="fas fa-chevron-down group-open:rotate-180 transition-transform"></i>
                            </summary>
                            <div class="mt-2 text-sm text-gray-600 bg-gray-50 p-4 rounded border border-gray-100">
                                <p class="mb-2"><span class="font-bold text-gray-800">Answer:</span> ${q.ans}</p>
                                <div class="text-xs space-y-2">
                                    <span class="font-bold text-gray-800">Explanation:</span> 
                                    <div class="mt-1">${q.exp_raw}</div>
                                </div>
                                <div class="text-right mt-3">
                                    <button onclick="copyToClipboard(appData.generated[${idx}].exp_raw)" class="text-xs text-muji-highlight border border-muji-highlight hover:bg-muji-highlight hover:text-white px-2 py-1 rounded transition-colors">
                                        <i class="fas fa-copy mr-1"></i>複製詳解 (含 LaTeX)
                                    </button>
                                </div>
                            </div>
                        </details>
                    </div>
                </div>
                `;
                container.innerHTML += html;
            });
            renderMath();
        }

        function saveMemo() {
            const val = document.getElementById('memo-input').value;
            appData.memo = val;
            saveData();
            renderMemoDisplay();
        }

        function renderMemoDisplay() {
            const display = document.getElementById('memo-display');
            document.getElementById('memo-input').value = appData.memo || '';
            
            if(!appData.memo) {
                display.innerHTML = '暫無備忘錄';
                return;
            }

            const urlRegex = /(https?:\/\/[^\s]+)/g;
            const formatted = appData.memo.replace(urlRegex, (url) => {
                return `<a href="${url}" target="_blank" class="inline-block mt-1 px-3 py-1 bg-muji-accent text-white text-xs rounded-full hover:bg-muji-accentHover no-underline"><i class="fas fa-link mr-1"></i>Link</a>`;
            });
            display.innerHTML = formatted;
        }

        // --- 8. Info Logic ---
        function saveSourceUrl() {
            appData.sourceUrl = document.getElementById('source-url-input').value;
            saveData();
            renderInfo();
        }

        function renderInfo() {
            const urlDisplay = document.getElementById('source-url-display');
            const input = document.getElementById('source-url-input');
            input.value = appData.sourceUrl || '';
            
            if (appData.sourceUrl) {
                urlDisplay.innerHTML = `<a href="${appData.sourceUrl}" target="_blank" class="block w-full text-center py-2 border border-muji-accent text-muji-accent rounded-lg hover:bg-muji-accent hover:text-white transition-colors"><i class="fas fa-external-link-alt mr-2"></i>前往來源網站</a>`;
            } else {
                urlDisplay.innerHTML = '';
            }

            const refQ = document.getElementById('info-ref-question');
            if(appData.question.text || appData.question.img || appData.question.focus) {
                let content = '';
                if(appData.question.text) content += `<p class="mb-2">**原始題目:** ${appData.question.text}</p>`;
                if(appData.question.focus) content += `<p class="mb-2">**改編重點:** <span class="text-muji-highlight">${appData.question.focus}</span></p>`;
                if(appData.question.img) content += `<p class="text-xs text-gray-400">[包含圖片]</p>`;
                refQ.innerHTML = content;
            } else {
                refQ.innerHTML = '暫無資料';
            }
        }

        function renderAll() {
            renderQuestionView();
            renderAnswerView();
            renderMemoDisplay();
        }

    </script>
</body>
</html>
