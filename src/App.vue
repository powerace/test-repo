<template>
  <div id="app">
    <h1>Bible Verse Finder</h1>
    <InputComponent @submit-query="handleQuery" />
    <VerseDisplayComponent :verses="verses" :loading="isLoading" :error="error" />
  </div>
</template>

<script>
import InputComponent from './components/InputComponent.vue';
import VerseDisplayComponent from './components/VerseDisplayComponent.vue';

const API_BIBLE_KEY = 'YOUR_API_KEY_HERE'; // User needs to replace this with their actual API key from docs.api.bible
const DEFAULT_BIBLE_ID = 'de4e12af7f28f599-01'; // King James Version

export default {
  name: 'App',
  components: { InputComponent, VerseDisplayComponent },
  data() {
    return {
      verses: [],
      isLoading: false,
      error: null,
    };
  },
  methods: {
    handleQuery(query) {
      this.isLoading = true;
      this.error = null;
      this.verses = [];
      this.fetchVerses(query);
    },
    async fetchVerses(searchText) {
      if (API_BIBLE_KEY === 'YOUR_API_KEY_HERE') {
        this.error = "Please add your API.bible API key in App.vue";
        this.isLoading = false;
        return;
      }

      const apiUrl = `https://api.scripture.api.bible/v1/bibles/${DEFAULT_BIBLE_ID}/search?query=${encodeURIComponent(searchText)}&sort=relevance`;

      try {
        const response = await fetch(apiUrl, {
          headers: {
            'api-key': API_BIBLE_KEY,
          },
        });

        if (!response.ok) {
          const errorData = await response.json(); // Or response.text() if not always JSON
          throw new Error(errorData.message || `HTTP error! status: ${response.status}`);
        }

        const result = await response.json();

        if (result.data && result.data.verses && result.data.verses.length > 0) {
          this.verses = result.data.verses.map(verse => ({
            reference: verse.reference,
            text: verse.text, // Contains HTML, VerseDisplayComponent should handle with v-html
          }));
        } else if (result.data && result.data.passages && result.data.passages.length > 0) {
          this.verses = result.data.passages.map(passage => ({
            reference: passage.reference,
            text: passage.content, // Contains HTML, VerseDisplayComponent should handle with v-html
          }));
        } else {
          this.verses = [];
          // Optionally set a "no results found" message, or rely on VerseDisplayComponent's default
        }
        this.error = null; // Clear any previous error on success

      } catch (err) {
        console.error("Error fetching verses:", err);
        this.error = err.message || 'Failed to fetch verses. Check console for details.';
        this.verses = []; // Clear verses on error
      } finally {
        this.isLoading = false;
      }
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  color: #42b983;
}
</style>
