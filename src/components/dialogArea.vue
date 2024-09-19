<template>
    <div class="diaArea">
        <ul v-infinite-scroll="load" class="infinite-list" style="overflow: auto">
            <li v-for="(message, index) in messages" 
            v-bind:key="index" 
            v-bind:class="{
                'type-dialog': message.type === 'dialog',
                'type-audioUpload': message.type === 'audioUpload',
                'type-response':message.type==='response'}"
            class="li">
                <template v-if="message.type==='dialog' || message.type==='audioUpload'">
                    <el-button aria-disabled="true" class="userButton" circle>
                        <el-icon>
                            <User/>
                        </el-icon>
                    </el-button>
                </template>
                <template v-if="message.type==='response'">
                    <el-button aria-disabled="true" class="cpuButton" circle>
                        <el-icon>
                            <Cpu/>
                        </el-icon>
                    </el-button>
                </template>
                <div class="divMessage">
                    {{ message.message }}
                </div>
                <template v-if="message.type === 'dialog' || message.type==='response'">
                    <el-button id="ttsBut" v-on:click="triggerTTS(message.message)" circle>
                        <el-icon>
                            <VideoPlay/>
                        </el-icon>
                    </el-button>
                </template>
            </li>
        </ul>
    </div>
</template>

<script>
import eventBus from '@/eventBus'
import { ElMessage } from 'element-plus'
import { VideoPlay,User,Cpu } from '@element-plus/icons-vue'
import { ref } from 'vue'
const count = ref(0)
const load = () => {
    count.value += 2
}
export default {

    created() {
        eventBus.on('input-event', this.handleCustomEvent);
        eventBus.on('sendAudioBut-clicked',this.handleButClick);
        eventBus.on('audio-response', this.response);
    },
    beforeUnmount() {
        eventBus.off('input-event', this.handleCustomEvent);
        eventBus.off('sendAudioBut-clicked',this.handleButClick);
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
                this.messages.push({message:payload.message,type:'dialog'});
            }
        },
        handleButClick(message1){
            this.messages.push(message1);
            console.log(message1);
        },
        response(audioResponse){
            this.messages.push(audioResponse);
        },
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
.infinite-list .type-dialog {
    display: flex;
    align-items: center;
    justify-content: space-between;
    /* height: 50px; */
    margin: 10px;
    color: black;
    font-size: 16px;
    font-weight: normal;
    border-radius:25px;
    margin-top: 10px;

}
.type-dialog,.type-audioUpload {
    padding-left: 10px;
    padding-right: 15px;
    background-color: #E6EEFE;
}
.type-audioUpload {
    display: flex;
    align-items: center;
    height: 50px;
    margin: 10px;
    color: black;
    font-size: 16px;
    font-weight: normal;
    border-radius: 50px;
    margin-top: 10px;
    text-align: center;
    justify-content: center;
}
.type-response {
    display: flex;
    align-items: center;
    justify-content: space-between;
    /* height: 50px; */
    margin: 10px;
    color: black;
    font-size: 16px;
    font-weight: normal;
    border-radius: 25px;
    margin-top: 10px;
    padding-left: 10px;
    padding-right: 15px;
    background-color: #F1F3F5;
}
.cpuButton {
    margin-left: 10px;
}
.divMessage {
    word-wrap: break-word;
    height: 100%;
    width: 90%;
    flex: 1;
}
.type-dialog, .type-response {
    display: flex;
    align-items: stretch;
}
</style>