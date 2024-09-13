<template>
    <el-upload
        class="upload-demo"
        action="http://127.0.0.1:5000/input_audio"
        :show-file-list="true"
        :on-success="handleSuccess"
        :before-upload="beforeUpload"
        name="file">
        <el-button size="small" type="primary" id="sendAudioBut">点击上传</el-button>
    </el-upload>
</template>

<script>
export default {
    methods: {
        handleSuccess(response, file, fileList) {
            // 文件上传成功的回调
            console.log("Upload successful,");
            console.log(response);
        },
        beforeUpload(file) {
            // 在上传之前的钩子，返回 false 可以取消上传
            console.log(file);
            return true;
        },
    },
};
</script>

<style scoped>
/* 根据实际情况调整样式 */
.upload-demo {
    display: inline-block;
}
</style>