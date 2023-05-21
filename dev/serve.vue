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
      email: '',
      tryRule: (value) => ({
                isValid: value.includes('123'),
                // errorMessage: '',
                errorMessage: 'Username must include 123',
            }),
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
      <div>
        <section>
          <!-- <v-input :idx="0" id="input" v-model="name" required :rules="['emailRule', tryRule, 'lengthRule', tryRule, 'requiredRule']" :maxLength="10"/> -->
          <v-input :idx="0" id="input" v-model="name" required :rules="['email', tryRule, 'length', tryRule, 'required']" :maxLength="10"/>
        </section>
        <v-input :idx="2" id="input2" v-model="email" :minLength="3" required/>
        <v-select :idx="1" id="select" v-model="count" :options="[1,2,3]" required/>
        <button>save</button>
      </div>
    </v-form>
    <!-- <v-select required :options="[1,2,3]" :idx="1" id="select"/> -->
  </div>
</template>
