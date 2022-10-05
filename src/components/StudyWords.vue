<template>
  <v-container>
    <HelloWorld v-for="word in words" :key="word.key" :label="word.chinese" :value="word.english" />
  </v-container>
</template>

<script>
import HelloWorld from './HelloWorld.vue';
import WORDS from '../words.json';

export default {
  components: { HelloWorld },
  data() {
    return {
      words: this.getIndexes(10, WORDS.length).map((i, key) => {
        return {
          key,
          input: "",
          message: "",
          ...WORDS[i]
        }
      }),
    } 
  },
  methods: {
    getIndexes(size, max) {
      const indexes = [];
      if (size >= max) {
        for (let i = 0; i < size; i++) {
            indexes.push(i);
        }
        return indexes;
      }
      while (indexes.length < size) {
        const i = this.getRandomInt(max);
        if (!indexes.includes(i)) {
            indexes.push(i);
        }
      }
      return indexes;
    },
    getRandomInt(max) {
      return Math.floor(Math.random() * max);
    }
  },
}
</script>