<template lang="pug">
  div
    template(v-for="round in rounds")
      v-icon.primary--text {{winStreak === round ? 'mdi-star' : 'mdi-star-outline'}}
</template>

<script>
export default {
  props: {
    winStreak: {
      type: Number,
      default: 0
    },
    rounds: {
      type: Number,
      default: 3
    }
  }
};
</script>
