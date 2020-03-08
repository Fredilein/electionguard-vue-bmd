<template>
  <div class="home">
    <h1>Ballot</h1>
    <code>
      <p>{{ ballot }}</p>
    </code>

    <div v-if="index < contests.length">
      <div v-if="contests[index].type == 'Candidate'">
        <candidate v-bind:contest="contests[index]" v-on:selection="newSelection($event)"/>
      </div>
      <div v-else-if="contests[index].type == 'YesNo'">
        <yesno v-bind:contest="contests[index]" v-on:selection="newSelection($event)"/>
      </div>
      <div v-else>
        <p>Contest type currently not supported...</p>
      </div>
    </div>
    <div v-else>
      <h1>Done!</h1>
      <button class="btn btn-primary" v-on:click="encryptVote()">Confirm Vote</button>
      <div v-if="apires">
        <div class="container apires">
          <h4>Ballot ID</h4>
          {{ apires.data.id }}
          <h4>Tracker</h4>
          {{ apires.data.tracker }}
        </div>
        <router-link to="/">
          <button class="btn btn-primary">Back to Home</button>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import election from '@/data/election.json';
import candidate from '@/components/Candidate.vue';
import yesno from '@/components/Yesno.vue';
import axios from 'axios';
// import config from '@/config.json';

export default {
  name: 'Home',
  components: {
    candidate,
    yesno,
  },
  data() {
    return {
      election,
      contests: election.contests,
      ballot: {},
      index: 0,
      apires: '',
    };
  },
  methods: {
    generateId() {
      return Math.floor(Math.random() * 1000);
    },
    newSelection(choice) {
      const id = choice[0];
      const selection = choice[1];
      this.ballot.votes[id] = selection;
      this.index += 1;
    },
    encryptVote() {
      const url = 'http://localhost:5000/electionguard/EncryptBallot';
      axios.post(url, { ballot: this.ballot })
        .then((res) => {
          this.apires = res;
        })
        .catch((err) => {
          this.apires = 'ERROR: {}'.format(err);
        });
    },
  },
  mounted() {
    const temp = {
      ballotId: this.generateId(),
      ballotStyle: {
        id: '1',
      },
      votes: {},
    };
    this.ballot = temp;
  },
};
</script>

<style lang="stylus">
.apires
  margin-top 20px
  margin-bottom 20px
</style>
