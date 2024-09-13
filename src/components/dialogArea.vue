<template>
    <div class="diaArea">
        <ul v-infinite-scroll="load" class="infinite-list" style="overflow: auto">
            <li v-for="(message, index) in messages" class="infinite-list-item">
                {{ message.message }}
                <el-button style="width: 1em; float: right;" id="ttsBut" v-on:click="triggerTTS(message.message)" round>
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
import { VideoPlay } from '@element-plus/icons-vue';
import { ref } from 'vue'
const count = ref(0)
const load = () => {
    count.value += 2
}
export default {

    created() {
        eventBus.on('input-event', this.handleCustomEvent);
    },
    beforeUnmount() {
        eventBus.off('input-event', this.handleCustomEvent);
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
        }
    },
};
</script>

<style>
.infinite-list {
    height: 90%;
    list-style: none;
    padding-left: 0px;
    grid-area: 2 / 2 / 5 / 5;
}

.infinite-list .infinite-list-item {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50px;
    background: var(--el-color-primary-light-9);
    margin: 10px;
    color: var(--el-color-primary);
    border-radius: 50px;
}

.infinite-list .infinite-list-item+.list-item {
    margin-top: 10px;
}

#ttsBut {
    position: relative;
    transform: translateX(440px);
}
</style>