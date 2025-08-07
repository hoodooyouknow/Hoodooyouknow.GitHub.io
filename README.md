- `type` must be one of:  
     `feat` (new feature)  
     `fix` (bug fix)  
     `docs` (documentation)  
     `style` (formatting)  
     `refactor` (code restructuring)  
     `test` (testing)  
     `chore` (maintenance)  
   - `scope` is optional (module/file affected)  
   - `description` is imperative tense, under 72 chars  

2. **Body** (optional)  
   - Explains WHAT changed and WHY  
   - Wrapped at 72 characters  
   - Uses bullet points if complex  

3. **Footer** (optional)  
   - References issues (e.g., `Closes #123`)  
   - Documents breaking changes  

**Example of my perfect form:**
graph TD
    A[Client] --> B[API Gateway]
    B --> C[Auth Service]
    C --> D[Database]
    ALTER SYSTEM SET max_connections = 200;
    node --inspect=9229 app.js
    ALTER SYSTEM SET max_connections = 200;
    # Clone repository  
git clone https://github.com/org/repo.git --branch develop  

# Initialize services  
docker-compose -f docker-compose.dev.yml up --build  

# Verify installation  
curl http://localhost:8080/healthcheck
graph TD
    A[Client] --> B[API Gateway]
    B --> C[Auth Service]
    C --> D[Database]
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Multi-Function Web Demo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #f4f4f4;
        }
        h1 {
            color: #333;
            text-align: center;
        }
        .container {
            width: 90%;
            max-width: 600px;
            margin-top: 20px;
            padding: 20px;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        video, img {
            width: 100%;
            border: 2px solid #ddd;
            border-radius: 8px;
            background-color: #000;
        }
        button, .file-input-wrapper {
            margin: 10px 0;
            padding: 12px 24px;
            font-size: 16px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            color: white;
            background-color: #007bff;
            text-align: center;
        }
        button:hover, .file-input-wrapper:hover {
            background-color: #0056b3;
        }
        #photo-input {
            display: none; /* Hide the default file input button */
        }
        .hidden {
            display: none;
        }
    </style>
</head>
<body>

    <h1>Web Access Demo</h1>
    <p>This page demonstrates how a website can request access to your device's hardware, respecting all privacy and security protocols.</p>

    <div class="container">
        <h2>Camera and Microphone</h2>
        <video id="videoElement" autoplay playsinline></video>
        <button id="camera-button">Switch Camera</button>
        <button id="mic-button">Start Microphone</button>
    </div>

    <div class="container">
        <h2>Photo Album Access</h2>
        <p>This shows how a website can ask you to select a photo from your album. The website cannot see your other photos.</p>
        <div class="file-input-wrapper">
            <label for="photo-input">Select Photo from Album</label>
            <input type="file" id="photo-input" accept="image/*">
        </div>
        <img id="selected-photo" class="hidden">
    </div>

    <script>
        const videoElement = document.getElementById('videoElement');
        const cameraButton = document.getElementById('camera-button');
        const micButton = document.getElementById('mic-button');
        const photoInput = document.getElementById('photo-input');
        const selectedPhoto = document.getElementById('selected-photo');
        let currentStream;
        let cameraDeviceId;
        let frontCamera = false;
        let micActive = false;

        // Function to get the camera devices
        async function getCameras() {
            try {
                const devices = await navigator.mediaDevices.enumerateDevices();
                const videoDevices = devices.filter(device => device.kind === 'videoinput');
                return videoDevices;
            } catch (err) {
                console.error("Error enumerating devices:", err);
                return [];
            }
        }

        // Function to start the camera and optionally the mic
        async function startMedia(deviceId, includeMic = false) {
            if (currentStream) {
                currentStream.getTracks().forEach(track => track.stop());
            }

            const constraints = {
                video: { deviceId: deviceId ? { exact: deviceId } : undefined },
                audio: includeMic
            };

            try {
                currentStream = await navigator.mediaDevices.getUserMedia(constraints);
                videoElement.srcObject = currentStream;
                if (includeMic) {
                    micButton.textContent = 'Stop Microphone';
                    micActive = true;
                } else {
                    micButton.textContent = 'Start Microphone';
                    micActive = false;
                }
            } catch (err) {
                console.error("Error accessing media:", err);
                alert("Could not access camera or microphone. Please check permissions.");
            }
        }

        // Initial setup for the camera
        async function initialize() {
            const cameras = await getCameras();
            if (cameras.length > 0) {
                const backCamera = cameras.find(camera => camera.label.toLowerCase().includes('back') || camera.label.toLowerCase().includes('environment'));
                
                if (backCamera) {
                    cameraDeviceId = backCamera.deviceId;
                    await startMedia(cameraDeviceId, false); // Start with back camera, no mic
                } else {
                    cameraDeviceId = cameras[0].deviceId;
                    await startMedia(cameraDeviceId, false);
                }

                if (cameras.length <= 1) {
                    cameraButton.style.display = 'none';
                }
            } else {
                alert("No camera found on this device.");
                cameraButton.style.display = 'none';
            }
        }

        // Handle the button click to switch cameras
        cameraButton.addEventListener('click', async () => {
            const cameras = await getCameras();
            if (cameras.length > 1) {
                frontCamera = !frontCamera;
                const newCamera = frontCamera ? cameras.find(camera => camera.label.toLowerCase().includes('front')) : cameras.find(camera => camera.label.toLowerCase().includes('back') || camera.label.toLowerCase().includes('environment'));

                if (newCamera) {
                    cameraDeviceId = newCamera.deviceId;
                    await startMedia(cameraDeviceId, micActive);
                } else {
                    const otherCamera = cameras.find(camera => camera.deviceId !== cameraDeviceId);
                    if (otherCamera) {
                        cameraDeviceId = otherCamera.deviceId;
                        await startMedia(cameraDeviceId, micActive);
                    }
                }
            }
        });

        // Handle the button click to toggle the microphone
        micButton.addEventListener('click', async () => {
            if (micActive) {
                // Stop the microphone
                const audioTracks = currentStream.getAudioTracks();

                if (audioTracks.length > 0) {
                    audioTracks.forEach(track => track.stop());
                }
                
                // Re-start the camera without audio
                const constraints = {
                    video: { deviceId: cameraDeviceId ? { exact: cameraDeviceId } : undefined },
                    audio: false
                };
                currentStream = await navigator.mediaDevices.getUserMedia(constraints);
                videoElement.srcObject = currentStream;

                micActive = false;
                micButton.textContent = 'Start Microphone';
            } else {
                // Start the microphone with the current camera
                await startMedia(cameraDeviceId, true);
            }
        });

        // Handle the photo selection
        photoInput.addEventListener('change', (event) => {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    selectedPhoto.src = e.target.result;
                    selectedPhoto.classList.remove('hidden');
                };
                reader.readAsDataURL(file);
            }
        });

        // Run the initialization
        initialize();
    </script>

</body>
</html>
