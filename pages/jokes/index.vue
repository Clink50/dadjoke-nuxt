<template>
  <div>
    <h1 v-if="loading">Loading...</h1>
    <div v-else>
      <SearchJokes v-on:search-text="searchText" />
      <Joke v-for="joke in jokes" :key="joke.id" :joke="joke.joke" :id="joke.id" />
    </div>
  </div>
</template>

<script>
import Joke from '../../components/Joke';
import SearchJokes from '../../components/SearchJokes';
export default {
  components: {
    Joke,
    SearchJokes,
  },
  data() {
    return {
      jokes: [],
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
      const res = await fetch('https://icanhazdadjoke.com/search', config);
      const data = await res.json();
      this.jokes = data.results;
      this.loading = false;
    } catch (error) {
      console.error(error);
    }
  },
  methods: {
    async searchText(text) {
      const config = {
        headers: {
          Accept: 'application/json',
        },
      };

      try {
        const res = await fetch(
          `https://icanhazdadjoke.com/search?term=${text}`,
          config,
        );
        const data = await res.json();
        this.jokes = data.results;
        this.loading = false;
      } catch (error) {
        console.error(error);
      }
    },
  },
  head() {
    return {
      title: 'Dad Jokes',
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
