<template>
    <el-upload class="upload-demo" 
    :action="uploadAction" 
    :show-file-list="false" 
    :on-success="handleSuccess"
    :before-upload="beforeUpload" 
    name="file" 
    ref="upload" 
    :auto-upload="false">
        <el-button id="sendAudioBut" style="width: 100px;" v-on:click="uploadFile" color="#0A59F7" round>
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
        uploadFile() {
            const filePath = './src/audioFiles/audioVad.m4a';
            fetch(filePath)
                .then(response=>response.blob())
                .then(blob=>{
                    // 创建文件对象
                    const file = new File([blob], 'audioVad.m4a', { type: 'audio/mp4' });

                    // 获取 el-upload 组件实例
                    const uploadComponent = this.$refs.upload;
                    this.$nextTick(()=>{
                        // 创建一个新的 FileList 对象
                        const dataTransfer = new DataTransfer();
                        dataTransfer.items.add(file);

                        // 将 FileList 对象赋值给 el-upload 组件的 input 元素
                        uploadComponent.$el.querySelector('input[type="file"]').files = dataTransfer.files;

                        // 手动触发上传
                        uploadComponent.submit();
                    });
                })
                .catch(error=>{
                    console.error("文件读取失败",error);
                });
        },
        handleSuccess(response, file, fileList) {
            // 文件上传成功的回调
            eventBus.emit('audio-response', response);
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
/* 根据实际情况调整样式 */
/* .upload-demo {
    display: inline-block;
} */

.upload-demo {
    height: 0px;
    width: 0px;
    position: relative;
    z-index: 2;
    grid-area: 5 / 4 / 6 / 5;
    transform: translate(200px, 110px);
}
</style>