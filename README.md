<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trình Đọc AI Đa Phong Cách - Siêu Tốc</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mammoth/1.6.0/mammoth.browser.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .glass { background: rgba(255, 255, 255, 0.98); backdrop-filter: blur(12px); }
        #textContent::-webkit-scrollbar { width: 6px; }
        #textContent::-webkit-scrollbar-track { background: #f1f1f1; border-radius: 10px; }
        #textContent::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
        
        .style-card, .speed-btn, .voice-card { transition: all 0.2s ease; cursor: pointer; border: 1px solid #e2e8f0; position: relative; }
        .active-item { border-color: #4f46e5; background-color: #f5f3ff; ring: 2px; ring-color: #4f46e5; }
        .hover-item:hover:not(.active-item) { background-color: #f8fafc; border-color: #cbd5e1; }
        
        .demo-btn { 
            opacity: 0.7; 
            transition: all 0.15s; 
            padding: 6px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            background: #f1f5f9;
            border: 1px solid #e2e8f0;
        }
        .demo-btn:hover { 
            opacity: 1; 
            background: #e2e8f0;
            color: #4f46e5;
            transform: scale(1.1);
        }
        .demo-btn.playing {
            background: #fee2e2;
            color: #ef4444;
            opacity: 1;
            border-color: #fca5a5;
            animation: pulse-red 1.5s infinite;
        }
        @keyframes pulse-red {
            0% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.4); }
            70% { box-shadow: 0 0 0 6px rgba(239, 68, 68, 0); }
            100% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0); }
        }
    </style>
