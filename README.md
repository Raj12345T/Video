# Video

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Advanced Video Player Demo</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/video.js/8.12.0/video-js.css" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .container {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            max-width: 900px;
            width: 100%;
            backdrop-filter: blur(10px);
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .header h1 {
            color: #333;
            font-size: 2.5em;
            margin-bottom: 10px;
            background: linear-gradient(45deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .header p {
            color: #666;
            font-size: 1.1em;
        }

        .video-container {
            position: relative;
            margin-bottom: 30px;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        .video-js .vjs-progress-control {
            display: none !important;
        }
        
        .video-js .vjs-progress-holder {
            display: none !important;
        }

        .video-js .vjs-load-progress,
        .video-js .vjs-play-progress {
            display: none !important;
        }

        .video-js {
            width: 100%;
            height: 500px;
        }

        .stats-panel {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease;
        }

        .stat-card:hover {
            transform: translateY(-5px);
        }

        .stat-card h3 {
            font-size: 0.9em;
            opacity: 0.9;
            margin-bottom: 10px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .stat-card .value {
            font-size: 2em;
            font-weight: bold;
        }

        .attention-warning {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: linear-gradient(135deg, #ff6b6b, #ee5a24);
            color: white;
            padding: 30px;
            border-radius: 20px;
            text-align: center;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
            z-index: 10000;
            display: none;
            backdrop-filter: blur(10px);
        }

        .attention-warning h2 {
            margin-bottom: 15px;
            font-size: 1.8em;
        }

        .attention-warning p {
            margin-bottom: 20px;
            font-size: 1.1em;
        }

        .countdown {
            font-size: 3em;
            font-weight: bold;
            color: #fff;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
        }

        .reward-message {
            text-align: center;
            padding: 30px;
            border-radius: 15px;
            margin-top: 20px;
            font-size: 1.2em;
            font-weight: bold;
            display: none;
            animation: slideIn 0.5s ease-out;
        }

        .reward-success {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
        }

        .reward-failure {
            background: linear-gradient(135deg, #fa709a 0%, #fee140 100%);
            color: #333;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .control-buttons {
            text-align: center;
            margin-top: 20px;
        }

        .btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            font-size: 1em;
            cursor: pointer;
            margin: 0 10px;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
        }

        .status-indicator {
            display: inline-block;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            margin-right: 8px;
        }

        .status-active { background-color: #4CAF50; }
        .status-paused { background-color: #FF9800; }
        .status-warning { background-color: #F44336; }

        @media (max-width: 768px) {
            .container {
                padding: 20px;
                margin: 10px;
            }
            
            .header h1 {
                font-size: 2em;
            }
            
            .video-js {
                height: 300px;
            }
            
            .stats-panel {
                grid-template-columns: 1fr;
            }
        }

        @keyframes slideInRight {
            from {
                transform: translateX(100%);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎬 Advanced Video Player</h1>
            <p>Complete demo with attention tracking, pause logic, and rewards</p>
        </div>

        <div class="video-container">
            <video-js
                id="my-video"
                class="video-js vjs-default-skin"
                controls
                preload="auto"
                data-setup="{}">
                <source src="https://storage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4" type="video/mp4">
                <p class="vjs-no-js">
                    To view this video please enable JavaScript, and consider upgrading to a web browser that
                    <a href="https://videojs.com/html5-video-support/" target="_blank">supports HTML5 video</a>.
                </p>
            </video-js>
        </div>

        <div class="stats-panel">
            <div class="stat-card">
                <h3>Watch Time</h3>
                <div class="value" id="watchTime">0:00</div>
            </div>
            <div class="stat-card">
                <h3>Progress</h3>
                <div class="value" id="progress">0%</div>
            </div>
            <div class="stat-card">
                <h3>Status</h3>
                <div class="value" id="status">
                    <span class="status-indicator status-paused"></span>Ready
                </div>
            </div>
            <div class="stat-card">
                <h3>Attention Score</h3>
                <div class="value" id="attentionScore">100%</div>
            </div>
        </div>

        <div class="control-buttons">
            <button class="btn" onclick="resetVideo()">🔄 Reset</button>
            <button class="btn" onclick="toggleFullscreen()">⛶ Fullscreen</button>
            <button class="btn" onclick="showStats()">📊 Stats</button>
        </div>

        <div id="rewardMessage" class="reward-message"></div>
    </div>

    <div id="attentionWarning" class="attention-warning">
        <h2>⚠️ Please Return to the Video</h2>
        <p>You'll lose your progress if you don't return within:</p>
        <div class="countdown" id="countdown">5</div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/video.js/8.12.0/video.min.js"></script>
    <script>
        class AdvancedVideoPlayer {
            constructor() {
                this.player = null;
                this.isTabActive = true;
                this.watchStartTime = null;
                this.totalWatchTime = 0;
                this.lastUpdateTime = 0;
                this.attentionWarningTimer = null;
                this.attentionCountdown = null;
                this.pauseTimer = null;
                this.hasShownReward = false;
                this.attentionScore = 100;
                this.tabSwitchCount = 0;
                this.maxAllowedTime = 0;
                this.middlePauseTime = null;
                this.middlePauseResumeTimeout = null;
                this.hasFailedMiddlePause = false;
                
                this.init();
            }

            init() {
                this.player = videojs('my-video', {
                    fluid: true,
                    responsive: true,
                    playbackRates: [1],
                    plugins: {}
                });

                this.player.ready(() => {
                    this.disableSeeking();
                });

                this.setupEventListeners();
                this.updateStats();
            }

            disableSeeking() {
                this.maxAllowedTime = 0;
                
                const progressControl = this.player.controlBar.progressControl;
                if (progressControl) {
                    progressControl.el().style.display = 'none';
                }

                this.player.off('keydown');
                this.player.on('keydown', (event) => {
                    const seekingKeys = [37, 38, 39, 40, 33, 34, 35, 36, 74, 75, 76];
                    if (seekingKeys.includes(event.which)) {
                        event.preventDefault();
                        event.stopPropagation();
                        return false;
                    }
                });

                this.player.on('seeking', (e) => {
                    e.preventDefault();
                    const currentTime = this.player.currentTime();
                    if (currentTime > this.maxAllowedTime + 1) {
                        this.player.currentTime(this.maxAllowedTime);
                        this.showMessage('Seeking forward is not allowed! Please watch continuously.', 'warning');
                    }
                });

                this.player.on('seeked', (e) => {
                    const currentTime = this.player.currentTime();
                    if (currentTime > this.maxAllowedTime + 1) {
                        this.player.currentTime(this.maxAllowedTime);
                        this.showMessage('Please watch the video from the beginning without skipping!', 'warning');
                    }
                });

                this.player.on('timeupdate', () => {
                    const currentTime = this.player.currentTime();
                    if (currentTime > this.maxAllowedTime && !this.player.paused()) {
                        this.maxAllowedTime = currentTime;
                    } else if (currentTime > this.maxAllowedTime + 2) {
                        this.player.currentTime(this.maxAllowedTime);
                        this.showMessage('Forward seeking detected! Returning to last valid position.', 'warning');
                    }
                });

                this.player.el().addEventListener('contextmenu', (e) => {
                    e.preventDefault();
                    return false;
                });

                this.lastValidTime = 0;
            }

            setupEventListeners() {
                this.player.on('play', () => {
                    this.onVideoPlay();
                });

                this.player.on('pause', () => {
                    this.onVideoPause();
                });

                this.player.on('timeupdate', () => {
                    this.onTimeUpdate();
                });

                this.player.on('ended', () => {
                    this.onVideoEnd();
                });

                document.addEventListener('visibilitychange', () => {
                    this.handleVisibilityChange();
                });

                window.addEventListener('focus', () => {
                    this.handleWindowFocus();
                });

                window.addEventListener('blur', () => {
                    this.handleWindowBlur();
                });

                document.addEventListener('mousemove', () => {
                    this.handleUserActivity();
                });

                document.addEventListener('keydown', () => {
                    this.handleUserActivity();
                });
            }

            onVideoPlay() {
                this.watchStartTime = Date.now();
                this.lastUpdateTime = this.player.currentTime();
                this.updateStatus('Playing', 'status-active');
                
                if (this.middlePauseTime && !this.hasFailedMiddlePause) {
                    const resumeTime = Date.now();
                    const pauseDuration = (resumeTime - this.middlePauseTime) / 1000;
                    
                    if (pauseDuration > 5) {
                        this.hasFailedMiddlePause = true;
                        this.attentionScore = Math.max(0, this.attentionScore - 30);
                        this.updateAttentionScore();
                        this.showMessage(`Took too long to resume (${Math.round(pauseDuration)}s). Attention score reduced!`, 'warning');
                    } else {
                        this.showMessage(`Good! Resumed within ${Math.round(pauseDuration)} seconds`, 'info');
                    }
                    
                    this.middlePauseTime = null;
                    if (this.middlePauseResumeTimeout) {
                        clearTimeout(this.middlePauseResumeTimeout);
                        this.middlePauseResumeTimeout = null;
                    }
                }
                
                if (this.player.duration() && !this.pauseTimer && !this.middlePauseTime) {
                    const middleTime = this.player.duration() / 2;
                    const currentTime = this.player.currentTime();
                    const timeToMiddle = (middleTime - currentTime) * 1000;
                    
                    if (timeToMiddle > 0) {
                        this.pauseTimer = setTimeout(() => {
                            if (!this.player.paused()) {
                                this.triggerMiddlePause();
                            }
                        }, timeToMiddle);
                    }
                }
            }

            onVideoPause() {
                this.updateWatchTime();
                this.updateStatus('Paused', 'status-paused');
                
                if (this.pauseTimer) {
                    clearTimeout(this.pauseTimer);
                    this.pauseTimer = null;
                }
            }

            onTimeUpdate() {
                this.lastValidTime = this.player.currentTime();
                this.updateWatchTime();
                this.updateProgress();
                this.checkCompletion();
            }

            onVideoEnd() {
                this.updateStatus('Completed', 'status-active');
                this.checkCompletion();
            }

            handleVisibilityChange() {
                if (document.hidden) {
                    this.handleTabSwitch();
                } else {
                    this.handleTabReturn();
                }
            }

            handleWindowFocus() {
                this.isTabActive = true;
                this.hideAttentionWarning();
            }

            triggerMiddlePause() {
                this.player.pause();
                this.middlePauseTime = Date.now();
                this.showMessage('Video paused at middle point for attention check - Resume within 5 seconds!', 'warning');
                this.updateStatus('Middle Pause - Resume Soon!', 'status-warning');
                
                this.middlePauseResumeTimeout = setTimeout(() => {
                    if (this.player.paused() && this.middlePauseTime) {
                        this.hasFailedMiddlePause = true;
                        this.attentionScore = Math.max(0, this.attentionScore - 40);
                        this.updateAttentionScore();
                        this.updateStatus('Failed Middle Pause Test', 'status-warning');
                        this.showMessage('Failed to resume within 15 seconds! Major attention penalty applied.', 'warning');
                        this.middlePauseTime = null;
                    }
                }, 15000);
            }

            handleWindowBlur() {
                this.isTabActive = false;
                if (!this.player.paused()) {
                    this.showAttentionWarning();
                }
            }

            handleTabSwitch() {
                this.isTabActive = false;
                this.tabSwitchCount++;
                this.updateAttentionScore();
                
                if (!this.player.paused()) {
                    this.showAttentionWarning();
                }
            }

            handleTabReturn() {
                this.isTabActive = true;
                this.hideAttentionWarning();
            }

            handleUserActivity() {
                if (!this.isTabActive) {
                    this.handleTabReturn();
                }
            }

            showAttentionWarning() {
                if (this.attentionWarningTimer) return;

                const warning = document.getElementById('attentionWarning');
                const countdown = document.getElementById('countdown');
                warning.style.display = 'block';
                
                let timeLeft = 5;
                countdown.textContent = timeLeft;
                
                this.attentionCountdown = setInterval(() => {
                    timeLeft--;
                    countdown.textContent = timeLeft;
                    
                    if (timeLeft <= 0) {
                        this.handleAttentionTimeout();
                    }
                }, 1000);

                this.attentionWarningTimer = setTimeout(() => {
                    this.handleAttentionTimeout();
                }, 5000);
            }

            hideAttentionWarning() {
                const warning = document.getElementById('attentionWarning');
                warning.style.display = 'none';
                
                if (this.attentionWarningTimer) {
                    clearTimeout(this.attentionWarningTimer);
                    this.attentionWarningTimer = null;
                }
                
                if (this.attentionCountdown) {
                    clearInterval(this.attentionCountdown);
                    this.attentionCountdown = null;
                }
            }

            handleAttentionTimeout() {
                this.hideAttentionWarning();
                this.player.pause();
                this.attentionScore = Math.max(0, this.attentionScore - 20);
                this.updateAttentionScore();
                this.updateStatus('Attention Lost', 'status-warning');
                this.showMessage('Video paused due to inactivity. Please stay focused!', 'warning');
            }

            updateWatchTime() {
                if (this.watchStartTime && !this.player.paused()) {
                    const currentTime = this.player.currentTime();
                    const timeDiff = currentTime - this.lastUpdateTime;
                    
                    if (timeDiff > 0 && timeDiff < 2) {
                        this.totalWatchTime += timeDiff;
                    } else if (timeDiff > 2) {
                        console.log('Potential seeking detected, not counting watch time');
                    }
                    
                    this.lastUpdateTime = currentTime;
                }
                
                const minutes = Math.floor(this.totalWatchTime / 60);
                const seconds = Math.floor(this.totalWatchTime % 60);
                document.getElementById('watchTime').textContent = 
                    `${minutes}:${seconds.toString().padStart(2, '0')}`;
            }

            updateProgress() {
                const progress = (this.player.currentTime() / this.player.duration()) * 100;
                document.getElementById('progress').textContent = `${Math.round(progress)}%`;
            }

            updateStatus(status, className) {
                const statusElement = document.getElementById('status');
                const indicator = statusElement.querySelector('.status-indicator');
                
                indicator.className = `status-indicator ${className}`;
                statusElement.innerHTML = `<span class="status-indicator ${className}"></span>${status}`;
            }

            updateAttentionScore() {
                const penalty = Math.min(this.tabSwitchCount * 10, 50);
                this.attentionScore = Math.max(0, 100 - penalty);
                document.getElementById('attentionScore').textContent = `${this.attentionScore}%`;
            }

            checkCompletion() {
                const videoDuration = this.player.duration();
                const watchPercentage = (this.totalWatchTime / videoDuration) * 100;
                
                if (watchPercentage >= 95 && !this.hasShownReward) {
                    this.hasShownReward = true;
                    this.showRewardMessage();
                }
            }

            showRewardMessage() {
                const rewardElement = document.getElementById('rewardMessage');
                const videoDuration = this.player.duration();
                const watchPercentage = (this.totalWatchTime / videoDuration) * 100;
                
                const hasGoodAttention = this.attentionScore >= 80;
                const hasWatchedEnough = watchPercentage >= 95;
                const passedMiddlePause = !this.hasFailedMiddlePause;
                
                const isEligible = hasGoodAttention && hasWatchedEnough && passedMiddlePause;
                
                if (isEligible) {
                    rewardElement.className = 'reward-message reward-success';
                    rewardElement.innerHTML = `
                        🎉 <strong>Congratulations!</strong> 🎉<br>
                        ✅ Watch completion: ${Math.round(watchPercentage)}%<br>
                        ✅ Attention score: ${this.attentionScore}%<br>
                        ✅ Middle pause test: PASSED<br>
                        <strong>You've earned your reward!</strong>
                    `;
                } else {
                    rewardElement.className = 'reward-message reward-failure';
                    let failureReasons = [];
                    if (!hasWatchedEnough) failureReasons.push(`Watch time: ${Math.round(watchPercentage)}% (need 95%)`);
                    if (!hasGoodAttention) failureReasons.push(`Attention: ${this.attentionScore}% (need 80%)`);
                    if (!passedMiddlePause) failureReasons.push('Middle pause test: FAILED');
                    
                    rewardElement.innerHTML = `
                        😔 <strong>No Reward - Requirements Not Met</strong><br>
                        ${failureReasons.join('<br>')}<br>
                        <strong>Better luck next time!</strong>
                    `;
                }
                
                rewardElement.style.display = 'block';
            }

            showMessage(message, type) {
                const messageEl = document.createElement('div');
                messageEl.style.cssText = `
                    position: fixed;
                    top: 20px;
                    right: 20px;
                    background: ${type === 'warning' ? '#ff6b6b' : '#4CAF50'};
                    color: white;
                    padding: 15px 20px;
                    border-radius: 10px;
                    z-index: 9999;
                    box-shadow: 0 5px 15px rgba(0,0,0,0.3);
                    animation: slideInRight 0.3s ease-out;
                `;
                messageEl.textContent = message;
                document.body.appendChild(messageEl);
                
                setTimeout(() => {
                    messageEl.remove();
                }, 3000);
            }

            updateStats() {
                setInterval(() => {
                    if (!this.player.paused()) {
                        this.updateWatchTime();
                        this.updateProgress();
                    }
                }, 1000);
            }
        }

        function resetVideo() {
            location.reload();
        }

        function toggleFullscreen() {
            if (videoPlayer.player.isFullscreen()) {
                videoPlayer.player.exitFullscreen();
            } else {
                videoPlayer.player.requestFullscreen();
            }
        }

        function showStats() {
            const videoDuration = videoPlayer.player.duration();
            const watchPercentage = videoDuration ? (videoPlayer.totalWatchTime / videoDuration) * 100 : 0;
            
            alert(`Video Statistics:
- Total Watch Time: ${document.getElementById('watchTime').textContent}
- Actual Watch Progress: ${Math.round(watchPercentage)}%
- Video Position: ${document.getElementById('progress').textContent}
- Attention Score: ${document.getElementById('attentionScore').textContent}
- Tab Switches: ${videoPlayer.tabSwitchCount}
- Middle Pause Test: ${videoPlayer.hasFailedMiddlePause ? 'FAILED ❌' : 'PASSED ✅'}
- Status: ${document.getElementById('status').textContent.replace(/.*/, '').trim()}
- Seeking Disabled: ✅
- Middle Pause: ${videoDuration ? Math.round(videoDuration/2) + 's' : 'Pending'}
- Resume Timeout: 5 seconds`);
        }

        let videoPlayer;
        document.addEventListener('DOMContentLoaded', () => {
            videoPlayer = new AdvancedVideoPlayer();
        });
    </script>
</body>
</html>
