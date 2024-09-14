<template>
  <div class="divTtsBack">
    <textarea type="text" v-model="textInput" id="text-input" placeholder="输入要发送的文本"></textarea>
  </div>
  <div class="divTts">
    <select v-model="selectedVoice">
      <option v-for="(voice, index) in voices" :key="index" :value="index">
        {{ voice.name }} ({{ voice.lang }})
      </option>
    </select>
    <button @click="speak">朗读</button>
  </div>
  <el-button v-on:click="sendMessage" id="sendMessageBut" style="width: 100px;" color="#0A59F7" round>
    <el-icon>
      <Edit />
    </el-icon>
    <span>发送</span>
  </el-button>
</template>

<script>
import eventBus from '@/eventBus';
import { Edit } from '@element-plus/icons-vue'
export default {
  created() {
    eventBus.on('tts-event', this.sendTts);
  },
  beforeUnmount() {
    eventBus.off('tts-event', this.sendTts);
  },
  components: {
    Edit,
  },

  data() {
    return {
      textInput: '',
      voices: [],
      selectedVoice: null,
    };
  },

  mounted() {
    this.populateVoiceList();
    if (window.speechSynthesis.onvoiceschanged !== undefined) {
      window.speechSynthesis.onvoiceschanged = this.populateVoiceList;
    }
    
  },
  methods: {
    sendMessage() {
      eventBus.emit('input-event', { message: this.textInput });
    },
    populateVoiceList() {
      this.voices = window.speechSynthesis.getVoices();
      if (this.voices.length > 0) {
        this.selectedVoice = 0;
      }
    },
    speak() {
      if (this.textInput !== '') {
        const msg = new SpeechSynthesisUtterance(this.textInput);
        msg.voice = this.voices[this.selectedVoice];
        msg.lang = this.voices[this.selectedVoice].lang;
        window.speechSynthesis.speak(msg);
      } else {
        alert('请输入要朗读的文本');
      }
    },
    sendTts(message) {
      if (!message) 
        return;
      console.log(message);
      console.log(typeof message);
      const utterance = new SpeechSynthesisUtterance(message);
      utterance.voice = this.voices[this.selectedVoice];
      window.speechSynthesis.speak(utterance);
      console.log("speak successfully");
    },
  }
};
</script>

<style>
.divTtsBack {
  padding: 12.5px 15px;
  border-radius: 40px;
  background-color: white;
  transition: all 0.5s;
  -webkit-transition: all 0.5s;
  margin-bottom: 5px;
  width: 100%;
}

#text-input {
  position: relative;
  padding: 12.5px 15px;
  border: 0;
  border-radius: 25px;
  background-color: #E5E7E9;
  transition: all 0.5s;
  top: 525px;
  z-index: 1;
  height: 150px;
  width: 100%;
  font-size: 16px;
  font-weight: normal;
  font-family: Arial, Helvetica, sans-serif;
  color: #5C5C5D;
  resize: none;
}

select {
  margin-bottom: 10px;
  padding: 5px;
  width: 320px;
}

button {
  padding: 5px 10px;
}

#sendMessageBut {
  position: relative;
  z-index: 3;
  grid-area: 5 / 4 / 6 / 5;
  transform: translate(200px,70px);
}

.text-input {
  grid-area: 2 / 2 / 6 / 5;
}
</style>