<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Chel duh tuk bik pg muh</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    html, body { width: 100%; height: 100%; overflow: hidden; font-family: 'Arial', sans-serif; }

    /* Animated gradient background */
    body {
      background: linear-gradient(-45deg, #ff9a9e, #fad0c4, #a18cd1, #fbc2eb);
      background-size: 400% 400%;
      animation: gradientShift 20s ease infinite;
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;
      color: white;
    }

    @keyframes gradientShift {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    /* Pulsing central text */
    h1 {
      font-size: 4rem;
      text-align: center;
      animation: pulse 3s ease-in-out infinite alternate;
      z-index: 1;
      pointer-events: none;
    }

    @keyframes pulse {
      0% { transform: scale(0.9); opacity: 0.7; }
      100% { transform: scale(1.1); opacity: 1; }
    }

    /* Floating circles */
    .circle {
      position: absolute;
      border-radius: 50%;
      opacity: 0.6;
      pointer-events: none;
      animation: floatUp linear infinite;
    }

    @keyframes floatUp {
      0% { transform: translateY(100vh) scale(0.5); opacity: 0; }
      50% { opacity: 0.6; }
      100% { transform: translateY(-10vh) scale(1); opacity: 0; }
    }
  </style>
</head>
<body>

  <h1>Chel duh tuk bik pg muh</h1>

  <!-- Background music -->
  <audio id="bgMusic" loop>
    <source src="your-music-file.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
  </audio>

  <script>
    const colors = ['#ffffff', '#ffccff', '#ccf5ff', '#ffff99'];
    const audio = document.getElementById('bgMusic');
    let audioContext, analyser, source, dataArray;

    // Create floating circle
    function createCircle(intensity = 1) {
      const circle = document.createElement('div');
      const size = Math.random() * 50 + 20 * intensity; // Bigger on beat
      circle.classList.add('circle');
      circle.style.width = `${size}px`;
      circle.style.height = `${size}px`;
      circle.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
      circle.style.left = `${Math.random() * 100}vw`;
      circle.style.animationDuration = `${Math.random() * 5 + 5}s`;
      document.body.appendChild(circle);
      setTimeout(() => circle.remove(), 10000);
    }

    // Setup Web Audio API
    function setupAudio() {
      audioContext = new (window.AudioContext || window.webkitAudioContext)();
      analyser = audioContext.createAnalyser();
      source = audioContext.createMediaElementSource(audio);
      source.connect(analyser);
      analyser.connect(audioContext.destination);
      analyser.fftSize = 64;
      dataArray = new Uint8Array(analyser.frequencyBinCount);

      function animate() {
        requestAnimationFrame(animate);
        analyser.getByteFrequencyData(dataArray);
        const avg = dataArray.reduce((a,b) => a+b, 0)/dataArray.length;

        // When beat is strong, create a circle
        if (avg > 150) createCircle(avg/255);
      }
      animate();
    }

    // Start music & audio context on user click (required by browser autoplay policy)
    document.body.addEventListener('click', () => {
      audio.play();
      if (!audioContext) setupAudio();
    });
  </script>
</body>
</html>