</head>
<body class="bg-slate-100 min-h-screen flex items-center justify-center p-4">

    <div class="max-w-5xl w-full glass p-6 md:p-8 rounded-3xl shadow-2xl border border-white">
        <!-- Header -->
        <div class="flex flex-col md:flex-row justify-between items-start mb-6 gap-4 border-b pb-4">
            <div class="text-left">
                <h1 class="text-xl md:text-2xl font-extrabold text-slate-900 leading-tight uppercase tracking-tight">Trình Đọc AI Đa Phong Cách</h1>
                <p class="text-slate-500 text-xs font-medium uppercase tracking-wider">Xử lý song song • Đọc nguyên văn • Đa cảm xúc</p>
            </div>
            <div class="flex items-center gap-3">
                <button id="clearBtn" class="px-3 py-1.5 bg-white border border-slate-200 text-slate-600 hover:bg-slate-50 rounded-lg font-bold text-[10px] transition shadow-sm uppercase">
                    Làm mới toàn bộ
                </button>
            </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            <!-- Left Column -->
            <div class="lg:col-span-7 space-y-6">
                <!-- Tải File -->
                <div class="relative group">
                    <input type="file" id="fileInput" accept=".docx,.txt" class="hidden">
                    <label for="fileInput" class="cursor-pointer flex items-center gap-3 p-3 border-2 border-dashed border-indigo-100 rounded-xl bg-indigo-50/20 hover:border-indigo-400 hover:bg-indigo-50 transition-all">
                        <div class="p-2 bg-indigo-500 rounded-lg text-white">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"></path></svg>
                        </div>
                        <div class="text-left">
                            <span class="text-indigo-900 font-bold text-sm block" id="fileNameDisplay">Tải văn bản lên (.docx, .txt)</span>
                        </div>
                    </label>
                </div>

                <!-- Chọn Người Đọc -->
                <div>
                    <label class="text-[10px] font-black text-slate-400 uppercase tracking-[0.2em] mb-2 block">Chọn Người đọc (Nhấn loa để thử giọng)</label>
                    <div class="grid grid-cols-2 sm:grid-cols-3 gap-2" id="voiceGrid">
                        <!-- Voices added by JS -->
                    </div>
                </div>

                <!-- Chọn phong cách -->
                <div>
                    <label class="text-[10px] font-black text-slate-400 uppercase tracking-[0.2em] mb-2 block">Chọn phong cách đọc</label>
                    <div class="grid grid-cols-2 sm:grid-cols-4 gap-2" id="styleGrid">
                        <!-- Styles added by JS -->
                    </div>
                </div>

                <!-- Chọn Tốc độ -->
                <div>
                    <label class="text-[10px] font-black text-slate-400 uppercase tracking-[0.2em] mb-2 block">Tốc độ đọc</label>
                    <div class="flex gap-2" id="speedGrid">
                        <button class="speed-btn hover-item px-3 py-1.5 rounded-lg bg-white text-[9px] font-bold uppercase text-slate-700 flex-1" data-speed="slow">Chậm</button>
                        <button class="speed-btn hover-item px-3 py-1.5 rounded-lg bg-white text-[9px] font-bold uppercase text-slate-700 flex-1 active-item" data-speed="normal">Vừa</button>
                        <button class="speed-btn hover-item px-3 py-1.5 rounded-lg bg-white text-[9px] font-bold uppercase text-slate-700 flex-1" data-speed="fast">Nhanh</button>
                        <button class="speed-btn hover-item px-3 py-1.5 rounded-lg bg-white text-[9px] font-bold uppercase text-slate-700 flex-1" data-speed="very-fast">Rất nhanh</button>
                    </div>
                </div>

                <!-- Preview Area -->
                <div class="space-y-2">
                    <div class="flex justify-between items-center px-1">
                        <label class="text-[10px] font-black text-slate-400 uppercase tracking-widest">Nội dung văn bản</label>
                        <span id="charCount" class="text-[9px] font-black bg-slate-200 px-1.5 py-0.5 rounded text-slate-600 uppercase">0 ký tự</span>
                    </div>
                    <textarea id="textContent" placeholder="Nội dung sẽ xuất hiện ở đây..." class="w-full h-40 overflow-y-auto text-sm text-slate-700 leading-relaxed bg-white p-4 rounded-xl border border-slate-200 shadow-inner resize-none focus:ring-2 focus:ring-indigo-100 outline-none transition-all"></textarea>
                </div>
            </div>

            <!-- Right Column -->
            <div class="lg:col-span-5 space-y-6">
                <!-- Status Card -->
                <div class="bg-slate-50 p-5 rounded-2xl border border-slate-200">
                    <div class="flex justify-between items-center mb-3">
                        <span class="text-[10px] font-bold text-slate-500 uppercase tracking-widest">Hệ thống</span>
                        <span id="statusIndicator" class="w-2.5 h-2.5 bg-slate-300 rounded-full"></span>
                    </div>
                    <p id="statusText" class="text-xs font-medium text-slate-600 mb-3">Sẵn sàng nhận tệp.</p>
                    
                    <div id="progressContainer" class="hidden space-y-1.5 border-t pt-3">
                        <div class="flex justify-between items-end">
                            <span class="text-[9px] font-bold text-indigo-600 uppercase">Tiến trình AI</span>
                            <span id="progressText" class="text-sm font-black text-indigo-600">0%</span>
                        </div>
                        <div class="w-full bg-slate-200 rounded-full h-1.5 overflow-hidden">
                            <div id="progressBar" class="bg-indigo-600 h-full w-0 transition-all duration-300"></div>
                        </div>
                    </div>
                </div>

                <!-- Main Action -->
                <button id="generateBtn" disabled class="w-full bg-indigo-600 hover:bg-indigo-700 disabled:bg-slate-300 text-white py-5 rounded-xl font-extrabold text-sm shadow-lg hover:shadow-indigo-200 transition-all flex flex-col items-center gap-1 uppercase tracking-wider">
                    <div id="btnContent" class="flex items-center gap-2">
                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M8 5v14l11-7z"/></svg>
                        <span>Chuyển đổi văn bản</span>
                    </div>
                </button>

                <!-- Player & Download -->
                <div id="resultContainer" class="hidden space-y-3 animate-in fade-in zoom-in duration-300">
                    <div class="p-3 bg-white border border-slate-200 rounded-xl shadow-sm">
                        <audio id="audioPlayer" controls class="w-full h-10"></audio>
                    </div>
                    <button id="downloadBtn" class="w-full bg-emerald-600 text-white hover:bg-emerald-700 py-3.5 rounded-xl font-bold text-xs flex items-center justify-center gap-2 shadow-md transition-all uppercase">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a2 2 0 002 2h12a2 2 0 002-2v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path></svg>
                        <span>Tải file âm thanh (.WAV)</span>
                    </button>
                </div>

                <!-- Hidden audio for demos -->
                <audio id="demoAudioPlayer" class="hidden"></audio>
            </div>
        </div>
    </div>

    <script>
        const apiKey = ""; 
        
        const voices = [
            // NAM
            { id: 'Fenrir', name: 'Hoài Nam', gender: 'Nam', desc: 'Trịnh trọng/Trầm ấm', sample: 'Tôi là Hoài Nam, giọng nam trịnh trọng dành cho các văn bản quan trọng.' },
            { id: 'Charon', name: 'Mạnh Cường', gender: 'Nam', desc: 'Quyền lực/Dứt khoát', sample: 'Chào bạn, Mạnh Cường đây. Giọng nam quyền lực của mình rất hợp cho các bản tin.' },
            { id: 'Zephyr', name: 'Duy Anh', gender: 'Nam', desc: 'Trẻ trung/Năng động', sample: 'Duy Anh đây! Giọng nam trẻ trung của mình rất hợp cho review phim đó.' },
            
            // NỮ
            { id: 'Leda', name: 'Lan Anh', gender: 'Nữ', desc: 'Diễn cảm/Mượt mà', sample: 'Mình là Lan Anh. Giọng nữ mượt mà của mình sẽ làm văn bản thêm sâu lắng.' },
            { id: 'Aoede', name: 'Minh Thư', gender: 'Nữ', desc: 'Ngọt ngào/Ấm áp', sample: 'Chào bạn, mình là Minh Thư. Giọng nữ ấm áp của mình rất hợp kể chuyện đó.' },
            { id: 'Erinome', name: 'Thu Trang', gender: 'Nữ', desc: 'Mạnh mẽ/Quyền lực', sample: 'Thu Trang đây! Giọng nữ mạnh mẽ của mình sẽ làm bài thuyết trình thêm ấn tượng.' },

            // TRẺ EM
            { id: 'Puck', name: 'Bé Bo', gender: 'Trẻ em', desc: 'Tinh nghịch/Đáng yêu', sample: 'Chào cô chú, con là Bé Bo nè! Giọng con tinh nghịch và đáng yêu lắm đó!' },
            { id: 'Autonoe', name: 'Bé Bống', gender: 'Trẻ em', desc: 'Ngây thơ/Trong sáng', sample: 'Con chào cô chú, con là Bé Bống. Giọng con ngây thơ và trong sáng lắm ạ!' }
        ];

        const styles = [
            { id: 'presentation', name: 'Thuyết trình', icon: '🎤', prompt: 'Đọc trịnh trọng, chuyên nghiệp, hào hùng.', sample: 'Kính thưa quý vị, hạnh phúc của nhân dân là mục tiêu tối thượng.' },
            { id: 'fb-film', name: 'Phim ngắn FB', icon: '🎬', prompt: 'Đọc kịch tính, hồi hộp, nhịp điệu nhanh.', sample: 'Cái kết không ai ngờ tới! Đừng rời mắt khỏi màn hình ngay lúc này!' },
            { id: 'children', name: 'Trẻ em', icon: '🧸', prompt: 'Hãy đọc bằng giọng trẻ em hồn nhiên, ngây thơ, trong sáng và đầy năng lượng, nhịp điệu vui vẻ.', sample: 'Chào các bạn nhỏ, hôm nay chúng mình cùng học về lòng tử tế nhé! Vui quá đi!' },
            { id: 'emotional', name: 'Diễn cảm', icon: '🎭', prompt: 'Đọc giàu cảm xúc, nhấn nhá uyển chuyển.', sample: 'Những kỷ niệm thơ ấu luôn là khoảng trời bình yên nhất trong mỗi chúng ta.' },
            { id: 'storytelling', name: 'Kể chuyện', icon: '📚', prompt: 'Đọc lôi cuốn, ấm áp, nhịp điệu chậm.', sample: 'Ngày xửa ngày xưa, tại một xứ sở xa xôi có một điều kỳ diệu đã xảy ra.' },
            { id: 'flat', name: 'Vô cảm', icon: '🤖', prompt: 'Đọc hoàn toàn vô cảm, giọng phẳng như robot.', sample: 'Tôi là hệ thống máy tính. Tôi đang thực hiện đọc văn bản một cách vô cảm.' },
            { id: 'news', name: 'Bản tin', icon: '📰', prompt: 'Đọc dứt khoát, chuyên nghiệp, chuẩn xác.', sample: 'Bản tin sáng nay có những nội dung đáng chú ý sau đây.' },
            { id: 'whisper', name: 'Tâm sự', icon: '💬', prompt: 'Đọc thì thầm, sâu lắng, gần gũi.', sample: 'Này bạn, đôi khi ta chỉ cần một khoảng lặng để thấu hiểu bản thân mình hơn.' }
        ];

        const speedPrompts = {
            'slow': 'Đọc tốc độ chậm.',
            'normal': 'Đọc tốc độ vừa.',
            'fast': 'Đọc tốc độ nhanh.',
            'very-fast': 'Đọc tốc độ rất nhanh.'
        };

        let selectedVoice = voices[0];
        let selectedStyle = styles[0];
        let selectedSpeed = 'normal';
        let activeDemoBtn = null;

        const voiceGrid = document.getElementById('voiceGrid');
        const styleGrid = document.getElementById('styleGrid');
        const speedButtons = document.querySelectorAll('.speed-btn');
        const fileInput = document.getElementById('fileInput');
        const fileNameDisplay = document.getElementById('fileNameDisplay');
        const textContent = document.getElementById('textContent');
        const charCount = document.getElementById('charCount');
        const generateBtn = document.getElementById('generateBtn');
        const statusText = document.getElementById('statusText');
        const statusIndicator = document.getElementById('statusIndicator');
        const progressContainer = document.getElementById('progressContainer');
        const progressBar = document.getElementById('progressBar');
        const progressText = document.getElementById('progressText');
        const resultContainer = document.getElementById('resultContainer');
        const audioPlayer = document.getElementById('audioPlayer');
        const demoAudioPlayer = document.getElementById('demoAudioPlayer');
        const downloadBtn = document.getElementById('downloadBtn');
        const clearBtn = document.getElementById('clearBtn');

        // Logic nghe thử siêu tốc và dừng ngay
        async function toggleDemo(text, voiceId, btn, stylePrompt = "") {
            if (activeDemoBtn === btn) {
                stopAllDemos();
                return;
            }

            stopAllDemos();
            activeDemoBtn = btn;
            btn.classList.add('playing');
            btn.innerHTML = `<svg class="w-3 h-3" fill="currentColor" viewBox="0 0 24 24"><rect x="6" y="6" width="12" height="12"/></svg>`;
            updateStatus("Đang tải...");

            const fullPrompt = `Hãy đọc ngay câu này: "${text}". ${stylePrompt ? 'Phong cách yêu cầu: ' + stylePrompt : ''}`;
            
            const payload = {
                contents: [{ parts: [{ text: fullPrompt }] }],
                generationConfig: {
                    responseModalities: ["AUDIO"],
                    speechConfig: { voiceConfig: { prebuiltVoiceConfig: { voiceName: voiceId } } }
                },
                model: "gemini-2.5-flash-preview-tts"
            };

            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                
                if (!response.ok) throw new Error();
                const result = await response.json();
                const audioData = result.candidates[0].content.parts[0].inlineData;
                const sampleRate = parseInt(audioData.mimeType.match(/sampleRate=(\d+)/)?.[1] || "24000");
                const binaryData = atob(audioData.data);
                const bytes = new Uint8Array(binaryData.length);
                for (let i = 0; i < binaryData.length; i++) bytes[i] = binaryData.charCodeAt(i);
                
                const wavBuffer = pcmToWav(bytes.buffer, sampleRate);
                const url = URL.createObjectURL(new Blob([wavBuffer], { type: 'audio/wav' }));
                
                if (activeDemoBtn !== btn) return;

                demoAudioPlayer.src = url;
                demoAudioPlayer.play();
                updateStatus("Đang nghe thử", "success");
            } catch (err) {
                stopAllDemos();
                updateStatus("Lỗi kết nối", "error");
            }
        }

        function stopAllDemos() {
            demoAudioPlayer.pause();
            demoAudioPlayer.src = "";
            if (activeDemoBtn) {
                activeDemoBtn.classList.remove('playing');
                activeDemoBtn.innerHTML = `<svg class="w-3 h-3" fill="currentColor" viewBox="0 0 24 24"><path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z"/></svg>`;
                activeDemoBtn = null;
            }
        }

        demoAudioPlayer.onended = stopAllDemos;

        function initVoices() {
            voiceGrid.innerHTML = '';
            voices.forEach(voice => {
                const card = document.createElement('div');
                card.className = `voice-card hover-item p-2 rounded-lg bg-white flex flex-col gap-0.5 shadow-sm ${voice.id === selectedVoice.id ? 'active-item' : ''}`;
                card.innerHTML = `
                    <div class="flex justify-between items-center">
                        <span class="text-[9px] font-extrabold text-slate-800 uppercase tracking-tight">${voice.name}</span>
                        <button class="demo-btn" title="Nghe thử">
                            <svg class="w-3 h-3" fill="currentColor" viewBox="0 0 24 24"><path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z"/></svg>
                        </button>
                    </div>
                    <span class="text-[7px] text-slate-400 italic font-medium">${voice.gender} • ${voice.desc}</span>
                `;
                card.onclick = (e) => {
                    if (e.target.closest('.demo-btn')) {
                        toggleDemo(voice.sample, voice.id, e.target.closest('.demo-btn'));
                        return;
                    }
                    document.querySelectorAll('.voice-card').forEach(c => c.classList.remove('active-item'));
                    card.classList.add('active-item');
                    selectedVoice = voice;
                    updateStatus(`Giọng: ${voice.name}`);
                };
                voiceGrid.appendChild(card);
            });
        }

        function initStyles() {
            styleGrid.innerHTML = '';
            styles.forEach(style => {
                const card = document.createElement('div');
                card.className = `style-card hover-item p-2 rounded-lg bg-white flex items-center justify-between gap-1 shadow-sm ${style.id === selectedStyle.id ? 'active-item' : ''}`;
                card.innerHTML = `
                    <div class="flex items-center gap-1.5 overflow-hidden">
                        <span class="text-xs flex-shrink-0">${style.icon}</span>
                        <span class="text-[9px] font-bold uppercase text-slate-700 tracking-tight truncate">${style.name}</span>
                    </div>
                    <button class="demo-btn flex-shrink-0" title="Nghe thử">
                        <svg class="w-3 h-3" fill="currentColor" viewBox="0 0 24 24"><path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z"/></svg>
                    </button>
                `;
                card.onclick = (e) => {
                    if (e.target.closest('.demo-btn')) {
                        toggleDemo(style.sample, selectedVoice.id, e.target.closest('.demo-btn'), style.prompt);
                        return;
                    }
                    document.querySelectorAll('.style-card').forEach(c => c.classList.remove('active-item'));
                    card.classList.add('active-item');
                    selectedStyle = style;
                    updateStatus(`Phong cách: ${style.name}`);
                };
                styleGrid.appendChild(card);
            });
        }

        initVoices();
        initStyles();

        speedButtons.forEach(btn => {
            btn.onclick = () => {
                speedButtons.forEach(b => b.classList.remove('active-item'));
                btn.classList.add('active-item');
                selectedSpeed = btn.getAttribute('data-speed');
                updateStatus(`Tốc độ: ${btn.innerText}`);
            };
        });

        function updateStatus(msg, type = 'info') {
            statusText.innerText = msg;
            statusIndicator.className = `w-2.5 h-2.5 rounded-full ${type === 'success' ? 'bg-emerald-500' : type === 'error' ? 'bg-red-500' : 'bg-indigo-500 animate-pulse'}`;
        }

        textContent.addEventListener('input', () => {
            const len = textContent.value.trim().length;
            charCount.innerText = `${len.toLocaleString()} KÝ TỰ`;
            generateBtn.disabled = len === 0;
        });

        fileInput.addEventListener('change', async (e) => {
            const file = e.target.files[0];
            if (!file) return;
            fileNameDisplay.innerText = file.name;
            updateStatus("Đang đọc tệp...");
            try {
                let text = "";
                if (file.name.endsWith('.docx')) {
                    const arrayBuffer = await file.arrayBuffer();
                    const result = await mammoth.extractRawText({ arrayBuffer: arrayBuffer });
                    text = result.value;
                } else {
                    text = await file.text();
                }
                textContent.value = text;
                textContent.dispatchEvent(new Event('input'));
                updateStatus("Đã nạp văn bản!", "success");
            } catch (err) {
                updateStatus("Lỗi đọc tệp", "error");
            }
        });

        clearBtn.addEventListener('click', () => {
            stopAllDemos();
            textContent.value = "";
            textContent.dispatchEvent(new Event('input'));
            fileNameDisplay.innerText = "Tải văn bản lên (.docx, .txt)";
            fileInput.value = "";
            selectedStyle = styles[0];
            selectedVoice = voices[0];
            selectedSpeed = 'normal';
            initStyles();
            initVoices();
            speedButtons.forEach(btn => {
                btn.classList.remove('active-item');
                if (btn.getAttribute('data-speed') === 'normal') btn.classList.add('active-item');
            });
            resultContainer.classList.add('hidden');
            progressContainer.classList.add('hidden');
            audioPlayer.src = "";
            updateStatus("Đã làm mới.");
        });

        function pcmToWav(pcmData, sampleRate) {
            const buffer = new ArrayBuffer(44 + pcmData.byteLength);
            const view = new DataView(buffer);
            const writeString = (offset, string) => {
                for (let i = 0; i < string.length; i++) view.setUint8(offset + i, string.charCodeAt(i));
            };
            writeString(0, 'RIFF');
            view.setUint32(4, 32 + pcmData.byteLength, true);
            writeString(8, 'WAVE');
            writeString(12, 'fmt ');
            view.setUint32(16, 16, true);
            view.setUint16(20, 1, true); 
            view.setUint16(22, 1, true); 
            view.setUint32(24, sampleRate, true);
            view.setUint32(28, sampleRate * 2, true);
            view.setUint16(32, 2, true);
            view.setUint16(34, 16, true);
            writeString(36, 'data');
            view.setUint32(40, pcmData.byteLength, true);
            new Uint8Array(buffer, 44).set(new Uint8Array(pcmData));
            return buffer;
        }

        function chunkText(text, maxLength = 2500) {
            const paragraphs = text.split(/\n+/);
            const chunks = [];
            let currentChunk = "";
            for (let p of paragraphs) {
                if ((currentChunk + p).length > maxLength && currentChunk.length > 0) {
                    chunks.push(currentChunk.trim());
                    currentChunk = p + "\n";
                } else {
                    currentChunk += p + "\n";
                }
            }
            if (currentChunk.trim()) chunks.push(currentChunk.trim());
            return chunks;
        }

        async function fetchChunkAudio(chunk, retryCount = 0) {
            const fullPrompt = `${speedPrompts[selectedSpeed]} ${selectedStyle.prompt}\n\nNội dung văn bản:\n${chunk}`;
            const payload = {
                contents: [{ parts: [{ text: fullPrompt }] }],
                generationConfig: {
                    responseModalities: ["AUDIO"],
                    speechConfig: { voiceConfig: { prebuiltVoiceConfig: { voiceName: selectedVoice.id } } }
                },
                model: "gemini-2.5-flash-preview-tts"
            };

            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                if (!response.ok) throw new Error();
                const result = await response.json();
                const audioData = result.candidates[0].content.parts[0].inlineData;
                const sampleRate = parseInt(audioData.mimeType.match(/sampleRate=(\d+)/)?.[1] || "24000");
                const binaryData = atob(audioData.data);
                const bytes = new Uint8Array(binaryData.length);
                for (let i = 0; i < binaryData.length; i++) bytes[i] = binaryData.charCodeAt(i);
                return { bytes, sampleRate };
            } catch (err) {
                if (retryCount < 5) {
                    await new Promise(r => setTimeout(r, Math.pow(2, retryCount) * 1000));
                    return fetchChunkAudio(chunk, retryCount + 1);
                }
                throw err;
            }
        }

        async function generateAudio() {
            const textToRead = textContent.value.trim();
            if (!textToRead) return;
            stopAllDemos();

            generateBtn.disabled = true;
            generateBtn.innerHTML = `<span>Đang xử lý...</span>`;
            progressContainer.classList.remove('hidden');
            resultContainer.classList.add('hidden');
            updateStatus(`Đang tạo giọng ${selectedVoice.name}...`);

            const chunks = chunkText(textToRead);
            let finishedChunks = 0;

            try {
                const promises = chunks.map(async (chunk, index) => {
                    const result = await fetchChunkAudio(chunk);
                    finishedChunks++;
                    const progress = Math.round((finishedChunks / chunks.length) * 100);
                    progressBar.style.width = `${progress}%`;
                    progressText.innerText = `${progress}%`;
                    return { index, data: result };
                });

                const results = await Promise.all(promises);
                results.sort((a, b) => a.index - b.index);

                const totalLength = results.reduce((acc, r) => acc + r.data.bytes.length, 0);
                const combinedBytes = new Uint8Array(totalLength);
                let offset = 0;
                for (let r of results) {
                    combinedBytes.set(r.data.bytes, offset);
                    offset += r.data.bytes.length;
                }

                const wavBuffer = pcmToWav(combinedBytes.buffer, results[0].data.sampleRate);
                const url = URL.createObjectURL(new Blob([wavBuffer], { type: 'audio/wav' }));

                audioPlayer.src = url;
                resultContainer.classList.remove('hidden');
                generateBtn.disabled = false;
                generateBtn.innerHTML = `<div class="flex items-center gap-2"><svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M8 5v14l11-7z"/></svg><span>Chuyển đổi văn bản</span></div>`;
                updateStatus("Hoàn tất!", "success");

                downloadBtn.onclick = () => {
                    const a = document.createElement('a');
                    a.href = url;
                    a.download = `Audio_${selectedVoice.name}_${selectedStyle.id}.wav`;
                    a.click();
                };
            } catch (err) {
                updateStatus("Lỗi xử lý", "error");
                generateBtn.disabled = false;
                generateBtn.innerHTML = `<span>Thử lại</span>`;
            }
        }

        generateBtn.addEventListener('click', generateAudio);
    </script>
</body>
</html>

