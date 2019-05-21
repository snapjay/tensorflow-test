<template>
    <div>
        <h1>Linear</h1>
        <b-alert show v-if="!tfModel">
            <h4>Please Wait...</h4>
            <h5>I'm still learning!</h5>
        </b-alert>
        <div v-if="tfModel">
            <b-input-group>
                <b-form-input type="number" v-model="value"/>
                <b-input-group-append>
                    <b-button @click="predict(value)">Predict</b-button>
                </b-input-group-append>
            </b-input-group>
            <h4 class="mt-3">Results:</h4>
            <div class="results">
                <div class="key">Value:</div>
                <div>{{ value }}</div>
                <div class="key">Correct Answer:</div>
                <div> {{ correct }}</div>
                <div class="key">Prediction:</div>
                <div>{{ prediction }}</div>
                <div class="key">Error:</div>
                <div> {{ error }}</div>
            </div>
        </div>
    </div>
</template>

<script>
  // import {tfjs } from '@tensorflow/tfwjs'
  //https://www.youtube.com/watch?v=pbCExciEbrc

  export default {
    name: 'home',
    mounted () {
      async function learnLinear () {
        const model = tf.sequential()
        model.add(tf.layers.dense({ units: 1, inputShape: [1] }))

        model.compile({
          loss: 'meanSquaredError',
          optimizer: 'sgd'
        })
        const cx = tf.tensor2d([-1, 0, 1, 2, 3, 4], [6, 1])
        const cy = tf.tensor2d([-3, -1, 1, 3, 5, 7], [6, 1])

        await model.fit(cx, cy, { epochs: 500 })
        return model
      }

      learnLinear().then((model) => {
        this.tfModel = model
      })
    },
    methods: {
      predict (value) {
        this.prediction = this.tfModel.predict(tf.tensor2d([parseFloat(value)], [1, 1])).dataSync()[0]
      }
    },
    watch: {
      value () {
        this.prediction = '-'
      }
    },
    computed: {
      correct () {
        return (this.value * 2) - 1
      },
      error () {
        if (this.prediction === '-') return this.prediction
        return this.correct - this.prediction
      }
    },
    data: () => {
      return {
        tfModel: null,
        value: 20,
        prediction: '-'
      }
    }
  }
</script>
<style scoped>
    .results {
        display: grid;
        grid-template-columns: 120px auto;
        grid-template-rows: auto;
        grid-row-gap: 10px;
        grid-column-gap: 10px;
    }

    .key {
        justify-self: end;
    }
</style>
