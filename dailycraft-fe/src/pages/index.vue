import { gql } from '@apollo/client/core';
<template>
  <div class="container">
    <v-row class="row">
      <v-col>
        <v-text-field
          v-model="word1"
          label="1" 
        />
        <v-text-field 
          v-model="word2"
          label="2" 
        />
        <v-btn
          @click="submit"
          color="primary"
        >
          Submit
        </v-btn>
        <p class="mt-4">{{ resultWord }}</p>
      </v-col>
    </v-row>
  </div>

</template>

<script>
import { gql } from '@apollo/client/core';
import apolloClient from '../apolloClient';

  export default {
    data () {
      return {
        word1: '',
        word2: '',
        resultWord: ''
      }
    },
    methods: {
      submit() {
        apolloClient.mutate({
          mutation: gql`
            mutation combineWords($word1: String!, $word2: String!) {
              combineWordsMutation(word1: $word1, word2: $word2) {
                resultWord
              }
            }
          `,
          variables: {
            word1: this.word1,
            word2: this.word2,
          },
        }).then((res) => {
          this.resultWord = res.data.combineWordsMutation.resultWord
          console.log(res)
        }).catch((err) => {
          console.log(err)
        });
      }
    }
  }
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Full viewport height */
  gap: 20px;
}

.row {
  align-items: center;
  justify-content: center;
  gap: 20px;
  max-width: 500px;
}

</style>
