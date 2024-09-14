<template>
    <div class="diaArea">
        <ul v-infinite-scroll="load" class="infinite-list" style="overflow: auto">
            <li v-for="(message, index) in messages" class="infinite-list-item">
                {{ message.message }}
                <el-button id="ttsBut" v-on:click="triggerTTS(message.message)" circle>
                    <el-icon>
                        <VideoPlay />
                    </el-icon>
                </el-button>
            </li>
        </ul>
    </div>
</template>

<script>
import eventBus from '@/eventBus'
import { ElMessage } from 'element-plus'
import { VideoPlay } from '@element-plus/icons-vue'
import { ref } from 'vue'
const count = ref(0)
const load = () => {
    count.value += 2
}
export default {

    created() {
        eventBus.on('input-event', this.handleCustomEvent);
        eventBus.on('audio-response', this.response);
    },
    beforeUnmount() {
        eventBus.off('input-event', this.handleCustomEvent);
        eventBus.off('audio-response', this.response);
    },
    components: {

    },

    data() {
        return {
            messages: [],
            load,
        };
    },
    methods: {
        triggerTTS(message) {
            // 触发全局事件总线的事件
            eventBus.emit('tts-event', message);
            console.log(message);
        },
        handleCustomEvent(payload) {
            if (payload.message == '') {
                console.log("空消息");
                ElMessage({
                    message: '缺少输入，请添加输入',
                    type: 'warning',
                });
            } else {
                this.messages.push(payload);
            }
        },
        response(audioResponse){
            this.messages.push(audioResponse.text);
        }
    },
};
</script>

<style>
.infinite-list {
    height: 100%;
    list-style: none;
    padding-left: 0px;
    grid-area: 2 / 2 / 5 / 5;
}
.infinite-list .infinite-list-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 50px;
    background: #F1F3F5;
    margin: 10px;
    color: black;
    font-size: 16px;
    font-weight: normal;
    border-radius: 50px;
    margin-top: 10px;
}
.infinite-list-item {
    padding-left: 50px;
    padding-right: 10px;
}
/* #ttsBut {
    position: relative;
    transform: translateX(440px);
} */
</style>