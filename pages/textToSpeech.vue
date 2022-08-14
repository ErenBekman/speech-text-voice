<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div class="d-flex justify-center">
        <select v-model="selectedVoice" class="select-box">
          <option v-for="voice in voiceList" :key="voice.voiceURI">
            {{ voice.name }}
          </option>
        </select>
      </div>
      <div>
        {{ speed[1] }}
        <v-range-slider
          hint="Im a hint"
          v-model="speed"
          ref="speed"
        ></v-range-slider>
      </div>
      <div>
        <v-textarea
          autocomplete="text"
          label="Text"
          clearable
          clear-icon="mdi-close-circle"
          v-model="textToSpeech"
        ></v-textarea>
      </div>
      <div class="span-box mt-5">
        <span
          :ref="`spoken_word_${i}`"
          v-for="(word, i) in textToSpeech.split(' ')"
          :key="i"
        >
          {{ word }}</span
        >
      </div>
      <div class="d-flex justify-center">
        <v-btn
          depressed
          color="blue-grey"
          class="ma-2 white--text"
          @click="speak"
        >
          Speak!
          <v-icon right dark> mdi-microphone </v-icon>
        </v-btn>
      </div>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "textToSpeechPage",
  mounted() {
    this.getVoices().then((voices) => {
      this.voiceList = voices;
      this.selectedVoice = "Yelda";
      this.$refs.speed.step = 10;
      console.log("this.speed :>> ", this.speed);
    });
  },
  data() {
    return {
      tts: window.speechSynthesis,
      voiceList: null,
      selectedVoice: null,
      textToSpeech: "eren",
      speed: [0, 10],
    };
  },

  methods: {
    getVoices() {
      let intervalID;
      return new Promise((resolve, reject) => {
        intervalID = setInterval(() => {
          if (this.tts.getVoices().length > 0) {
            resolve(this.tts.getVoices());
            clearInterval(intervalID);
          }
        }, 10);
      });
    },
    speak() {
      let toSpeak = new SpeechSynthesisUtterance(this.textToSpeech);
      toSpeak.voice =
        this.voiceList.find((v) => v.name == this.selectedVoice) || null;
      toSpeak.rate = this.speed[1];
      toSpeak.onboundary = this.onBoundary;
      this.tts.speak(toSpeak);
    },
    onBoundary(event) {
      let spokenWord = event.utterance.text.substr(
        event.charIndex,
        event.charLength
      );
      const wordIndex = this.textToSpeech
        .split(" ")
        .findIndex((w) => w === spokenWord);
      this.$refs[`spoken_word_${wordIndex}`][0].classList.add("spoken_word");
      console.log("spokenWord :>> ", spokenWord);
    },
  },
};
</script>

<style>
.select-box {
  border: 1px solid black !important;
  text-align: center;
}
.spoken_word {
  color: #c00201;
  font-weight: 600;
  font-size: 20px;
  transition: all 0.1s;
}
.span-box {
  border: 1px solid black !important;
  text-align: center;
  padding: 10px;
}
</style>
