<template>
  <div>
    <h1>{{ contest.title }}</h1>
    <div v-for="(c, i) in contest.candidates" v-bind:key="i" >
      <div class="form-check">
        <input type="radio" class="form-check-input" :id="c.id" :value="c.id" v-model="choice">
        <label class="form-check-label" for="c.id">{{ c.name }}</label>
      </div>
    </div>

    <button class="btn btn-primary" v-on:click="next()">Next</button>
  </div>
</template>

<script>
export default {
  name: 'candidate',
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
      const answer = [{ id: this.choice }];
      this.$emit('selection', [this.contest.id, answer]);
    },
  },
};
</script>

<style lang="stylus">
</style>
