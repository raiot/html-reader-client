<template>
  <div class="container">
    <div>
      <h1>HTML Converter from Markup</h1>
      <label for="markup">Markup</label>
      <textarea v-model="original">
      </textarea>
      <button v-on:click="handleClick()">Send</button>
    </div>
    <div>
      <h1>Result</h1>
      <pre>{{result}}</pre>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Converter',
  data () {
    return {
      loading: false,
      original: '',
      result: ''
    }
  },
  methods: {
    handleClick() {
      this.loading = true;
      const clean = this.original.replace(/(?:\r\n|\r|\n)/g, '|');
      axios.post('http://localhost:5000/api/parser', `"${clean}"`, {
        headers: {
          'Content-Type': 'application/json'
        }
      })
      .then(response => {
        this.result = response.data;
      })
      .catch(() => {
        this.result = 'There was an error, try again later.'
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: flex;
  margin: 0 auto;
  width: 100%;
  justify-content: space-evenly;
}

.container > div {
  width: 30%;
  height: auto;
}

label {
  display: block;
}

textarea, pre {
  width: 100%;
  min-height: 300px;
}

div > button {
  width: 20%;
  display: block;
  margin: 50px auto;
}
</style>
