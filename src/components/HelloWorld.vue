<template>
  <v-container>
    <v-row>
      <v-col
        cols="12"
        md="4"
      >
        <h1>{{chinese}}</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col
        cols="12"
        md="4"
      >
        <v-form ref="form" @submit.prevent="submit">
          <v-text-field
            v-model="model"
            ref="fieldId"
            @keydown.enter="onSubmit"
            :rules="rules"
          ></v-text-field>
        </v-form>
      </v-col>

      <v-col
        cols="4"
        md="4"
      >
      <v-btn tile color="success" @click="submit">
        <v-icon left> mdi-pencil </v-icon>
        下一个单词
        </v-btn>
      </v-col>
    </v-row>
    <v-row
      justify="space-between"
    >
      <v-col
        cols="12"
        md="4"
      >
      <v-alert v-if="done" type="success">正确： {{done.data.length}} 个英文单词</v-alert>
      <v-alert v-if="fail" type="error">错误： {{fail.data.length}} 个英文单词</v-alert>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import axios from 'axios';
  export default {
    data: () => ({
      allowSpaces: false,
      model: '',
      id: null,
      english: '',
      chinese: '',
      done: null,
      fail: null
    }),
    mounted() {
      this.loadData();
      this.$refs.fieldId.focus();
    },
    computed: {
      rules () {
        const rules = []

        if (this.english) {
          const rule = v => {
            if (v && v.toLowerCase() !== this.english.toLowerCase()) {
              return 'Oops, 错了哟 :(';
            }
            return true;
          }

          rules.push(rule)
        }

        return rules
      },
    },
    watch: {
      model: 'validateField',
    },
    methods: {
      validateField () {
        this.$refs.form.validate()
      },
      async loadData() {
        this.model = '';
        const resp = await axios.get('/api/words/todo');
        const words = resp.data;
        const index = this.getRandomInt(words.length);
        const word = words[index];
        this.id = word.id;
        this.english = word.english;
        this.chinese = word.chinese;
        this.fail = await axios.get('/api/words/fail');
        this.done = await axios.get('/api/words/done');
        window.console.log(JSON.stringify(word));
      },
      onSubmit() {
        if (this.model.toLowerCase() === this.english.toLowerCase()) {
          axios.put(`/api/words/${this.id}/right`, {}).then(res => {
            window.console.log(res);
          });
        } else {
          axios.put(`/api/words/${this.id}/wrong`, {}).then(res => {
            window.console.log(res);
          });
        }
      },
      submit() {
        this.loadData();
      },
      getRandomInt(max) {
        return Math.floor(Math.random() * max);
      }
    },
  }
</script>