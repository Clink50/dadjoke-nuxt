<template>
  <div>
    <nuxt-link to="/jokes">Back to Jokes</nuxt-link>
    <h1 v-if="loading">Loading...</h1>
    <h2 v-else>{{ joke }}</h2>
    <hr />
    <small>Joke ID: {{ $route.params.id }}</small>
  </div>
</template>

<script>
export default {
  data() {
    return {
      joke: {},
      loading: true,
    };
  },
  async created() {
    const config = {
      headers: {
        Accept: 'application/json',
      },
    };

    try {
      //Access parameter
      const res = await fetch(
        `https://icanhazdadjoke.com/j/${this.$route.params.id}`,
        config,
      );
      const data = await res.json();
      this.joke = data.joke;
      this.loading = false;
    } catch (error) {
      console.error(error);
    }
  },
  head() {
    return {
      title: this.joke,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Best place for corny dad jokes',
        },
      ],
    };
  },
};
</script>

<style>
</style>
