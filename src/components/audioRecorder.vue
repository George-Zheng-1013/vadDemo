<template>
    <div class="div3">
        <div class="roundedRectangle">
            <p id="newRole">新建说话人</p>
            <button @click="startRecording" class="vadButton" id="startRecordingButton">开始录音</button><br>
            <button @click="stopRecording" class="vadButton" id="stopRecordingButton">停止录音</button>
        </div>
    </div>
    <div class="newRoleAudioPlayer" id="audioPlayerBack">
        <p id="newRoleAudio">新建说话人音频</p>
        <audio ref="audioPlayer" controls></audio>
        <a ref="downloadLink" style="display: none;">下载音频</a>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
    setup() {
        const audioPlayer = ref(null);
        const downloadLink = ref(null);
        let mediaRecorder;
        let audioChunks = [];

        async function startRecording() {
            const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
            mediaRecorder = new MediaRecorder(stream, { mimeType: 'audio/mp4' });

            mediaRecorder.ondataavailable = (event) => {
                audioChunks.push(event.data);
            };

            mediaRecorder.onstop = () => {
                const audioBlob = new Blob(audioChunks, { type: 'audio/mp4' });
                const url = URL.createObjectURL(audioBlob);
                audioPlayer.value.src = url;
                downloadLink.value.href = url;
                downloadLink.value.download = 'audio.m4a';
                downloadLink.value.style.display = 'block';
                downloadLink.value.textContent = '下载音频';
                audioChunks = [];
            };

            mediaRecorder.start();
        }

        function stopRecording() {
            if (mediaRecorder) {
                mediaRecorder.stop();
            }
        }

        return {
            startRecording,
            stopRecording,
            audioPlayer,
            downloadLink
        };
    },
};
</script>

<style>
#startRecordingButton {
  padding: 12.5px 15px;
  border: 0;
  border-radius: 100px;
  background-color: #64BB5C;
  color: #ffffff;
  font-weight: Bold;
  transition: all 0.5s;
  -webkit-transition: all 0.5s;
  font-size: 24px;
  margin-bottom: 5px;
  width: 100%;
}

#startRecordingButton:hover {
  background-color: #64BB5C;opacity: 0.6;
  box-shadow: 0 0 20px #64BB5C;opacity: 0.6;
  transform: scale(0.9);
}

#startRecordingButton:active {
  background-color: #64BB5C;opacity: 0.4;
  transition: all 0.25s;
  -webkit-transition: all 0.25s;
  box-shadow: none;
  transform: scale(0.98);
}

#stopRecordingButton {
  padding: 12.5px 15px;
  border: 0;
  border-radius: 100px;
  background-color: #ED6F21;
  color: #ffffff;
  font-weight: Bold;
  transition: all 0.5s;
  -webkit-transition: all 0.5s;
  font-size: 24px;
  width: 100%;
}

#stopRecordingButton:hover {
  background-color: #ED6F21;opacity: 0.6;
  box-shadow: 0 0 20px #ED6F21;opacity: 0.6;
  transform: scale(0.9);
}

#stopRecordingButton:active {
  background-color: #ED6F21;opacity: 0.4;
  transition: all 0.25s;
  -webkit-transition: all 0.25s;
  box-shadow: none;
  transform: scale(0.98);
}
.roundedRectangle {
    padding: 12.5px 15px;
    border: 0;
    border-radius: 40px;
    background-color: white;
    transition: all 0.5s;
    -webkit-transition: all 0.5s;
    margin-bottom: 5px;
    height: 100%;
    width: 100%;
}
#newRole,#newRoleAudio {
    color: #0A59F7;
    padding: 10.5px 15px;
    border: 0;
    border-radius: 100px;
    background-color: #E5E7E9;
    font-weight: Bold;
    transition: all 0.5s;
    -webkit-transition: all 0.5s;
    font-size: 24px;
    margin-bottom: 5px;
    width: 100%;
    text-align: center;
}
.vadButton {
    width: 200px;
}
</style>