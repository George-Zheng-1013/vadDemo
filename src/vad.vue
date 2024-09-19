<template>
    <div class="div2">
        <p id="realtimeVad">实时说话检测</p>
        <button @click="startVAD" class="vadButton" id="startVadButton" ref="startVadButton">开始说话检测</button>
        <button @click="pauseVAD" class="vadButton" id="pauseVadButton" ref="pauseVadButton">停止说话检测</button>
    </div>
    <audio-recorder ref="audioRecorderRef"></audio-recorder>
</template>

<script>
import { onMounted, ref } from 'vue'
import audioRecorder from './components/audioRecorderVad.vue'
import eventBus from '@/eventBus'

export default {
    created() {
        eventBus.on('tts-start-event', this.startTts);
        eventBus.on('tts-end-event', this.endTts);
        eventBus.on('response-shown-event',this.startTts);
    },
    beforeUnmount() {
        eventBus.off('tts-start-event', this.startTts);
        eventBus.off('tts-end-event', this.endTts);
    },
    components: {
        audioRecorder,
    },
    setup() {
        const audioRecorderRef = ref(null);
        const isPaused = ref(false);
        function stopRecord() {
            audioRecorderRef.value.stopRecording();
            console.log("录音结束");
        }
        function pauseVAD(){
            console.log("pausedButton has been clicked");
            isPaused.value = true;
        }
        async function startVAD() {
            console.log("已开始说话检测");
            
            const myvad = await vad.MicVAD.new({
                onSpeechStart: () => {
                    console.log("语音开始");
                    if(isPaused.value){
                        myvad.pause();
                        console.log("已暂停");
                        isPaused.value=false;
                    }else{
                        audioRecorderRef.value.startRecording();
                        console.log("开始录音");
                    }
                },
                onSpeechEnd: (audio) => {
                    console.log("语音结束");
                    myvad.pause();
                    console.log("已暂停");
                    isPaused.value=false;
                    stopRecord();
                    const wavBuffer = vad.utils.encodeWAV(audio);
                    const blob = new Blob([wavBuffer], { type: 'audio/mp4' });
                    console.log(blob.size);
                    const formData = new FormData();
                    formData.append('file', blob, 'audio.m4a');
                    fetch('http://127.0.0.1:5000/input_audio', {
                        method: 'POST',
                        body: formData
                    })
                    .then(response => response.json())
                    .then(data => {
                        console.log('上传成功:', data);
                        eventBus.emit('sendAudioBut-clicked',{message:"已上传对话音频",type:'audioUpload'});
                        eventBus.emit('audio-response', {message:data.text,type:'response'});
                    })
                    .catch(error => {
                        console.error('上传失败:', error);
                    });
                },
            });
            myvad.start();
        }

        return {
            startVAD,
            audioRecorderRef,
            pauseVAD,
            isPaused,          
        };
    }, 
    methods:{
        startTts(){
            this.$refs.startVadButton.click();
        },
        endTts(){
            this.$refs.pauseVadButton.click();
        }
    }
}
</script>

<style scoped>
.vadButton {
    padding: 12.5px 15px;
    border: 0;
    border-radius: 100px;
    background-color: #0A59F7;
    color: #ffffff;
    font-weight: Bold;
    transition: all 0.5s;
    -webkit-transition: all 0.5s;
    font-size: 24px;
    width: 100%;
}
#startVadButton {
        margin-bottom: 5px;
    }

.div2 {
    padding: 12.5px 15px;
    border: 0;
    border-radius: 40px;
    background-color: white;
    transition: all 0.5s;
    -webkit-transition: all 0.5s;
    margin-bottom: 5px;
    z-index: 1;
    width: 100%;
}
#realtimeVad {
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
#startVadButton {
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

#startVadButton:hover {
  background-color: #64BB5C;opacity: 0.6;
  box-shadow: 0 0 20px #64BB5C;opacity: 0.6;
  transform: scale(0.9);
}

#startVadButton:active {
  background-color: #64BB5C;opacity: 0.4;
  transition: all 0.25s;
  -webkit-transition: all 0.25s;
  box-shadow: none;
  transform: scale(0.98);
}
#pauseVadButton {
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

#pauseVadButton:hover {
  background-color: #ED6F21;opacity: 0.6;
  box-shadow: 0 0 20px #ED6F21;opacity: 0.6;
  transform: scale(0.9);
}

#pauseVadButton:active {
  background-color: #ED6F21;opacity: 0.4;
  transition: all 0.25s;
  -webkit-transition: all 0.25s;
  box-shadow: none;
  transform: scale(0.98);
}
</style>