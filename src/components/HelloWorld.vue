<template>
  <v-container>
    <v-row
      justify="space-between"
    >
      <v-col
        cols="12"
        md="4"
      >
        <v-form ref="form">
          <v-text-field
            v-model="model"
            ref="fieldId"
            :counter="max"
            :rules="rules"
            :label="label"
          ></v-text-field>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    props: {
      label: {
        type: String,
      },
      value: {
        type: String,
      }
    },
    data: () => ({
      allowSpaces: false,
      max: 0,
      model: '',
    }),
    mounted() {
      this.$refs.fieldId.focus();
    },
    computed: {
      rules () {
        const rules = []

        if (this.max) {
          const rule =
            v => (v || '').length <= this.max ||
              `A maximum of ${this.max} characters is allowed`

          rules.push(rule)
        }

        if (!this.allowSpaces) {
          const rule =
            v => (v || '').indexOf(' ') < 0 ||
              'No spaces are allowed'

          rules.push(rule)
        }

        if (this.value) {
          const rule =
            v => (v || '').toLowerCase() === this.value.toLowerCase() ||
              'Oops, 错了哟 :('

          rules.push(rule)
        }

        return rules
      },
    },

    watch: {
      value: 'validateField',
      max: 'validateField',
      model: 'validateField',
    },

    methods: {
      validateField () {
        this.$refs.form.validate()
      },
    },
  }
</script>