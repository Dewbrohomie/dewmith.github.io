# dewmith.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Envelope Opening & Video Animation</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400..700;1,400..700&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #ffffff;
            color: #1c1917;
            overflow-x: hidden;
        }
        .serif-font {
            font-family: 'Playfair Display', serif;
        }
        .envelope-container {
            perspective: 1200px;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center justify-between p-6 bg-white">

    <header class="text-center my-4">
        <h1 class="serif-font text-3xl md:text-4xl font-semibold tracking-wide text-stone-800 drop-shadow-sm">Invitation Unveiling</h1>
        <p class="text-xs md:text-sm text-stone-500 mt-1 uppercase tracking-widest">Tap the envelope to open and play video</p>
    </header>

    <main class="w-full max-w-md bg-stone-50/80 border border-stone-200 backdrop-blur-xl rounded-3xl p-6 md:p-8 shadow-xl flex flex-col items-center justify-center relative my-auto">
        
        <!-- Status indicator badge -->
        <div id="status-badge" class="absolute top-4 px-3 py-1 bg-amber-100 border border-amber-300 text-amber-800 rounded-full text-xs font-medium tracking-wide uppercase transition-all duration-300 z-20 shadow-sm">
            Step 1: Closed Envelope
        </div>

        <!-- Image Display Stage with smooth crossfade & scale -->
        <div class="w-full aspect-square relative flex items-center justify-center my-8 envelope-container cursor-pointer group" onclick="nextStep()">
            
            <!-- Closed Envelope (poster.jpg) -->
            <div id="step-1-wrapper" class="absolute inset-0 flex items-center justify-center transition-all duration-700 ease-in-out transform scale-100 opacity-100">
                <div class="relative w-full h-full rounded-2xl overflow-hidden flex items-center justify-center">
                    <img id="img-closed" src="poster.jpg" alt="Closed Envelope" class="w-full h-full object-contain filter drop-shadow-[0_15px_25px_rgba(0,0,0,0.15)] group-hover:scale-105 transition-transform duration-500" style="mix-blend-mode: multiply; filter: contrast(1.05) brightness(1.02);" onerror="this.src='https://placehold.co/600x600/f5f5f0/333333?text=Closed+Envelope'">
                </div>
            </div>

            <!-- Open Envelope with Video Inside (floral-liner.png + Video Player) -->
            <div id="step-2-wrapper" class="absolute inset-0 flex items-center justify-center transition-all duration-700 ease-in-out transform scale-95 opacity-0 pointer-events-none">
                <div class="relative w-full h-full rounded-2xl overflow-hidden shadow-lg flex items-center justify-center bg-stone-100 border border-stone-200">
                    <!-- Background open envelope graphic -->
                    <img id="img-open" src="floral-liner.png" alt="Open Envelope" class="absolute inset-0 w-full h-full object-contain opacity-50 filter blur-[0.5px]" onerror="this.src='https://placehold.co/600x600/f5f5f0/333333?text=Open+Envelope'">
                    
                    <!-- Video Container Area inside the envelope opening -->
                    <div class="relative z-10 w-[78%] h-[60%] rounded-xl overflow-hidden shadow-2xl border border-stone-300 bg-black flex items-center justify-center group/video">
                        <video id="envelope-video" class="w-full h-full object-cover" controls playsinline loop>
                            <!-- Replace with your actual video source or upload URL -->
                            <source src="https://assets.mixkit.co/videos/preview/mixkit-set-of-plate-ceramic-crafts-42939-large.mp4" type="video/mp4">
                            Your browser does not support the video tag.
                        </video>
                    </div>
                </div>
            </div>

        </div>

        <div class="w-full flex flex-col gap-3 mt-2">
            <button id="action-btn" onclick="nextStep()" class="w-full py-3.5 px-6 bg-stone-900 text-stone-100 font-semibold rounded-xl shadow-md hover:bg-stone-800 active:scale-95 transition-all duration-300 text-sm tracking-wider uppercase flex items-center justify-center gap-2">
                <span>Tap to Open & Play</span>
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
                </svg>
            </button>
            
            <!-- Video File Input to allow uploading custom video -->
            <label class="w-full py-2.5 px-4 bg-white hover:bg-stone-100 border border-stone-200 text-stone-700 rounded-xl text-xs tracking-wider uppercase text-center cursor-pointer transition-all duration-300 flex items-center justify-center gap-2 shadow-sm">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-3.5 w-3.5 text-stone-600" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12"/></svg>
                <span>Upload Custom Video</span>
                <input type="file" id="video-upload" accept="video/*" class="hidden" onchange="handleVideoUpload(event)">
            </label>

            <button onclick="resetAnimation()" class="w-full py-2 px-6 bg-transparent text-stone-400 hover:text-stone-600 transition-all duration-300 text-xs tracking-wider uppercase">
                Reset State
            </button>
        </div>

    </main>

    <footer class="text-xs text-stone-400 text-center my-2">
        Clean Minimalist White Theme &bull; HTML5 Video Player Integration
    </footer>

    <script>
        let currentStep = 1;

        function removeWhiteBackground(imgElement) {
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');
            const img = new Image();
            img.crossOrigin = "anonymous";
            img.src = imgElement.src;
            
            img.onload = function() {
                canvas.width = img.naturalWidth || img.width;
                canvas.height = img.naturalHeight || img.height;
                ctx.drawImage(img, 0, 0);
                
                try {
                    const imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);
                    const data = imgData.data;
                    
                    for (let i = 0; i < data.length; i += 4) {
                        let r = data[i], g = data[i+1], b = data[i+2];
                        if (r > 235 && g > 235 && b > 230) {
                            data[i+3] = 0;
                        }
                    }
                    
                    ctx.putImageData(imgData, 0, 0);
                    imgElement.src = canvas.toDataURL("image/png");
                } catch (e) {
                    console.log("Canvas background removal handled via blend mode fallback.");
                }
            };
        }

        window.addEventListener('DOMContentLoaded', () => {
            removeWhiteBackground(document.getElementById('img-closed'));
            removeWhiteBackground(document.getElementById('img-open'));
        });

        function nextStep() {
            const step1 = document.getElementById('step-1-wrapper');
            const step2 = document.getElementById('step-2-wrapper');
            const badge = document.getElementById('status-badge');
            const btn = document.getElementById('action-btn');
            const video = document.getElementById('envelope-video');

            if (currentStep === 1) {
                currentStep = 2;
                
                step1.classList.remove('scale-100', 'opacity-100');
                step1.classList.add('scale-95', 'opacity-0', 'pointer-events-none');
                
                step2.classList.remove('scale-95', 'opacity-0', 'pointer-events-none');
                step2.classList.add('scale-100', 'opacity-100');

                badge.innerText = "Step 2: Envelope Open & Playing";
                badge.className = "absolute top-4 px-3 py-1 bg-emerald-100 border border-emerald-300 text-emerald-800 rounded-full text-xs font-medium tracking-wide uppercase transition-all duration-300 z-20 shadow-sm";
                
                btn.innerHTML = `<span>Close Envelope</span><svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"/></svg>`;

                // Automatically start playing the video when opened
                video.play().catch(e => console.log("Autoplay prevented by browser policy:", e));
            } else {
                resetAnimation();
            }
        }

        function resetAnimation() {
            currentStep = 1;
            const step1 = document.getElementById('step-1-wrapper');
            const step2 = document.getElementById('step-2-wrapper');
            const badge = document.getElementById('status-badge');
            const btn = document.getElementById('action-btn');
            const video = document.getElementById('envelope-video');

            step1.classList.remove('scale-95', 'opacity-0', 'pointer-events-none');
            step1.classList.add('scale-100', 'opacity-100');
            
            step2.classList.remove('scale-100', 'opacity-100');
            step2.classList.add('scale-95', 'opacity-0', 'pointer-events-none');

            badge.innerText = "Step 1: Closed Envelope";
            badge.className = "absolute top-4 px-3 py-1 bg-amber-100 border border-amber-300 text-amber-800 rounded-full text-xs font-medium tracking-wide uppercase transition-all duration-300 z-20 shadow-sm";
            
            btn.innerHTML = `<span>Tap to Open & Play</span><svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3"/></svg>`;

            video.pause();
            video.currentTime = 0;
        }

        function handleVideoUpload(event) {
            const file = event.target.files[0];
            if (file) {
                const videoURL = URL.createObjectURL(file);
                const video = document.getElementById('envelope-video');
                video.src = videoURL;
                video.load();
            }
        }
    </script>
</body>
</html>
