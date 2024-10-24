<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Audio Drop Player</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
            font-family: Arial, sans-serif;
        }
        #drop-area {
            border: 2px dashed #ccc;
            border-radius: 20px;
            width: 300px;
            height: 200px;
            display: flex;
            justify-content: center;
            align-items: center;
            text-align: center;
            background-color: white;
            margin-bottom: 20px;
        }
        #audio-player {
            display: none;
        }
        #play-button, #shuffle-button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            margin: 5px;
        }
    </style>
</head>
<body>

<div id="drop-area">Drop audio files here</div>
<audio id="audio-player" controls></audio>
<button id="play-button" style="display: none;">Play</button>
<button id="shuffle-button" style="display: none;">Shuffle</button>

<script>
    const dropArea = document.getElementById('drop-area');
    const audioPlayer = document.getElementById('audio-player');
    const playButton = document.getElementById('play-button');
    const shuffleButton = document.getElementById('shuffle-button');
    let audioFiles = [];
    
    dropArea.addEventListener('dragover', (event) => {
        event.preventDefault();
        dropArea.style.borderColor = '#666';
    });

    dropArea.addEventListener('dragleave', () => {
        dropArea.style.borderColor = '#ccc';
    });

    dropArea.addEventListener('drop', (event) => {
        event.preventDefault();
        dropArea.style.borderColor = '#ccc';
        
        const files = event.dataTransfer.files;
        for (const file of files) {
            if (file.type.startsWith('audio/')) {
                audioFiles.push(file);
            }
        }

        if (audioFiles.length > 0) {
            playButton.style.display = 'inline';
            shuffleButton.style.display = 'inline';
            loadAudio(audioFiles[0]);
        }
    });

    playButton.addEventListener('click', () => {
        if (audioPlayer.src) {
            audioPlayer.paused ? audioPlayer.play() : audioPlayer.pause();
        }
    });

    shuffleButton.addEventListener('click', () => {
        if (audioFiles.length > 0) {
            const randomIndex = Math.floor(Math.random() * audioFiles.length);
            loadAudio(audioFiles[randomIndex]);
        }
    });

    function loadAudio(file) {
        const url = URL.createObjectURL(file);
        audioPlayer.src = url;
        audioPlayer.style.display = 'block';
        audioPlayer.play();
    }
</script>

</body>
</html>
