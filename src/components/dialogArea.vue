<template>
    <div class="diaArea">
        <p>hello</p>
        <p>{{ receivedMessage }}</p>
    </div>
</template>

<script>
import {
    Delete,
    EditPen,
    Microphone,
} from '@element-plus/icons-vue'
import eventBus from '@/eventBus'

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
            receivedMessage: '',
        };
    },
    methods: {
        handleCustomEvent(payload) {
            this.receivedMessage = payload.message;
        }
    },
};
</script>

<style></style>