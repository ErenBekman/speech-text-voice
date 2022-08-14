<template>
  <v-card class="mx-auto mt-3 pa-3" max-width="344" outlined>
    <v-card-text class="d-flex">
      <v-avatar color="indigo">
        <span class="white--text text-h5">#{{ noteItem.id }} </span>
      </v-avatar>

      <p class="text-h6 ml-6">
        {{ noteItem.note }}
      </p>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    noteItem: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style></style>
