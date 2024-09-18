<template>
    <div class="newRoleAudioPlayerVad" id="audioPlayerBack">
        <p id="inputAudio">对话音频</p>
        <audio ref="audioPlayerVad" controls></audio>
        <a ref="downloadLink" style="display: none;">下载音频</a>
        <button v-on:click="uploadAudioVad">上传对话音频</button>
    </div>
</template>

<script>
import { ref } from 'vue'
import axios from 'axios'
import eventBus from '@/eventBus'
export default {
    setup() {
        const audioPlayerVad = ref(null);
        const downloadLink=ref(null);
        let mediaRecorder;
        let audioChunks = [];

        async function startRecording() {
            const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
            mediaRecorder = new MediaRecorder(stream, { mimeType: 'audio/mp4' });

            mediaRecorder.ondataavailable = (event) => {
                audioChunks.push(event.data);
            };

            mediaRecorder.onstop = async () => {
                const audioBlob = new Blob(audioChunks, { type: 'audio/mp4' });
                const url = URL.createObjectURL(audioBlob);
                audioPlayerVad.value.src = url;
                downloadLink.value.href = url;
                downloadLink.value.download = 'audioVad.m4a';
                downloadLink.value.style.display = 'block';
                downloadLink.value.textContent = '下载音频';
                
            };

            mediaRecorder.start();
        }

        function stopRecording() {
            if (mediaRecorder) {
                mediaRecorder.stop();
            }
        }    
        
        const uploadAudioVad=async()=> {
            let formData;
            try {
                // 获取blob数据
                let blob = new Blob(audioChunks, { type: 'audio/mp4' });

                // 创建FormData对象
                formData = new FormData();
                formData.append('file', blob, 'audioVad.m4a'); // 第一个参数是后台接收的文件参数名，第二个参数是blob数据，第三个参数是文件名
                // 打印FormData内容
                for (let [key, value] of formData.entries()) {
                    console.log(`${key}: ${value.name}, ${value.type}`);
                }
                // 发送ajax请求
                const response = await axios.post('http://127.0.0.1:5000/input_audio', formData, {
                    headers: {
                        'Content-Type': 'multipart/form-data'
                    }
                });
                audioChunks = [];
                // 处理响应数据
                console.log('上传成功');
                console.log(response.data);
                eventBus.emit('sendAudioBut-clicked',{message:"已上传对话音频",type:'audioUpload'});
                eventBus.emit('audio-response', {message:response.message,type:'response'});
            } catch (error) {
                // 处理错误
                console.error('上传失败:', error);
                console.log(formData);
            }
        };

        return {
            startRecording,
            stopRecording,
            audioPlayerVad,
            uploadAudioVad,
            downloadLink,
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
    background-color: #64BB5C;
    opacity: 0.6;
    box-shadow: 0 0 20px #64BB5C;
    opacity: 0.6;
    transform: scale(0.9);
}

#startRecordingButton:active {
    background-color: #64BB5C;
    opacity: 0.4;
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
    background-color: #ED6F21;
    opacity: 0.6;
    box-shadow: 0 0 20px #ED6F21;
    opacity: 0.6;
    transform: scale(0.9);
}

#stopRecordingButton:active {
    background-color: #ED6F21;
    opacity: 0.4;
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

#newRole,
#inputAudio {
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