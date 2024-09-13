<template>
    <div class="diaArea">
        <ul v-infinite-scroll="load" class="infinite-list" style="overflow: auto">
            <li v-for="(message, index) in messages" class="infinite-list-item">{{ message.message }}</li>
        </ul>
    </div>
</template>

<script>
import eventBus from '@/eventBus'
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
        };
    },
    methods: {
        handleCustomEvent(payload) {
            this.messages.push(payload);
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
}

.infinite-list .infinite-list-item+.list-item {
    margin-top: 10px;
}
</style>