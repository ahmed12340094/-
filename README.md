<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>التقاط صورة</title>
</head>
<body>
    <h2>اضغط على الزر لالتقاط صورة</h2>
    <video id="video" autoplay></video>
    <button id="capture">التقاط صورة</button>
    <canvas id="canvas" style="display:none;"></canvas>
    <script>
        const video = document.getElementById('video');
        const canvas = document.getElementById('canvas');
        const context = canvas.getContext('2d');
        const captureButton = document.getElementById('capture');

        navigator.mediaDevices.getUserMedia({ video: true })
            .then(stream => video.srcObject = stream)
            .catch(err => console.error("حدث خطأ:", err));

        captureButton.addEventListener('click', () => {
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            context.drawImage(video, 0, 0, canvas.width, canvas.height);
            canvas.toBlob(blob => {
                const formData = new FormData();
                formData.append('image', blob, 'captured.jpg');
                fetch('/upload', { method: 'POST', body: formData });
            }, 'image/jpeg');
        });
    </script>
</body>
</html>
