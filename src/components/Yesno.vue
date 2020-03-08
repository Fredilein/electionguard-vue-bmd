<template>
  <div>
    <h1>{{ contest.title }}</h1>
    <h3>{{ contest.description }}</h3>
    <div class="form-check">
      <input type="radio" class="form-check-input" id="yes" value="yes" v-model="choice">
      <label class="form-check-label" for="yes">Yes</label>
    </div>
    <div class="form-check">
      <input type="radio" class="form-check-input" id="no" value="no" v-model="choice">
      <label class="form-check-label" for="no">No</label>
    </div>

    <button class="btn btn-primary" v-on:click="next()">Next</button>
  </div>
</template>

<script>
export default {
  name: 'yesno',
  data() {
    return {
      choice: '',
    };
  },
  props: {
    contest: Object,
  },
  methods: {
    next() {
      const answer = this.choice;
      this.$emit('selection', [this.contest.id, answer]);
    },
  },
};
</script>

<style lang="stylus">
.form-check
  font-size 1.2em
</style>
