<script>
import { defineComponent } from 'vue'
// Uncomment import and local "components" registration if library is not registered globally.
// import { ValidationVueSample } from '@/entry.esm';
import { VSelect, VInput, VForm } from '@/entry.esm'

export default defineComponent({
  name: 'ServeDev',
  data() {
    return {
      name: '',
    }
  },
  components: {
    //  ValidationVueSample,
    VForm,
    VInput,
    VSelect,
  }
})
</script>

<template>
  <div id="app">
    <!-- <validation-vue-sample /> -->
    <v-form>
      <v-input :idx="0" id="input" v-model="name" required/>
      <v-select required :options="[1,2,3]" :idx="1" id="select"/>
    </v-form>
  </div>
</template>
