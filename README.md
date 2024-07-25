# fascia_

To host your HTML file and the cricket.wav file on GitHub Pages, you need to create a GitHub repository, upload your files, and then enable GitHub Pages for your repository. Here’s a step-by-step guide to do this:

## Step 1: Create a GitHub Repository
Log in to GitHub:

Go to GitHub and log in with your account.
Create a New Repository:

Click on the + icon in the upper-right corner and select "New repository".
Name your repository (e.g., my-sound-project).
Choose "Public" for the repository visibility.
Click the "Create repository" button.

## Step 2: Upload Your Files
Upload Files:
Go to your new repository on GitHub.
Click on the "Add file" button and select "Upload files".
Drag and drop your index.html and cricket.wav files into the upload area.
Commit the changes by clicking the "Commit changes" button at the bottom of the page.

## Step 3: Enable GitHub Pages
Go to Repository Settings:

Click on the "Settings" tab in your repository.
Enable GitHub Pages:

Scroll down to the "Pages" section in the left sidebar.
Under "Source", select the branch you want to use (usually main) and the root directory (/).
Click "Save".
Wait for the Site to Build:

GitHub will take a few minutes to build your site. Once it's done, you’ll see a URL where your site is hosted, usually something like https://<username>.github.io/<repository-name>/.
Example URL
If your username is exampleuser and your repository is named my-sound-project, your GitHub Pages URL will likely be: https://exampleuser.github.io/my-sound-project/.
<br>
## Step 4: Access Your Site
Visit the URL:
Open the URL provided by GitHub Pages in your web browser.
Your index.html file should load, and it should have access to the cricket.wav file.

<br>

## Example HTML File
Here is the content of the index.html file for reference:

html
Copy code
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Self Myofascial Release Program</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            align-items: center;
            height: 100vh;
            margin: 0;
            padding-top: 20px; /* Adjust this value to move the content further up or down */
            font-family: 'Consolas', monospace;
        }
        #startButton {
            padding: 20px 40px;
            font-size: 24px;
            cursor: pointer;
            margin-top: 20px;
        }
        h1 {
            margin-bottom: 20px;
        }
        .status-line {
            display: flex;
            width: 100%;
            justify-content: flex-start;
            margin-bottom: 5px;
        }
        .status-label {
            min-width: 120px;
            display: inline-block;
        }
        #waitSecValue {
            font-size: 200px;
            color: red;
            position: fixed;
            bottom: 20px;
            text-align: center;
            width: 100%;
        }
    </style>
</head>
<body>
    <h1>FASCIA V05</h1>
    <div id="status">Self Myofascial Release since 2024.07.16</div>
    <div id="waitSecValue"></div>
    <button id="startButton">CLICK STARTING</button>
    <audio id="cricketSound" src="cricket.wav"></audio>
    <script>
        let loopCount = 0;
        const cricketSound = document.getElementById('cricketSound');
        cricketSound.volume = 0.25;  // Set volume to 25%
        let endTime;
        let countdownInterval;
        let timerStartTime;
        let pausedTime = 0;

        function playCricketSound(times = 1, interval = 500) {
            let count = 0;
            const playSound = () => {
                if (count < times) {
                    cricketSound.currentTime = 0;
                    cricketSound.play().then(() => {
                        console.log('Cricket sound played');
                    }).catch(error => {
                        console.error('Error playing sound:', error);
                    });
                    count++;
                    setTimeout(playSound, interval);
                }
            };
            playSound();
        }

        function startCountdown(duration, loopCount, display) {
            let timer = duration;
            timerStartTime = Date.now();
            countdownInterval = setInterval(() => {
                if (document.hidden) {
                    pausedTime = Date.now() - timerStartTime;
                    clearInterval(countdownInterval);
                    return;
                }

                if (pausedTime > 0) {
                    timer -= Math.floor(pausedTime / 1000);
                    pausedTime = 0;
                    timerStartTime = Date.now();
                }

                const seconds = parseInt(timer % 60, 10);
                const currentTime = new Date().toTimeString().split(' ')[0];
                document.getElementById('waitSecValue').textContent = `${seconds}`;

                display.innerHTML = `
                    <div class="status-line"><span class="status-label">Step:</span> <span>${loopCount}/60</span></div>
                    <div class="status-line"><span class="status-label">Current_time:</span> <span>${currentTime}</span></div>
                    <div class="status-line"><span class="status-label">End_time:</span> <span>${endTime}</span></div>
                `;

                if (--timer < 0) {
                    clearInterval(countdownInterval);
                    if (loopCount < 59) {
                        showAlert();
                    } else {
                        playCricketSound(5, 1000); // Play cricket sound 5 times, 1 second apart
                        document.getElementById('status').innerHTML = '<div class="status-line"><span class="status-label">Total time out:</span> <span>Alerting Finished</span></div>';
                    }
                }
            }, 1000);
        }

        function showAlert() {
            playCricketSound();
            loopCount++;
            startCountdown(59, loopCount, document.getElementById('status'));
        }

        function calculateEndTime() {
            const now = new Date();
            now.setMinutes(now.getMinutes() + 60);
            endTime = now.toTimeString().split(' ')[0]; // Get HH:MM:SS format
        }

        document.getElementById('startButton').addEventListener('click', () => {
            alert('Please set your device volume to 25% for optimal experience.');
            calculateEndTime();
            playCricketSound(1, 1000); // Play cricket sound 1 time, 1 second apart
            setTimeout(() => {
                showAlert();
                document.getElementById('startButton').style.display = 'none';
            }, 1000); // Delay to ensure the first sound is heard
        });

        document.addEventListener('visibilitychange', () => {
            if (!document.hidden && countdownInterval) {
                const remainingTime = parseInt(document.getElementById('waitSecValue').textContent, 10);
                startCountdown(remainingTime, loopCount, document.getElementById('status'));
            }
        });
    </script>
</body>
</html>
Notes:
The above code assumes that your HTML and cricket.wav files are in the same directory.
Make sure the filenames match exactly, including the extension.
After following these steps, your site should be live on GitHub Pages, and you can access it via the provided URL. This will allow you to test your webpage on any device with an internet connection.
