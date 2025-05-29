<template>
  <form @submit.prevent="submitQuery">
    <label for="userInput">Enter your emotion or circumstance:</label>
    <textarea id="userInput" v.model="userInput" placeholder="e.g., feeling anxious, seeking guidance"></textarea>
    <button type="submit">Find Verses</button>
  </form>
</template>

<script>
export default {
  name: 'InputComponent',
  data() {
    return {
      userInput: '',
    };
  },
  methods: {
    submitQuery() {
      if (this.userInput.trim() !== '') {
        this.$emit('submit-query', this.userInput.trim());
        // this.userInput = ''; // Optionally clear after submit
      }
    },
  },
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 20px;
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
}
label {
  font-weight: bold;
  text-align: left;
}
textarea {
  min-height: 60px;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
  font-family: inherit;
}
button {
  padding: 10px 15px;
  background-color: #42b983; /* Vue green */
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}
button:hover {
  background-color: #3aa875;
}
</style>
