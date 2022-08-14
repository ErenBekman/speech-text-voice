<template>
  <v-card class="mx-auto" width="344">
    <v-card-text class="d-flex justify-center">
      <v-icon x-large :class="{ isListening }" @click="listen"
        >mdi-microphone</v-icon
      >
      <p v-if="speechToText !== null">{{ speechToText }}</p>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    addNote: {
      type: Function,
      required: true,
    },
    deleteNote: {
      type: Function,
      required: true,
    },
    removeAllNote: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      speechToText: null,
      isListening: false,
      recognition: null,
    };
  },
  mounted() {
    this.recognition = new webkitSpeechRecognition();
    this.recognition.lang = "tr";
    this.recognition.onresult = this.record;
  },
  methods: {
    listen() {
      this.isListening = true;
      this.recognition.start();
    },
    record(event) {
      console.log("event :>> ", event);
      this.isListening = false;
      this.speechToText = event.results[0][0].transcript;
      const parseRegex = /(?<id>(\d*))\s(?=nolu).*(?<command>(sil))$/giu;
      const voiceMatch = parseRegex.exec(this.speechToText);
      const allNoteRemoveRegex = /tüm notları sil/giu;
      const allNoteRemoveMatch = allNoteRemoveRegex.test(this.speechToText);
      // exec() : calistirir geri bildirim verir / test() icindeki degeri var mi yok mu kontrol eder

      setTimeout(() => {
        if (voiceMatch && voiceMatch.groups.id && voiceMatch.groups.command) {
          this.deleteNote(voiceMatch.groups.id);
        } else if (allNoteRemoveMatch) {
          this.removeAllNote();
        } else {
          this.addNote(event.results[0][0].transcript);
        }
        this.speechToText = null;
      }, 1000);
    },
  },
};
</script>

<style>
.isListening {
  color: red !important;
}
</style>
