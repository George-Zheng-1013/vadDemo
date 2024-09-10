<template>
    <div class="newRoleAudioPlayerVad" id="audioPlayerBack">
        <p id="inputAudio">对话音频</p>
        <audio ref="audioPlayerVad" controls></audio>
        <!-- <av-line :line-width="2" line-color="lime" :src="audioUrl"></av-line> -->
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
    setup() {
        const audioPlayerVad = ref(null);
        let mediaRecorder;
        let audioChunks = [];

        async function startRecording() {
            const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
            mediaRecorder = new MediaRecorder(stream);

            mediaRecorder.ondataavailable = (event) => {
                audioChunks.push(event.data);
            };

            mediaRecorder.onstop = () => {
                const audioBlob = new Blob(audioChunks, { type: 'audio/mpeg' });
                const audioUrl = URL.createObjectURL(audioBlob);
                audioPlayerVad.value.src = audioUrl;
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
            audioPlayerVad,
        };
    },
};
</script>

<style scoped>
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
    border-radius: 20%;
    background-color: white;
    transition: all 0.5s;
    -webkit-transition: all 0.5s;
    margin-bottom: 5px;
    z-index: 1;
    width: 230px;
}
#newRole,#inputAudio {
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