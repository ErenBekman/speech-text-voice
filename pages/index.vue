<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <speech
        :addNote="addNote"
        :deleteNote="deleteNote"
        :removeAllNote="removeAllNote"
      />
      <note-item v-for="note in notes" :key="note.id" :noteItem="note" />
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "IndexPage",
  components: {
    speech: () => import("@/components/speech"),
    noteItem: () => import("@/components/noteItem"),
  },
  data() {
    return {
      notes: [],
    };
  },
  methods: {
    addNote(note) {
      this.notes.push({
        id: this.notes.length + 1,
        note: note,
      });
    },

    deleteNote(id) {
      this.notes.splice(
        this.notes.findIndex((n) => n.id == id),
        1
      );
    },
    removeAllNote() {
      this.notes = [];
    },
  },
};
</script>
