<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CoolPark</title>
    <style>
        /* --- 基础样式 --- */
        body { margin: 0; overflow: hidden; background-color: #000000; /* 纯黑背景 */ font-family: 'Songti SC', 'SimSun', serif; }
        #canvas-container { width: 100vw; height: 100vh; position: absolute; top: 0; left: 0; z-index: 1; }
        
        /* 引入精选艺术字体 */
        @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@700&family=Great+Vibes&family=Monoton&family=Abril+Fatface&family=Ma+Shan+Zheng&display=swap');

        /* --- UI 层 --- */
        #ui-layer { position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 10; pointer-events: none; }
        
        /* 隐藏动画类 */
        .panel-hidden { opacity: 0 !important; pointer-events: none !important; transform: translateX(-20px); }
        .hidden { display: none !important; }

        /* 按钮样式 - 正红色调优化 */
        .elegant-btn {
            background: rgba(200, 20, 20, 0.45); /* 正红半透明 */
            border: 1px solid rgba(220, 40, 40, 0.6); /* 正红边框 */
            color: #f8d080; /* 温润香槟金文字 */
            padding: 8px 12px; cursor: pointer; 
            text-transform: uppercase; letter-spacing: 1px; font-size: 11px;
            transition: all 0.2s; display: inline-flex; align-items: center; justify-content: center;
            backdrop-filter: blur(4px); text-decoration: none; min-width: 100px;
            font-family: 'Microsoft YaHei', sans-serif; pointer-events: auto; border-radius: 2px;
            box-sizing: border-box;
        }
        .elegant-btn:hover { background: #f8d080; color: #800000; border-color: #f8d080; }
        .elegant-btn input[type="file"] { display: none !important; }
        .btn-red { border-color: #c02020; color: #f8cccc; }
        .btn-red:hover { background: #c02020; color: #fff; }
        
        #play-pause-btn { width: 100%; margin-bottom: 6px; font-weight: bold; border-color: #f8d080; padding: 8px; }

        /* --- 标题 (可拖拽) --- */
        #title-container {
            position: absolute; top: 10%; left: 50%; transform: translateX(-50%);
            text-align: center; pointer-events: auto; cursor: move; 
            z-index: 50; transition: color 0.2s; user-select: none; padding: 10px;
        }
        .title-line { 
            margin: 0; transition: all 0.2s ease; white-space: nowrap;
            color: #fff3e0; /* 淡金色文字 */
            text-shadow: 0 0 30px rgba(220, 20, 20, 0.7), 0 0 10px rgba(248, 208, 128, 0.7); /* 正红+淡金阴影 */
        }

        /* --- 左侧侧边栏容器 (V16.4 新增：用于自动对齐) --- */
        #left-sidebar {
            position: absolute; top: 20px; left: 20px; width: 200px;
            display: flex; flex-direction: column; gap: 10px; /* 面板与手势指引的间距 */
            pointer-events: none; /* 让容器不阻挡，内部元素单独开启 */
            z-index: 20;
        }

        /* --- 左上角：设置面板 (放在侧边栏内) --- */
        .top-left-panel {
            pointer-events: auto; display: flex; flex-direction: column; gap: 8px; 
            background:linear-gradient(rgba(200, 20, 20, 0.45), rgba(120, 10, 10, 0.3)); /* 正红渐变 */
            padding: 12px; border-radius: 4px;
            border: 1px solid rgba(220, 40, 40, 0.7); /* 正红边框 */
            backdrop-filter: blur(8px);
            transition: all 0.4s ease;
            font-family: 'Microsoft YaHei', sans-serif;
            width: 100%; box-sizing: border-box; /* 填满侧边栏 */
        }
        .panel-header { color: #f8d080; border-bottom: 1px solid rgba(255,255,255,0.15); padding-bottom: 4px; margin-bottom: 2px; font-size: 10px; letter-spacing: 1px; }
        
        .control-row { display: flex; flex-direction: column; gap: 3px; }
        .control-label { color: #ffe0c0; font-size: 10px; }
        .input-dark { 
            background: rgba(255,255,255,0.05); border: 1px solid #c02020; color: #f8d080; 
            padding: 4px 6px; font-size: 11px; outline: none; transition: 0.2s; border-radius: 2px;
        }
        .input-dark:focus { border-color: #f8d080; background: rgba(255,255,255,0.1); }
        
        input[type="color"] { -webkit-appearance: none; border: none; width: 100%; height: 20px; cursor: pointer; padding: 0; background: none; }
        .slider { -webkit-appearance: none; width: 100%; height: 2px; background: linear-gradient(to right, #c02020, #f8d080, #a04020); outline: none; margin-top: 4px; }
        .slider::-webkit-slider-thumb { -webkit-appearance: none; width: 10px; height: 10px; background: #f8d080; border-radius: 50%; cursor: pointer; border: 1px solid #c02020; }
        select.input-dark { cursor: pointer; appearance: none; }
        select.input-dark option { background: #100500; color: #f8d080; padding: 5px; }

        /* --- 手势交互指引 (V16.4 修改：网格布局) --- */
        .left-gesture-panel {
            pointer-events: none; opacity: 0.9; transition: all 0.4s ease;
            display: grid; grid-template-columns: 1fr 1fr; /* 两排 */
            gap: 8px; width: 100%; box-sizing: border-box;
        }
        .gesture-item {
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            background:linear-gradient(rgba(200, 20, 20, 0.6), rgba(120, 10, 10, 0.4)); /* 正红渐变 */
            border: 1px solid rgba(220, 40, 40, 0.7); /* 正红边框 */
            border-radius: 4px; padding: 8px 4px;
            color: #f8d080; font-family: 'Microsoft YaHei', sans-serif; font-size: 10px;
        }
        .gesture-icon { font-size: 18px; margin-bottom: 2px; filter: drop-shadow(0 0 5px rgba(220, 40, 40, 0.7)) drop-shadow(0 0 3px rgba(248, 208, 128, 0.7)); }
        .gesture-text { opacity: 0.9; letter-spacing: 1px; text-shadow: 0 0 2px #000; }

        /* --- 左下角：媒体管理 --- */
        .bottom-left-panel { 
            position: absolute; bottom: 20px; left: 20px; 
            pointer-events: auto; display: flex; flex-direction: column; gap: 8px; 
            transition: all 0.4s ease; width: 140px;
        }
        .bottom-left-panel .elegant-btn { width: 100%; }

        /* --- 右上角：视图控制 --- */
        #top-right-controls { 
            position: absolute; top: 20px; right: 20px; 
            pointer-events: auto; display: flex; flex-direction: column; gap: 8px; align-items: flex-end; z-index: 50; 
        }

        /* --- 右下角：摄像头 --- */
        #webcam-wrapper {
            position: absolute; bottom: 20px; right: 20px; 
            width: 180px; height: 135px;
            border: 1px solid rgba(220, 40, 40, 0.7); /* 正红边框 */
            border-radius: 4px;
            box-shadow: 0 0 15px rgba(220, 40, 40, 0.4), 0 0 5px rgba(248, 208, 128, 0.4); /* 正红金阴影 */
            overflow: hidden; z-index: 20; pointer-events: auto; background: #000;
            transition: opacity 0.4s ease, transform 0.4s ease;
        }
        #webcam-wrapper.camera-hidden { opacity: 0; pointer-events: none; transform: translateY(10px); }
        #webcam-canvas { width: 100%; height: 100%; object-fit: cover; transform: scaleX(-1); display: block; }
        #cam-status {
            position: absolute; bottom: 5px; right: 5px; width: 6px; height: 6px; background: #c02020;
            border-radius: 50%; box-shadow: 0 0 4px #e04040, 0 0 2px #f8d080; z-index: 30; transition: 0.2s;
        }
        #cam-status.active { background: #f8d080; box-shadow: 0 0 6px #f8d080, 0 0 3px #e04040; }

        /* --- 弹窗 --- */
        #delete-manager {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.95); z-index: 60;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            pointer-events: auto; backdrop-filter: blur(4px);
        }
        #photo-grid {
            display: flex; flex-wrap: wrap; gap: 15px; width: 70%; height: 60%;
            overflow-y: auto; justify-content: center; padding: 20px;
            border: 1px solid rgba(220, 40, 40, 0.7); /* 正红边框 */
            margin: 15px 0; background: linear-gradient(rgba(100,10,10,0.6), rgba(80,10,10,0.5)); /* 正红棕背景 */
            border-radius: 4px;
        }
        .photo-item { width: 80px; height: 80px; position: relative; border: 1px solid #f8d080; transition: 0.1s; }
        .photo-item:hover { transform: scale(1.05); border-color: #fff; }
        .photo-thumb { width: 100%; height: 100%; object-fit: cover; }
        .delete-x {
            position: absolute; top: -8px; right: -8px; width: 20px; height: 20px; background: #c02020; color: #f8d080;
            border-radius: 50%; text-align: center; line-height: 18px; font-size: 12px;
            cursor: pointer; font-weight: bold; border: 1px solid #f8d080; 
        }
        .manager-title { color: #f8d080; font-size: 20px; font-family: 'Microsoft YaHei', serif; letter-spacing: 2px; text-shadow: 0 0 10px #c02020; }

        /* --- Loader --- */
        #loader {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: #000000; /* 纯黑背景 */
            z-index: 100; display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: opacity 0.4s ease-out;
        }
        .spinner { width: 40px; height: 40px; border: 1px solid rgba(220, 40, 40, 0.7); border-top: 1px solid #f8d080; border-radius: 50%; animation: spin 0.8s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        .loader-text { color: #f8d080; font-size: 12px; letter-spacing: 3px; margin-top: 20px; font-family: 'Cinzel', serif; text-shadow: 0 0 5px #c02020; }
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #000000; /* 纯黑滚动条轨道 */ }
        ::-webkit-scrollbar-thumb { background: linear-gradient(#c02020, #e0b060); border-radius: 3px; }
    </style>

    <script type="importmap">
        { "imports": { "three": "https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js", "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.160.0/examples/jsm/", "@mediapipe/tasks-vision": "https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.3/+esm" } }
    </script>
</head>
<body>

    <div id="loader"><div class="spinner"></div><div class="loader-text">V16.4 NEW YEAR RED READY</div></div>
    <div id="canvas-container"></div>

    <div id="title-container">
        <h1 id="display-line1" class="title-line">Merry</h1>
        <h1 id="display-line2" class="title-line">Christmas</h1>
    </div>

    <div id="ui-layer">
        <div id="top-right-controls">
            <button class="elegant-btn" id="fs-btn" onclick="toggleFullScreen()">⛶ 全屏显示</button>
            <button class="elegant-btn" id="toggle-ui-btn" onclick="toggleUI()">👁 隐藏界面</button>
        </div>

        <div id="left-sidebar">
            <div class="top-left-panel">
                <div class="panel-header">自定义文本</div> <div class="control-row">
                    <input type="text" id="input-line1" class="input-dark" placeholder="第一行文字" oninput="updateTextConfig()">
                    <input type="text" id="input-line2" class="input-dark" placeholder="第二行文字" oninput="updateTextConfig()">
                </div>
                <div class="control-row">
                    <span class="control-label">字体样式 (5种精选)</span>
                    <select id="font-select" class="input-dark" onchange="updateTextConfig()">
                        <option value="style1">书法韵味 (Ma Shan Zheng)</option>
                        <option value="style2">古典衬线 (Cinzel)</option>
                        <option value="style3">优雅手写 (Great Vibes)</option>
                        <option value="style4">艺术线条 (Monoton)</option>
                        <option value="style5">复古重磅 (Abril Fatface)</option>
                    </select>
                </div>
                <div class="control-row">
                    <span class="control-label">字体大小</span>
                    <input type="range" min="50" max="250" value="100" class="slider" id="slider-fontsize" oninput="updateTextConfig()">
                </div>
                <div class="control-row">
                    <span class="control-label">字体颜色</span>
                    <input type="color" id="color-picker" value="#f8d080" oninput="updateTextConfig()">
                </div>

                <div class="panel-header" style="margin-top:8px;">媒体控制</div>
                <button class="elegant-btn" id="play-pause-btn" onclick="toggleMusicPlay()">▶ 播放音乐</button>
                <div class="control-row">
                    <span class="control-label">音量: <span id="val-vol">50</span>%</span>
                    <input type="range" min="0" max="100" value="50" class="slider" id="slider-volume" oninput="updateVolume(this.value)">
                </div>

                <div class="panel-header" style="margin-top:8px;">粒子系统</div>
                <div class="control-row">
                    <span class="control-label">装饰密度</span>
                    <input type="range" min="500" max="3000" value="1500" class="slider" id="slider-tree">
                </div>
                <div class="control-row">
                    <span class="control-label">星尘密度</span>
                    <input type="range" min="500" max="5000" value="2500" class="slider" id="slider-dust">
                </div>
                <button class="elegant-btn" style="width:100%; margin-top:5px;" onclick="applyParticleSettings()">⚡ 重建场景</button>
            </div>

            <div class="left-gesture-panel">
                <div class="gesture-item">
                    <div class="gesture-icon">✊</div>
                    <div class="gesture-text">聚合</div>
                </div>
                <div class="gesture-item">
                    <div class="gesture-icon">✋</div>
                    <div class="gesture-text">散开</div>
                </div>
                <div class="gesture-item">
                    <div class="gesture-icon">👌</div>
                    <div class="gesture-text">抓取</div>
                </div>
                <div class="gesture-item">
                    <div class="gesture-icon">↔</div>
                    <div class="gesture-text">旋转</div>
                </div>
            </div>
        </div>

        <div class="bottom-left-panel">
            <label class="elegant-btn">
                + 上传照片
                <input type="file" id="file-input" multiple accept="image/*">
            </label>
            <button class="elegant-btn" onclick="openDeleteManager()">▣ 照片库</button>
            <label class="elegant-btn" id="music-upload-label">
                ♫ 更换音乐
                <input type="file" id="music-input" accept=".mp3,audio/mpeg">
            </label>
            <button class="elegant-btn" id="toggle-cam-btn" onclick="toggleCameraDisplay()">📷 摄像头</button>
        </div>
    </div>
    
    <div id="gesture-hint" style="position: absolute; bottom: 10px; width: 100%; text-align: center; color: rgba(248, 208, 128, 0.9); font-size: 10px; pointer-events: none; text-shadow: 0 0 5px #c02020, 0 0 2px #a04020; z-index: 5;">
        正在初始化系统...
    </div>

    <div id="webcam-wrapper">
        <canvas id="webcam-canvas" width="320" height="240"></canvas>
        <div id="cam-status"></div>
    </div>
    <video id="webcam-video" autoplay playsinline muted style="display:none"></video>

    <div id="delete-manager" class="hidden">
        <div class="manager-title">照片库管理</div>
        <div id="photo-grid"></div>
        <div class="manager-actions">
            <button class="elegant-btn btn-red" onclick="clearAllPhotos()">清空所有</button>
            <button class="elegant-btn" onclick="closeDeleteManager()">关闭</button>
        </div>
    </div>

    <script type="module">
        import * as THREE from 'three';
        import { EffectComposer } from 'three/addons/postprocessing/EffectComposer.js';
        import { RenderPass } from 'three/addons/postprocessing/RenderPass.js';
        import { UnrealBloomPass } from 'three/addons/postprocessing/UnrealBloomPass.js';
        import { RoomEnvironment } from 'three/addons/environments/RoomEnvironment.js'; 
        import { FilesetResolver, HandLandmarker } from '@mediapipe/tasks-vision';

        // 核心修改：纯黑背景 + 新年正红色调配置
        const CONFIG = {
            colors: { 
                bg: 0x000000, // 纯黑背景
                champagneGold: 0xf8d080, // 温润香槟金
                accentRed: 0xe02020,  // 正红（主色调）
                bronze: 0xc06020,     // 红铜色（更暖）
                darkRed: 0xa01010     // 深红（不突兀）
            },
            particles: { count: 1500, dustCount: 2500, treeHeight: 24, treeRadius: 8 },
            camera: { z: 50 },
            interaction: { rotationSpeed: 1.4, grabRadius: 0.55 }
        };

        const STATE = {
            mode: 'TREE', focusTarget: null,
            focusType: 0, 
            hand: { detected: false, x: 0, y: 0 },
            rotation: { x: 0, y: 0 },
            uiVisible: true, cameraVisible: true
        };

        const FONT_STYLES = {
            'style1': { font: "'Ma Shan Zheng', cursive", spacing: "4px", shadow: "2px 2px 8px rgba(220, 20, 20, 0.7), 0 0 5px rgba(248, 208, 128, 0.7)", transform: "none", weight: "normal" },
            'style2': { font: "'Cinzel', serif", spacing: "6px", shadow: "0 0 20px rgba(220, 20, 20, 0.7), 0 0 10px rgba(248, 208, 128, 0.7)", transform: "uppercase", weight: "700" },
            'style3': { font: "'Great Vibes', cursive", spacing: "1px", shadow: "0 0 15px rgba(220, 20, 20, 0.7), 0 0 5px rgba(192, 128, 96, 0.6)", transform: "none", weight: "normal" },
            'style4': { font: "'Monoton', cursive", spacing: "1px", shadow: "0 0 10px #f8d080, 0 0 20px #e02020, 0 0 5px #a04020", transform: "uppercase", weight: "normal" },
            'style5': { font: "'Abril Fatface', cursive", spacing: "0px", shadow: "0 5px 15px rgba(220, 20, 20, 0.7), 0 0 5px rgba(248, 208, 128, 0.7)", transform: "none", weight: "normal" }
        };

        // --- IndexedDB ---
        const DB_NAME = "GrandTreeDB_v16"; 
        let db;

        function initDB() {
            return new Promise((resolve) => {
                const request = indexedDB.open(DB_NAME, 1);
                request.onupgradeneeded = (e) => {
                    const db = e.target.result;
                    if (!db.objectStoreNames.contains('photos')) db.createObjectStore('photos', { keyPath: "id" });
                    if (!db.objectStoreNames.contains('music')) db.createObjectStore('music', { keyPath: "id" });
                };
                request.onsuccess = (e) => { db = e.target.result; resolve(db); };
                request.onerror = () => resolve(null);
            });
        }

        function savePhotoToDB(base64) {
            if(!db) return null;
            const tx = db.transaction('photos', "readwrite");
            const id = Date.now() + Math.random().toString();
            tx.objectStore('photos').add({ id: id, data: base64 });
            return id;
        }
        function loadPhotosFromDB() {
            if(!db) return Promise.resolve([]);
            return new Promise((r) => {
                db.transaction('photos', "readonly").objectStore('photos').getAll().onsuccess = (e) => r(e.target.result);
            });
        }
        function deletePhotoFromDB(id) { if(db) db.transaction('photos', "readwrite").objectStore('photos').delete(id); }
        function clearPhotosDB() { if(db) db.transaction('photos', "readwrite").objectStore('photos').clear(); }
        
        function saveMusicToDB(blob) {
            if(!db) return;
            const tx = db.transaction('music', "readwrite");
            tx.objectStore('music').put({ id: 'bgm', data: blob });
        }
        function loadMusicFromDB() {
            if(!db) return Promise.resolve(null);
            return new Promise((r) => {
                db.transaction('music', "readonly").objectStore('music').get('bgm').onsuccess = (e) => r(e.target.result ? e.target.result.data : null);
            });
        }

        let scene, camera, renderer, composer;
        let mainGroup, particleSystem = [], photoMeshGroup = new THREE.Group();
        let clock = new THREE.Clock();
        let handLandmarker, videoElement;
        let caneTexture; 
        let bgmAudio = new Audio(); bgmAudio.loop = true; let isMusicPlaying = false;

        async function init() {
            initThree();
            setupEnvironment(); 
            setupLights();
            createTextures();
            createParticles(); 
            createDust();
            createDefaultPhotos();
            setupPostProcessing();
            setupEvents();
            animate();

            const loader = document.getElementById('loader');
            if(loader) { loader.style.opacity = 0; setTimeout(() => loader.remove(), 500); }

            try {
                await initDB();
                loadTextConfig(); 
                
                const savedPhotos = await loadPhotosFromDB();
                if(savedPhotos && savedPhotos.length > 0) {
                    photoMeshGroup.clear();
                    particleSystem = particleSystem.filter(p => p.type !== 'PHOTO');
                    savedPhotos.forEach(item => createPhotoTexture(item.data, item.id));
                }

                const savedMusic = await loadMusicFromDB();
                if(savedMusic) {
                    bgmAudio.src = URL.createObjectURL(savedMusic);
                    updatePlayBtnUI(false); 
                }

            } catch(e) { console.warn("Init Warning", e); }

            initMediaPipe(); 
            initDraggableTitle(); 
        }

        function initDraggableTitle() {
            const title = document.getElementById('title-container');
            let isDragging = false;
            let offset = { x: 0, y: 0 };
            title.addEventListener('mousedown', (e) => {
                isDragging = true;
                const rect = title.getBoundingClientRect();
                offset.x = e.clientX - rect.left; offset.y = e.clientY - rect.top;
                title.style.transform = 'none'; title.style.left = rect.left + 'px'; title.style.top = rect.top + 'px';
            });
            window.addEventListener('mousemove', (e) => {
                if(isDragging) { title.style.left = (e.clientX - offset.x) + 'px'; title.style.top = (e.clientY - offset.y) + 'px'; }
            });
            window.addEventListener('mouseup', () => { isDragging = false; });
        }

        window.toggleUI = function() {
            STATE.uiVisible = !STATE.uiVisible;
            const tl = document.querySelector('.top-left-panel');
            const bl = document.querySelector('.bottom-left-panel');
            const gest = document.querySelector('.left-gesture-panel');
            const btn = document.getElementById('toggle-ui-btn');
            if(!STATE.uiVisible) {
                tl.classList.add('panel-hidden'); bl.classList.add('panel-hidden'); gest.classList.add('panel-hidden'); btn.innerText = "👁 显示界面";
            } else {
                tl.classList.remove('panel-hidden'); bl.classList.remove('panel-hidden'); gest.classList.remove('panel-hidden'); btn.innerText = "👁 隐藏界面";
            }
        }

        window.toggleCameraDisplay = function() {
            STATE.cameraVisible = !STATE.cameraVisible;
            const cam = document.getElementById('webcam-wrapper');
            if(STATE.cameraVisible) cam.classList.remove('camera-hidden'); else cam.classList.add('camera-hidden');
        }

        window.toggleFullScreen = function() {
            const btn = document.getElementById('fs-btn');
            if (!document.fullscreenElement) { 
                document.documentElement.requestFullscreen(); 
                btn.innerText = "⛶ 退出全屏";
            } else { 
                document.exitFullscreen(); 
                btn.innerText = "⛶ 全屏显示";
            }
        }
        document.addEventListener('fullscreenchange', () => {
            const btn = document.getElementById('fs-btn');
            if (!document.fullscreenElement) btn.innerText = "⛶ 全屏显示";
            else btn.innerText = "⛶ 退出全屏";
        });

        function loadTextConfig() {
            const saved = JSON.parse(localStorage.getItem('v16_text_config'));
            if(saved) {
                document.getElementById('input-line1').value = saved.line1 || "";
                document.getElementById('input-line2').value = saved.line2 || "";
                document.getElementById('font-select').value = saved.fontKey || "style1";
                document.getElementById('slider-fontsize').value = saved.size || 100;
                document.getElementById('color-picker').value = saved.color || "#f8d080";
                applyTextConfig(saved.fontKey, saved.line1, saved.line2, saved.size, saved.color);
            } else {
                document.getElementById('input-line1').value = "Merry"; document.getElementById('input-line2').value = "Christmas";
                applyTextConfig("style1", "Merry", "Christmas", 100, "#f8d080");
            }
        }

        window.updateTextConfig = function() {
            const key = document.getElementById('font-select').value;
            const l1 = document.getElementById('input-line1').value;
            const l2 = document.getElementById('input-line2').value;
            const s = document.getElementById('slider-fontsize').value;
            const c = document.getElementById('color-picker').value;
            localStorage.setItem('v16_text_config', JSON.stringify({ fontKey: key, line1: l1, line2: l2, size: s, color: c }));
            applyTextConfig(key, l1, l2, s, c);
        }

        function applyTextConfig(key, l1, l2, size, color) {
            const style = FONT_STYLES[key] || FONT_STYLES['style1'];
            const t1 = document.getElementById('display-line1');
            const t2 = document.getElementById('display-line2');
            t1.innerText = l1; t2.innerText = l2;
            const container = document.getElementById('title-container');
            container.style.fontFamily = style.font;
            t1.style.letterSpacing = style.spacing; t2.style.letterSpacing = style.spacing;
            t1.style.textShadow = style.shadow; t2.style.textShadow = style.shadow;
            t1.style.textTransform = style.transform; t2.style.textTransform = style.transform;
            t1.style.color = color; t2.style.color = color;
            t1.style.webkitTextFillColor = color; t2.style.webkitTextFillColor = color;
            t1.style.background = 'none'; t2.style.background = 'none';
            if(style.transform.includes('rotate')) { t1.style.transform = style.transform; t2.style.transform = style.transform; }
            else { t1.style.transform = 'none'; t2.style.transform = 'none'; }
            t1.style.fontSize = (0.48 * size) + "px"; t2.style.fontSize = (0.48 * size) + "px";
        }

        window.toggleMusicPlay = function() {
            if(!bgmAudio.src) return alert("请先在左下角上传音乐");
            if(isMusicPlaying) { bgmAudio.pause(); isMusicPlaying = false; } 
            else { bgmAudio.play(); isMusicPlaying = true; }
            updatePlayBtnUI(isMusicPlaying);
        }

        window.updateVolume = function(val) {
            bgmAudio.volume = val / 100;
            document.getElementById('val-vol').innerText = val;
        }

        function updatePlayBtnUI(playing) {
            const btn = document.getElementById('play-pause-btn');
            btn.innerText = playing ? "❚❚ 暂停音乐" : "▶ 播放音乐";
        }

        function initThree() {
            const container = document.getElementById('canvas-container');
            scene = new THREE.Scene();
            scene.background = new THREE.Color(CONFIG.colors.bg); // 纯黑背景
            scene.fog = new THREE.FogExp2(CONFIG.colors.bg, 0.01); 
            camera = new THREE.PerspectiveCamera(42, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.set(0, 2, CONFIG.camera.z); 
            renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false, powerPreference: "high-performance" });
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
            renderer.toneMapping = THREE.ReinhardToneMapping; 
            renderer.toneMappingExposure = 1.8; // 降低曝光，更柔和
            container.appendChild(renderer.domElement);
            mainGroup = new THREE.Group();
            scene.add(mainGroup);
        }

        function setupEnvironment() {
            const pmremGenerator = new THREE.PMREMGenerator(renderer);
            scene.environment = pmremGenerator.fromScene(new RoomEnvironment(), 0.04).texture;
        }

        // 新年正红色灯光系统
        function setupLights() {
            // 正红金环境光（更暖）
            scene.add(new THREE.AmbientLight(0xffddcc, 0.6)); 
            // 正红点光源（主）
            const innerLight = new THREE.PointLight(0xe02020, 3.0, 20); 
            innerLight.position.set(0, 5, 0); mainGroup.add(innerLight);
            // 香槟金聚光灯（更温润）
            const spotGold = new THREE.SpotLight(0xf8d080, 600); 
            spotGold.position.set(30, 40, 40); spotGold.angle = 0.5; spotGold.penumbra = 0.6; scene.add(spotGold);
            // 正红聚光灯（主）
            const spotRed = new THREE.SpotLight(0xe02020, 600); 
            spotRed.position.set(-30, 20, -30); scene.add(spotRed);
            // 红铜色补光
            const fillBronze = new THREE.DirectionalLight(0xc06020, 0.4);
            fillBronze.position.set(20, -10, -20); scene.add(fillBronze);
            // 正红填充光
            const fillRed = new THREE.DirectionalLight(0xc02020, 0.4); 
            fillRed.position.set(0, 0, 50); scene.add(fillRed);
        }

        function setupPostProcessing() {
            const renderScene = new RenderPass(scene, camera);
            const bloomPass = new UnrealBloomPass(new THREE.Vector2(window.innerWidth, window.innerHeight), 1.2, 0.5, 0.85);
            bloomPass.threshold = 0.75; // 提高阈值，减少过度光晕
            bloomPass.strength = 0.5; // 降低光晕强度，更自然
            bloomPass.radius = 0.4;
            composer = new EffectComposer(renderer);
            composer.addPass(renderScene); composer.addPass(bloomPass);
        }

        // 新年正红金糖果纹理
        function createTextures() {
            const canvas = document.createElement('canvas'); canvas.width = 128; canvas.height = 128;
            const ctx = canvas.getContext('2d');
            ctx.fillStyle = '#ffe8e8'; // 浅红底（偏正红）
            ctx.fillRect(0,0,128,128);
            // 正红条纹
            ctx.fillStyle = '#e02020'; // 正红
            ctx.beginPath();
            for(let i=-128; i<256; i+=32) { 
                ctx.moveTo(i, 0); ctx.lineTo(i+32, 128); ctx.lineTo(i+16, 128); ctx.lineTo(i-16, 0); 
            }
            ctx.fill();
            
            // 香槟金细条纹
            ctx.fillStyle = '#f8d080';
            ctx.beginPath();
            for(let i=-100; i<256; i+=32) { 
                ctx.moveTo(i+8, 0); ctx.lineTo(i+24, 128); ctx.lineTo(i+16, 128); ctx.lineTo(i, 0); 
            }
            ctx.fill();
            
            // 淡金描边
            ctx.strokeStyle = '#f8d080';
            ctx.lineWidth = 2;
            ctx.strokeRect(1,1,126,126);
            
            caneTexture = new THREE.CanvasTexture(canvas);
            caneTexture.wrapS = caneTexture.wrapT = THREE.RepeatWrapping; caneTexture.repeat.set(3, 3);
        }

        class Particle {
            constructor(mesh, type, isDust = false) {
                this.mesh = mesh; this.type = type; this.isDust = isDust;
                this.posTree = new THREE.Vector3(); this.posScatter = new THREE.Vector3();
                this.baseScale = mesh.scale.x; 
                this.photoId = null;
                const speedMult = (type === 'PHOTO') ? 0.3 : 2.0;
                this.spinSpeed = new THREE.Vector3((Math.random()-0.5)*speedMult, (Math.random()-0.5)*speedMult, (Math.random()-0.5)*speedMult);
                this.calculatePositions();
            }

            calculatePositions() {
                const h = CONFIG.particles.treeHeight;
                let t = Math.pow(Math.random(), 0.8); 
                const y = (t * h) - (h/2);
                let rMax = Math.max(0.5, CONFIG.particles.treeRadius * (1.0 - t));
                const angle = t * 50 * Math.PI + Math.random() * Math.PI; 
                const r = rMax * (0.8 + Math.random() * 0.4); 
                this.posTree.set(Math.cos(angle) * r, y, Math.sin(angle) * r);

                let rScatter = this.isDust ? (12 + Math.random()*20) : (8 + Math.random()*12);
                const theta = Math.random() * Math.PI * 2;
                const phi = Math.acos(2 * Math.random() - 1);
                this.posScatter.set(rScatter * Math.sin(phi) * Math.cos(theta), rScatter * Math.sin(phi) * Math.sin(theta), rScatter * Math.cos(phi));
            }

            update(dt, mode, focusTargetMesh) {
                let target = this.posTree;
                if (mode === 'SCATTER') target = this.posScatter;
                else if (mode === 'FOCUS') {
                    if (this.mesh === focusTargetMesh) {
                        let offset = new THREE.Vector3(0, 1, 38); 
                        if (STATE.focusType === 1) offset.set(-4, 2, 35); 
                        else if (STATE.focusType === 2) offset.set(3, 0, 32); 
                        else if (STATE.focusType === 3) offset.set(0, -2.5, 30); 
                        const invMatrix = new THREE.Matrix4().copy(mainGroup.matrixWorld).invert();
                        target = offset.applyMatrix4(invMatrix);
                    } else target = this.posScatter;
                }

                const lerpSpeed = (mode === 'FOCUS' && this.mesh === focusTargetMesh) ? 8.0 : 4.0; 
                this.mesh.position.lerp(target, lerpSpeed * dt);

                if (mode === 'SCATTER') {
                    this.mesh.rotation.x += this.spinSpeed.x * dt;
                    this.mesh.rotation.y += this.spinSpeed.y * dt;
                    this.mesh.rotation.z += this.spinSpeed.z * dt;
                } else if (mode === 'TREE') {
                    this.mesh.rotation.x = THREE.MathUtils.lerp(this.mesh.rotation.x, 0, dt);
                    this.mesh.rotation.z = THREE.MathUtils.lerp(this.mesh.rotation.z, 0, dt);
                    this.mesh.rotation.y += 0.5 * dt; 
                }
                
                if (mode === 'FOCUS' && this.mesh === focusTargetMesh) {
                    this.mesh.lookAt(camera.position); 
                    if(STATE.focusType === 1) this.mesh.rotateZ(0.38); 
                    if(STATE.focusType === 2) this.mesh.rotateZ(-0.15); 
                    if(STATE.focusType === 3) this.mesh.rotateX(-0.4); 
                }

                let s = this.baseScale;
                if (this.isDust) {
                    s = this.baseScale * (0.8 + 0.4 * Math.sin(clock.elapsedTime * 4 + this.mesh.id));
                    if (mode === 'TREE') s = 0; 
                } else if (mode === 'SCATTER' && this.type === 'PHOTO') s = this.baseScale * 2.5; 
                else if (mode === 'FOCUS') {
                    if (this.mesh === focusTargetMesh) {
                        if(STATE.focusType === 2) s = 3.5; 
                        else if(STATE.focusType === 3) s = 4.8; 
                        else s = 3.0; 
                    }
                    else s = this.baseScale * 0.8; 
                }
                this.mesh.scale.lerp(new THREE.Vector3(s,s,s), 6*dt);
            }
        }

        // 新年正红金粒子系统（增加正红占比）
        function createParticles() {
            const sphereGeo = new THREE.SphereGeometry(0.5, 32, 32); 
            const boxGeo = new THREE.BoxGeometry(0.55, 0.55, 0.55); 
            const curve = new THREE.CatmullRomCurve3([ new THREE.Vector3(0, -0.5, 0), new THREE.Vector3(0, 0.3, 0), new THREE.Vector3(0.1, 0.5, 0), new THREE.Vector3(0.3, 0.4, 0) ]);
            const candyGeo = new THREE.TubeGeometry(curve, 16, 0.08, 8, false);

            // 材质定义：新年正红金系
            const goldMat = new THREE.MeshStandardMaterial({ 
                color: CONFIG.colors.champagneGold, 
                metalness: 0.8,
                roughness: 0.2, 
                envMapIntensity: 1.8, 
                emissive: 0xc0a060,
                emissiveIntensity: 0.4
            });
            const deepRedMat = new THREE.MeshStandardMaterial({ 
                color: CONFIG.colors.darkRed, 
                metalness: 0.1, 
                roughness: 0.85, 
                emissive: 0x801010, // 深红自发光
                emissiveIntensity: 0.25
            });
            const brightRedMat = new THREE.MeshPhysicalMaterial({ 
                color: CONFIG.colors.accentRed, 
                metalness: 0.2,
                roughness: 0.3, 
                clearcoat: 0.8,
                emissive: 0xa01010, // 正红自发光
                emissiveIntensity: 0.3
            });
            const bronzeMat = new THREE.MeshStandardMaterial({ 
                color: CONFIG.colors.bronze, 
                metalness: 0.7,
                roughness: 0.3, 
                emissive: 0x804010, // 红铜自发光
                emissiveIntensity: 0.3
            });
            const candyMat = new THREE.MeshStandardMaterial({ map: caneTexture, roughness: 0.5 });
            // 顶部香槟金星星材质
            const topGoldMat = new THREE.MeshStandardMaterial({ 
                color: CONFIG.colors.champagneGold, 
                metalness: 0.8, 
                roughness: 0.2,
                emissive: 0xf8d080, 
                emissiveIntensity: 0.6
            });

            for (let i = 0; i < CONFIG.particles.count; i++) {
                const rand = Math.random();
                let mesh, type;
                const h = CONFIG.particles.treeHeight;
                let t = Math.pow(Math.random(), 0.8); 
                const y = (t * h) - (h/2);
                const isTopArea = y > h * 0.3;

                if (rand < 0.55) { 
                    // 深红色方块（主）- 大幅增加正红占比
                    mesh = new THREE.Mesh(boxGeo, deepRedMat); 
                    type = 'RED_BOX'; 
                }
                else if (rand < 0.70) { 
                    // 香槟金方块（点缀）
                    mesh = new THREE.Mesh(boxGeo, goldMat); 
                    type = 'GOLD_BOX'; 
                }
                else if (rand < 0.82) { 
                    // 红铜色球体（过渡）
                    mesh = new THREE.Mesh(sphereGeo, bronzeMat); 
                    type = 'BRONZE_SPHERE'; 
                }
                else if (rand < 0.96) { 
                    // 正红色球体（强调）- 增加占比
                    mesh = new THREE.Mesh(sphereGeo, brightRedMat); 
                    type = 'BRIGHT_RED'; 
                }
                else { 
                    // 正红金糖果
                    mesh = new THREE.Mesh(candyGeo, candyMat); 
                    type = 'COLOR_CANE'; 
                }

                const s = 0.4 + Math.random() * 0.5;
                mesh.scale.set(s,s,s); mesh.rotation.set(Math.random()*6, Math.random()*6, Math.random()*6);
                mainGroup.add(mesh); particleSystem.push(new Particle(mesh, type, false));
            }

            // 顶部香槟金星星
            const star = new THREE.Mesh(new THREE.OctahedronGeometry(1.2, 0), topGoldMat);
            star.position.set(0, CONFIG.particles.treeHeight/2 + 1.2, 0); mainGroup.add(star);
            mainGroup.add(photoMeshGroup);
        }

        // 新年正红金星尘
        function createDust() {
            const geo = new THREE.TetrahedronGeometry(0.08, 0);
            // 正红/香槟金/红铜色星尘
            const colors = [
                new THREE.MeshBasicMaterial({ color: 0xe08080, transparent: true, opacity: 0.85 }), // 浅正红
                new THREE.MeshBasicMaterial({ color: CONFIG.colors.champagneGold, transparent: true, opacity: 0.75 }), // 香槟金
                new THREE.MeshBasicMaterial({ color: CONFIG.colors.bronze, transparent: true, opacity: 0.7 }) // 红铜色
            ];
            
            for(let i=0; i<CONFIG.particles.dustCount; i++) {
                 // 随机选择正红/金/红铜色
                 const mat = colors[Math.floor(Math.random() * colors.length)];
                 const mesh = new THREE.Mesh(geo, mat); 
                 mesh.scale.setScalar(0.5 + Math.random());
                 mainGroup.add(mesh); 
                 particleSystem.push(new Particle(mesh, 'DUST', true));
            }
        }

        // 新年正红风格默认照片
        function createDefaultPhotos() {
            const canvas = document.createElement('canvas'); canvas.width = 512; canvas.height = 512;
            const ctx = canvas.getContext('2d');
            ctx.fillStyle = '#000000'; // 纯黑背景
            ctx.fillRect(0,0,512,512);
            // 香槟金边框
            ctx.strokeStyle = '#f8d080'; 
            ctx.lineWidth = 15; 
            ctx.strokeRect(20,20,472,472);
            // 红铜色细边框
            ctx.strokeStyle = '#c06020';
            ctx.lineWidth = 3;
            ctx.strokeRect(30,30,452,452);
            // 香槟金文字
            ctx.font = '500 60px Times New Roman'; 
            ctx.fillStyle = '#f8d080'; 
            ctx.textAlign = 'center'; 
            ctx.fillText("JOYEUX", 256, 230); 
            ctx.fillText("NOEL", 256, 300);
            // 正红描边
            ctx.strokeStyle = '#e02020';
            ctx.lineWidth = 1;
            ctx.strokeText("JOYEUX", 256, 230);
            ctx.strokeText("NOEL", 256, 300);
            
            createPhotoTexture(canvas.toDataURL(), 'default');
        }

        function createPhotoTexture(base64, id) {
            const img = new Image();
            img.src = base64;
            img.onload = () => {
                const tex = new THREE.Texture(img);
                tex.colorSpace = THREE.SRGBColorSpace;
                tex.needsUpdate = true;
                addPhotoToScene(tex, id, base64);
            }
        }

        function addPhotoToScene(texture, id, base64) {
            const frameGeo = new THREE.BoxGeometry(1.4, 1.4, 0.05);
            // 香槟金相框
            const frameMat = new THREE.MeshStandardMaterial({ color: CONFIG.colors.champagneGold, metalness: 0.8, roughness: 0.2 });
            const frame = new THREE.Mesh(frameGeo, frameMat);
            const photoGeo = new THREE.PlaneGeometry(1.2, 1.2);
            const photoMat = new THREE.MeshBasicMaterial({ map: texture });
            const photo = new THREE.Mesh(photoGeo, photoMat);
            photo.position.z = 0.04;
            const group = new THREE.Group();
            group.add(frame); group.add(photo);
            const s = 0.8; group.scale.set(s,s,s);
            
            photoMeshGroup.add(group);
            const p = new Particle(group, 'PHOTO', false);
            p.photoId = id; 
            p.texture = texture; 
            particleSystem.push(p);
        }

        window.applyParticleSettings = function() {
            const photos = particleSystem.filter(p => p.type === 'PHOTO');
            const toRemove = [];
            mainGroup.children.forEach(c => {
                if(c !== photoMeshGroup) toRemove.push(c);
            });
            toRemove.forEach(c => mainGroup.remove(c));
            particleSystem = [...photos];
            CONFIG.particles.count = parseInt(document.getElementById('slider-tree').value);
            CONFIG.particles.dustCount = parseInt(document.getElementById('slider-dust').value);
            createParticles();
            createDust();
        }

        async function initMediaPipe() {
            videoElement = document.getElementById('webcam-video');
            const hint = document.getElementById('gesture-hint');
            if (navigator.mediaDevices?.getUserMedia) {
                try {
                    const stream = await navigator.mediaDevices.getUserMedia({ video: true });
                    videoElement.srcObject = stream;
                    videoElement.onloadedmetadata = () => {
                        videoElement.play(); 
                        renderWebcamPreview(); 
                    };
                } catch (e) {
                    console.error("Camera denied:", e);
                    hint.innerText = "Camera Access Denied";
                }
            }
            try {
                const vision = await FilesetResolver.forVisionTasks("https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.3/wasm");
                handLandmarker = await HandLandmarker.createFromOptions(vision, {
                    baseOptions: { modelAssetPath: `https://storage.googleapis.com/mediapipe-models/hand_landmarker/hand_landmarker/float16/1/hand_landmarker.task`, delegate: "GPU" },
                    runningMode: "VIDEO", numHands: 1
                });
                hint.innerText = "手势识别就绪";
                predictWebcam();
            } catch(e) { console.warn("AI Load Failed:", e); hint.innerText = "AI Failed"; }
        }

        function renderWebcamPreview() {
            const canvas = document.getElementById('webcam-canvas');
            const ctx = canvas.getContext('2d', { willReadFrequently: true });
            function draw() {
                if(videoElement.readyState >= 2) ctx.drawImage(videoElement, 0, 0, canvas.width, canvas.height);
                requestAnimationFrame(draw);
            }
            draw();
        }

        let lastVideoTime = -1;
        async function predictWebcam() {
            if (videoElement && videoElement.currentTime !== lastVideoTime && handLandmarker) {
                lastVideoTime = videoElement.currentTime;
                const result = handLandmarker.detectForVideo(videoElement, performance.now());
                processGestures(result);
                if(result.landmarks.length > 0) document.getElementById('cam-status').classList.add('active');
                else document.getElementById('cam-status').classList.remove('active');
            }
            requestAnimationFrame(predictWebcam);
        }

        // --- REALTIME ZERO-LATENCY GESTURES ---
        function processGestures(result) {
            const hint = document.getElementById('gesture-hint');
            if (result.landmarks && result.landmarks.length > 0) {
                STATE.hand.detected = true;
                const lm = result.landmarks[0];
                STATE.hand.x = (lm[9].x - 0.5) * 2; 
                STATE.hand.y = (lm[9].y - 0.5) * 2;

                const thumb = lm[4]; const index = lm[8]; const wrist = lm[0];
                const pinchDist = Math.hypot(thumb.x - index.x, thumb.y - index.y);
                const tips = [lm[8], lm[12], lm[16], lm[20]];
                let openDist = 0; tips.forEach(t => openDist += Math.hypot(t.x - wrist.x, t.y - wrist.y)); openDist /= 4;

                // Immediate switching without buffer
                if (pinchDist < 0.05) {
                    hint.innerText = "状态: 抓取 / 聚焦";
                    if (STATE.mode !== 'FOCUS') {
                        let closestPhoto = null; let minScreenDist = Infinity;
                        
                        STATE.focusType = Math.floor(Math.random() * 4);

                        particleSystem.filter(p => p.type === 'PHOTO').forEach(p => {
                            p.mesh.updateMatrixWorld();
                            const pos = new THREE.Vector3(); p.mesh.getWorldPosition(pos);
                            const screenPos = pos.project(camera); 
                            const dist = Math.hypot(screenPos.x, screenPos.y);
                            if (screenPos.z < 1 && dist < CONFIG.interaction.grabRadius && dist < minScreenDist) {
                                minScreenDist = dist; closestPhoto = p.mesh;
                            }
                        });
                        if (closestPhoto) { STATE.mode = 'FOCUS'; STATE.focusTarget = closestPhoto; }
                    }
                } 
                else if (openDist < 0.25) { STATE.mode = 'TREE'; STATE.focusTarget = null; hint.innerText = "状态: 聚合 (新年正红树)"; } 
                else if (openDist > 0.4) { STATE.mode = 'SCATTER'; STATE.focusTarget = null; hint.innerText = "状态: 散开 (新年星云)"; }
            } else {
                STATE.hand.detected = false; hint.innerText = "等待手势...";
            }
        }

        window.setupEvents = function() {
            window.addEventListener('resize', () => { camera.aspect = window.innerWidth/window.innerHeight; camera.updateProjectionMatrix(); renderer.setSize(window.innerWidth, window.innerHeight); composer.setSize(window.innerWidth, window.innerHeight); });
            document.getElementById('file-input').addEventListener('change', (e) => {
                const files = e.target.files;
                if(!files.length) return;
                Array.from(files).forEach(f => {
                    const reader = new FileReader();
                    reader.onload = (ev) => {
                        const base64 = ev.target.result;
                        const id = savePhotoToDB(base64);
                        createPhotoTexture(base64, id);
                    }
                    reader.readAsDataURL(f);
                });
            });
            document.getElementById('music-input').addEventListener('change', (e) => {
                const file = e.target.files[0];
                if (file) {
                    saveMusicToDB(file);
                    bgmAudio.src = URL.createObjectURL(file);
                    bgmAudio.play().then(() => { isMusicPlaying = true; updatePlayBtnUI(true); }).catch(console.error);
                }
            });
            window.addEventListener('keydown', (e) => { if (e.key.toLowerCase() === 'h') window.toggleUI(); });
        }

        window.openDeleteManager = async function() {
            const modal = document.getElementById('delete-manager');
            const grid = document.getElementById('photo-grid');
            grid.innerHTML = '';
            const photos = await loadPhotosFromDB();
            if(!photos || photos.length === 0) grid.innerHTML = '<div style="color:#f8d080;">暂无照片</div>';
            else {
                photos.forEach((p) => {
                    const div = document.createElement('div'); div.className = 'photo-item';
                    const img = document.createElement('img'); img.className = 'photo-thumb';
                    img.src = p.data;
                    const btn = document.createElement('div'); btn.className = 'delete-x'; btn.innerText = 'X';
                    btn.onclick = () => confirmDelete(p.id, divElement);
                    div.appendChild(img); div.appendChild(btn); grid.appendChild(div);
                });
            }
            modal.classList.remove('hidden');
        }

        window.confirmDelete = function(id, divElement) {
            deletePhotoFromDB(id);
            divElement.remove();
            const p = particleSystem.find(part => part.photoId === id);
            if(p) { photoMeshGroup.remove(p.mesh); particleSystem.splice(particleSystem.indexOf(p), 1); }
        }

        window.clearAllPhotos = function() {
            if(confirm("确定要清空所有照片吗？")) {
                clearPhotosDB();
                particleSystem.filter(p => p.type === 'PHOTO').forEach(p => photoMeshGroup.remove(p.mesh));
                particleSystem = particleSystem.filter(p => p.type !== 'PHOTO');
                window.openDeleteManager();
            }
        }

        window.closeDeleteManager = function() { document.getElementById('delete-manager').classList.add('hidden'); }

        function animate() {
            requestAnimationFrame(animate);
            const dt = clock.getDelta();

            if (STATE.mode === 'SCATTER' && STATE.hand.detected) {
                const threshold = 0.3; 
                const speed = CONFIG.interaction.rotationSpeed; 
                
                if (STATE.hand.x > threshold) STATE.rotation.y -= speed * dt * (STATE.hand.x - threshold);
                else if (STATE.hand.x < -threshold) STATE.rotation.y -= speed * dt * (STATE.hand.x + threshold);
                
                if (STATE.hand.y < -threshold) STATE.rotation.x += speed * dt * (-STATE.hand.y - threshold); 
                else if (STATE.hand.y > threshold) STATE.rotation.x -= speed * dt * (STATE.hand.y - threshold);
            } else {
                if(STATE.mode === 'TREE') {
                    STATE.rotation.y += 0.3 * dt;
                    STATE.rotation.x += (0 - STATE.rotation.x) * 2.0 * dt;
                } else {
                     STATE.rotation.y += 0.1 * dt; 
                }
            }

            mainGroup.rotation.y = STATE.rotation.y;
            mainGroup.rotation.x = STATE.rotation.x;

            particleSystem.forEach(p => p.update(dt, STATE.mode, STATE.focusTarget));
            composer.render();
        }

        init();
    </script>
</body>
</html>
