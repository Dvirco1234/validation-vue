<template>
  <div id="app">
    <!-- <validation-vue-sample /> -->
    <!-- <v-input :idx="0" id="input" v-model="name" required :rules="['emailRule', tryRule, 'lengthRule', tryRule, 'requiredRule']" :maxLength="10"/> -->
    <!-- <v-form>
      <div>
        <section>
          <v-input :idx="0" id="input" v-model="name" required :rules="['email', tryRule, 'length', tryRule, 'required']" :maxLength="10"/>
        </section>
        <v-input :idx="2" id="input2" v-model="email" :minLength="3" required/>
        <v-select :idx="1" id="select" v-model="count" :options="[1,2,3]" required/>
        <button>save</button>
      </div>
    </v-form> -->
    <v-form>
      <div class="wrapper">
        <v-input v-for="(field, idx) in fields" :key="field.id" :idx="idx" :id="field.keyName" v-model="name" required :rules="['email', tryRule, 'length', tryRule, 'required']" :maxLength="10" />
        <v-input :idx="2" id="input2" v-model="email" :minLength="3" required />
        <v-select :idx="1" id="select" v-model="count" :options="[1, 2, 3]" required />
        <button class="submit-btn">Submit</button>
      </div>
    </v-form>
    <!-- <v-select required :options="[1,2,3]" :idx="1" id="select"/> -->
  </div>
</template>
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
      fields: [
        { label: 'First name', keyName: 'firstName', type: 'text', isRequired: true, isFullLine: false, },
        { label: 'Last name', keyName: 'lastName', type: 'text', isRequired: true, isFullLine: false, },
        { label: 'Country', keyName: 'contry', type: 'text', isRequired: true, isFullLine: false, },
        { label: 'Business Email', keyName: 'email', type: 'text', isRequired: true, isFullLine: false, validationOptions: ['emailRule'] },
        { label: 'Password', keyName: 'password', type: 'password', isRequired: true, isFullLine: true, rules: this.passwordRules, isChecklist: true },
        { label: 'Confirm Password', keyName: 'confirmPassword', type: 'password', isRequired: false, isFullLine: true, rules: [this.confirmPasswordRule] },
        { label: 'Phone number', keyName: 'phone', type: 'phone', isRequired: true, isFullLine: true, },
      ],
      passwordRules: [
        (val) => ({
          isValid: /[a-z]/.test(val),
          errorMessage: '1 lowercase character',
        }),
        (val) => ({
          isValid: /\d/.test(val),
          errorMessage: '1 number',
        }),
        (val) => ({
          isValid: /[A-Z]/.test(val),
          errorMessage: '1 uppercase character',
        }),
        (val) => ({
          isValid: val.length >= 8,
          errorMessage: '8 characters minimum',
        }),
      ],
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
<style>
.wrapper {
  position: absolute;
  top: 0;
  left: 50%;
  translate: -50% 100px;
  width: 50%;
}

.wrapper > * {
  margin-bottom: 30px;
}

.submit-btn {
  width: 200px;
  height: 47px;
  color: #002644;
  background-color: #FFCC65;
  border: none;
  margin-inline-start: 400px;
  border-radius: 30px;
  font-size: 20px;
  font-family: almoni-bold, "Times New Roman", Times, serif;
}
</style>
