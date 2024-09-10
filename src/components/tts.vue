<template>
    <div class="divTtsBack">
        <div class="divTts">
            <textarea type="text" v-model="textInput" id="text-input" placeholder="输入要朗读的文本"></textarea>
            <select v-model="selectedVoice">
                <option v-for="(voice, index) in voices" :key="index" :value="index">
                    {{ voice.name }} ({{ voice.lang }})
                </option>
            </select>
            <button @click="speak">朗读</button>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      textInput: '',
      voices: [],
      selectedVoice: null
    };
  },
  mounted() {
    this.populateVoiceList();
    if (window.speechSynthesis.onvoiceschanged !== undefined) {
      window.speechSynthesis.onvoiceschanged = this.populateVoiceList;
    }
  },
  methods: {
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
    }
  }
};
</script>

<style>
.divTtsBack {
    padding: 12.5px 15px;
    border: 0;
    border-radius: 40px;
    background-color: white;
    transition: all 0.5s;
    -webkit-transition: all 0.5s;
    margin-bottom: 5px;
    z-index: 1;
}
#text-input {
    position: absolute;
    padding: 12.5px 15px;
    border: 0;
    border-radius: 25px;
    background-color: #E5E7E9;
    transition: all 0.5s;
    top: 700px;
    z-index: 2;
    height: 190px;
    width: 955px;
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
</style>