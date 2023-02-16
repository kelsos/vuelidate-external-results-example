<script setup lang='ts'>
import { ref } from 'vue'
import useVuelidate from '@vuelidate/core'
import { minLength } from '@vuelidate/validators'

const inp = ref<string | null>(null)
const externalResults = ref({})

const rules = {
  inp: {
    min: minLength(2)
  }
}

const v = useVuelidate(rules, { inp }, { $externalResults: externalResults, $autoDirty: true })


const clearExternal = () => {
  externalResults.value = {}
}

const addExternal = async () => {
  if (!(await v.value.$validate())) {
    return
  }

  externalResults.value = { inp: ['external', 'external 2'] }
}
</script>

<template>
  <div>
    <p> {{ v.inp.$errors.map(e => e.$message) }} </p>

    <input v-model='inp'>

    <div class='buttons'>
      <button type='button' @click='clearExternal()'>Clear External</button>
      <button type='button' @click='addExternal()'>Add External</button>
    </div>


  </div>
</template>


<style scoped>
a {
    color: #42b983;
}

label {
    margin: 0 0.5em;
    font-weight: bold;
}

code {
    background-color: #eee;
    padding: 2px 4px;
    border-radius: 4px;
    color: #304455;
}

.buttons {
    margin-top: 10px;
}
</style>
