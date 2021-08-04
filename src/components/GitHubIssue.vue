<template>
  <div class="container">
    <div v-if="loader.getIssue">
      <img src="/static/loading.svg" alt="loading" />
    </div>
    <div v-if="!loader.getIssue && issue.number">
      <h1>Issue #{{ issue.number }}</h1>
      <hr />
      <h2>{{ issue.title }}</h2>
      <br />
      <div>{{ issue.body }}</div>
      <a
        class="btn btn-primary mt-2 text-center"
        href="javascript:history.go(-1)"
        >Voltar</a
      >
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'GitHubIssue',

  created() {
    this.getIssue();
  },

  data() {
    return {
      issue: {},
      loader: {
        getIssue: false,
      },
    };
  },

  methods: {
    getIssue() {
      this.loader.getIssue = true;
      const url = `https://api.github.com/repos/${this.$route.params.name}/${this.$route.params.repo}/issues/${this.$route.params.issue}`;

      axios
        .get(url)
        .then((response) => {
          this.issue = response.data;
        })
        .finally(() => {
          this.loader.getIssue = false;
        });
    },
  },
};
</script>
