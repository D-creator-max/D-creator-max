<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Continuous Audio Playlist Player</title>
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
        #drop-area.hover {
            border-color: #00f;
        }
    </style>
</head>
<body>
    <div id="drop-area">Drop audio files here</div>
    <audio id="audio-player" controls></audio>

    <script>
        const dropArea = document.getElementById('drop-area');
        const audioPlayer = document.getElementById('audio-player');
        let audioFiles = [];
        let currentAudioIndex = 0;

        dropArea.addEventListener('dragover', (event) => {
            event.preventDefault();
            dropArea.classList.add('hover');
        });

        dropArea.addEventListener('dragleave', () => {
            dropArea.classList.remove('hover');
        });

        dropArea.addEventListener('drop', (event) => {
            event.preventDefault();
            dropArea.classList.remove('hover');
            const files = Array.from(event.dataTransfer.files);
            files.forEach(file => {
                if (file.type.startsWith('audio/')) {
                    audioFiles.push(URL.createObjectURL(file));
                }
            });
            if (audioFiles.length > 0) {
                playAudio(currentAudioIndex);
            }
        });

        audioPlayer.addEventListener('ended', () => {
            currentAudioIndex++;
            if (currentAudioIndex < audioFiles.length) {
                playAudio(currentAudioIndex);
            } else {
                currentAudioIndex = 0; // Reset to first audio if at the end
                playAudio(currentAudioIndex);
            }
        });

        function playAudio(index) {
            audioPlayer.src = audioFiles[index];
            audioPlayer.play();
        }
    </script>
</body>
</html>
