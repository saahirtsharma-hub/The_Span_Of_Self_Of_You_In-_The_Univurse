<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Span of Self</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            background-color: #050508;
            color: white;
            margin: 0;
            font-family: system-ui, -apple-system, sans-serif;
            overflow-x: hidden;
        }

        /* The long track that creates the scrollbar */
        .scroll-tracker {
            height: 800vh; 
            position: relative;
        }

        /* The container that stays still while you scroll */
        .viewport {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            perspective: 1000px;
        }

        .stage {
            position: absolute;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            width: 100%;
            padding: 20px;
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.1s linear;
            will-change: transform, opacity;
        }

        .icon-box {
            font-size: 8rem;
            margin-bottom: 20px;
            filter: drop-shadow(0 0 20px rgba(59, 130, 246, 0.5));
        }

        .progress-line {
            position: fixed;
            top: 0;
            left: 0;
            height: 4px;
            background: #3b82f6;
            z-index: 100;
            transition: width 0.1s ease-out;
        }

        .grid-overlay {
            position: fixed;
            inset: 0;
            background-image: radial-gradient(circle at 2px 2px, rgba(255,255,255,0.05) 1px, transparent 0);
            background-size: 40px 40px;
            z-index: -1;
        }
    </style>
</head>
<body>

    <div class="progress-line" id="bar"></div>
    <div class="grid-overlay"></div>

    <div class="scroll-tracker">
        <div class="viewport">
            
            <div class="stage" id="s1">
                <div class="icon-box">👶</div>
                <h1 class="text-5xl font-black mb-4 uppercase tracking-tighter">The Beginning</h1>
                <p class="text-blue-400 font-mono tracking-widest">SCALE: 50 CENTIMETERS</p>
                <p class="max-w-md mt-4 text-gray-400">Everything starts small. A single life, weighing no more than a few bags of sugar.</p>
            </div>

            <div class="stage" id="s2">
                <div class="icon-box">🚲</div>
                <h1 class="text-5xl font-black mb-4 uppercase tracking-tighter">The Playground</h1>
                <p class="text-blue-400 font-mono tracking-widest">SCALE: 10 YEARS</p>
                <p class="max-w-md mt-4 text-gray-400">By now, you've walked roughly 15,000 miles—enough to walk halfway across the world.</p>
            </div>

            <div class="stage" id="s3">
                <div class="icon-box">🏢</div>
                <h1 class="text-5xl font-black mb-4 uppercase tracking-tighter">The Build</h1>
                <p class="text-blue-400 font-mono tracking-widest">SCALE: 40 YEARS</p>
                <p class="max-w-md mt-4 text-gray-400">You have spent 13 years of your life at work. Your heart has beaten 1.5 billion times.</p>
            </div>

            <div class="stage" id="s4">
                <div class="icon-box">🌌</div>
                <h1 class="text-5xl font-black mb-4 uppercase tracking-tighter">The Horizon</h1>
                <p class="text-blue-400 font-mono tracking-widest">SCALE: 80 YEARS</p>
                <p class="max-w-md mt-4 text-gray-400">A full human life is just 0.0000002% of the age of the Earth. Make it count.</p>
            </div>

        </div>
    </div>

    <div class="fixed bottom-8 w-full text-center text-gray-500 uppercase text-xs tracking-[0.5em] animate-pulse">
        Scroll Down to Zoom Out
    </div>

    <script>
        const stages = [
            { el: document.getElementById('s1'), start: 0, end: 0.25 },
            { el: document.getElementById('s2'), start: 0.25, end: 0.5 },
            { el: document.getElementById('s3'), start: 0.5, end: 0.75 },
            { el: document.getElementById('s4'), start: 0.75, end: 1.0 }
        ];

        const progressBar = document.getElementById('bar');

        window.addEventListener('scroll', () => {
            // Get scroll percentage (0 to 1)
            const scrollTop = window.pageYOffset || document.documentElement.scrollTop;
            const scrollHeight = document.documentElement.scrollHeight - document.documentElement.clientHeight;
            const progress = scrollTop / scrollHeight;

            // Update top bar
            progressBar.style.width = (progress * 100) + "%";

            stages.forEach((stage) => {
                if (progress >= stage.start && progress <= stage.end) {
                    // Calculate how far we are into THIS specific stage (0 to 1)
                    const sectionProgress = (progress - stage.start) / (stage.end - stage.start);
                    
                    // Zoom effect: Start at 2.0 scale, end at 0.5
                    const scale = 2 - (sectionProgress * 1.5);
                    
                    // Fade effect: Fade in at start, fade out at end
                    let opacity = 1;
                    if (sectionProgress < 0.2) opacity = sectionProgress * 5;
                    if (sectionProgress > 0.8) opacity = (1 - sectionProgress) * 5;

                    stage.el.style.opacity = opacity;
                    stage.el.style.transform = `scale(${scale})`;
                } else {
                    stage.el.style.opacity = 0;
                }
            });
        });

        // Initialize first stage on load
        window.dispatchEvent(new Event('scroll'));
    </script>
</body>
</html>
