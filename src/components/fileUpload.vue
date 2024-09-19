<template>
    <el-upload class="upload-demo" 
    :action="uploadAction" 
    :show-file-list="false" 
    :on-success="handleSuccess"
    :before-upload="beforeUpload" 
    name="file" 
    :auto-upload="true">
        <el-button id="sendAudioBut" style="width: 100px;" color="#0A59F7" v-on:click="handleButClick" round>
            <el-icon>
                <Microphone />
            </el-icon>
            <span>上传</span>
        </el-button>
    </el-upload>
</template>

<script>
import { Microphone } from '@element-plus/icons-vue'
import eventBus from '@/eventBus'
export default {
    data() {
        return {
            uploadAction: "",
        };
    },
    methods: {
        handleButClick(){
            eventBus.emit('sendAudioBut-clicked',{message:"已上传音频文件",type:'audioUpload'});
        },
        handleSuccess(response, file, fileList) {
            // 文件上传成功的回调
            eventBus.emit('audio-response', {message:response.text,type:'response'});
            console.log(response);
        },
        beforeUpload(file) {
            // 在上传之前的钩子，返回 false 可以取消上传
            if (file.name.includes('Vad')) {
                this.uploadAction = "http://127.0.0.1:5000/input_audio";
            } else {
                this.uploadAction = "http://127.0.0.1:5000/register_speaker";
            }
            console.log(file);
            console.log(this.uploadAction);
            return true;
        },
    },
};
</script>

<style scoped>

.upload-demo {
    height: 0px;
    width: 0px;
    position: relative;
    z-index: 2;
    grid-area: 5 / 4 / 6 / 5;
    transform: translate(200px, 110px);
}
</style>