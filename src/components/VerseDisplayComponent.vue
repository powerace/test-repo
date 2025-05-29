<template>
  <div class="verse-display">
    <div v-if="loading" class="loading">Loading verses...</div>
    <div v-if="error" class="error-message">{{ error }}</div>
    <div v-if="!loading && !error && verses.length === 0" class="no-verses">
      Enter your emotion or circumstance above to find relevant Bible verses.
    </div>
    <div v-if="!loading && !error && verses.length > 0">
      <div v-for="(verse, index) in verses" :key="index" class="verse-item">
        <h4>{{ verse.reference }}</h4>
        <p class="verse-text" v-html="'&quot;' + verse.text + '&quot;'"></p>
        <p v-if="verse.commentary" class="commentary">{{ verse.commentary }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VerseDisplayComponent',
  props: {
    verses: {
      type: Array,
      default: () => [], // Expect objects like { reference: 'John 3:16', text: '...', commentary: '...' }
    },
    loading: {
      type: Boolean,
      default: false,
    },
    error: {
      type: String,
      default: null,
    },
  },
};
</script>

<style scoped>
.verse-display {
  margin-top: 20px;
}
.loading, .error-message, .no-verses {
  padding: 10px;
  border: 1px solid #eee;
  border-radius: 4px;
  text-align: center;
}
.error-message {
  color: red;
  background-color: #ffe0e0;
  border-color: red;
}
.no-verses {
  color: #777;
}
.verse-item {
  margin-bottom: 20px;
  padding: 15px;
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  background-color: #f9f9f9;
}
.verse-item h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #333;
}
.verse-text {
  font-style: italic;
  margin-bottom: 10px;
}
.commentary {
  font-size: 0.9em;
  color: #555;
  margin-top: 5px;
  padding-left: 15px;
  border-left: 2px solid #ddd;
}
</style>
