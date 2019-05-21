<template>
    <div>
        <h1>Linear</h1>

        <b-input-group>
            <b-form-input type="number" v-model="value"/>
            <b-input-group-append>
                <b-button @click="predict(value)">Predict</b-button>
            </b-input-group-append>
        </b-input-group>
        <h4>Results:</h4>
        <dl>
            <dt>Value</dt>
            <dd>{{ value }}</dd>
            <dt>Prediction</dt>
            <dd><span id="a"></span> | {{ prediction }}</dd>
            <dt>Correct</dt>
            <dd>{{correct}}</dd>
        </dl>
    </div>
</template>

<script>
  // import {tfjs } from '@tensorflow/tfwjs'

  async function learnLinear (value) {
    const model = tf.sequential()
    model.add(tf.layers.dense({ units: 1, inputShape: [1] }))

    model.compile({
      loss: 'meanSquaredError',
      optimizer: 'sgd'
    })
    const cx = tf.tensor2d([-1, 0, 1, 2, 3, 4], [6, 1])
    const cy = tf.tensor2d([-3, -1, 1, 3, 5, 7], [6, 1])

    await model.fit(cx, cy, { epochs: 4 })
    return model.predict(tf.tensor2d([parseFloat(value)], [1, 1]))
  }

  export default {
    name: 'home',
    methods: {
      predict (value) {
       learnLinear(value).then((rsp) => {
         console.log(rsp)
         debugger
         this.prediction = rsp
       })
      }
    },
    computed: {
      correct () {
        return (this.value * 2) - 1
      }
    },
    data: () => {
      return {
        value: 20,
        prediction: '-'
      }
    }
  }
</script>
