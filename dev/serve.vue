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
    <v-form @submitForm="sub" @submitStart="start" @submitFailed="failed">
      <div class="wrapper">
        <v-input v-for="(field, idx) in fields" :key="field.id" :idx="idx" :id="field.keyName" :type="field.type" :label="field.label" v-model="model[field.keyName]" :required="!isFocus"
        :rules="field.rules" :isChecklist="field.isChecklist" showPasswordIcon isChecklistGrid />
        <v-select :idx="3" id="select" v-model="count" isSearch :options="[1, 2, 3,4,5,6,7,8,9,11,222,3,4,5,645,3456,42355,4235,234,3245,2345,2345,3245,2345,345]" required />
        <v-input :idx="1" id="field" label="field.label" v-model="model.password" required type="password"
          :rules="passwordRules"  showPasswordIcon :invalidTerm="invalidTerm"/>
        <v-input id="input4" v-model="model.firstName" required label="123"  :rules="['minLength:6', 'maxLength:10']" :focus="isFocus" autofocus :readonly="!isFocus"/>
        <v-input id="input5" v-model="model.lastName" required label="textarea"  :rules="['minLength:6', 'maxLength:10']" textareaRows="4" :readonly="readonly" />
        <button class="submit-btn" style="font-family: sans-serif; cursor: pointer;" >{{isLoading? 'loading':'Submit'}}</button>
        <button type="button" @click="isFocus = !isFocus">focus</button>
      </div>



      <!-- <article class="cc-details">
        <v-input id="ccNumber" name="ccNumber" label="Credit card number" type="cc" placeholder="XXXX XXXX XXXX XXXX"
          @checkIsVaild="setInputValidations" class="input" v-model="ccInfo.CC_number" :idx="0" required :rules="['cc']" showValidIcon />
        <div class="ex-date flex">
          <label class="ex-date-label">Expiry date<span style="color: #c10015">*</span></label>
          <v-select id="expirationMonth" :idx="3" class="select"
            :options="['01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12',]" v-model="ccInfo.expiration_month"
            :defaultOption="{ label: 'MM', key: '' }" selectLabel="month"
            :class="{ required: hasSubmitted && !ccInfo.expiration_month && payment_type === 'credit_card' }" isInsideLabel />
          <v-select id="expirationYear" :idx="4" class="select" :options="yearOptions" v-model="ccInfo.expiration_year"
            :defaultOption="{ label: 'YY', key: '' }" selectLabel="year"
            :class="{ required: hasSubmitted && !ccInfo.expiration_year && payment_type === 'credit_card' }" isInsideLabel />
          <div class="cvv-wraper position-relative">
            <label>CVV Code<span style="color: #c10015">*</span></label>
            <v-input id="cvv" name="cvv" type="text" placeholder="CVV" class="input cvv" v-model="ccInfo.CVV" :idx="1" required isDigitsOnly
              :maxLength="3" :rules="['length']" />
            <img src="./cc.png" class="ccimg">
          </div>
        </div>
        <v-input id="username" name="username" label="Name on Card" type="text" placeholder="Enter your username" class="input"
          v-model="ccInfo.name_on_card" :idx="2" required />
      </article> -->
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
      isFocus: false,
      readonly: true,
      invalidTerm: '',
      isLoading: false,
      ccInfo: {
        CC_number: '',
        expiration_year: '',
        expiration_month: '',
        CVV: '',
        name_on_card: '',
      },
      name: 'kuyg',
      email: '',
      tryRule: (value) => ({
        isValid: value.includes('123'),
        // errorMessage: '',
        errorMessage: 'Username must include 123',
      }),
      model: {
        firstName: '',
        lastName: '',
        contry: '',
        email: '',
        password: '',
        confirmPassword: '',
        phone: '',
      },
      fields: [
        // { label: 'First name', keyName: 'firstName', type: 'text', isRequired: true, isFullLine: false, },
        // { label: 'Last name', keyName: 'lastName', type: 'text', isRequired: true, isFullLine: false, },
        // { label: 'Country', keyName: 'contry', type: 'text', isRequired: true, isFullLine: false, },
        // { label: 'Email', keyName: 'email', type: 'text', isRequired: true, isFullLine: false, rules: ['email'] },
        {
          label: 'Password', keyName: 'password', type: 'password', isRequired: true, isFullLine: true, rules: [
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
          ], isChecklist: true
        },
        // {
        //   label: 'Confirm Password', keyName: 'confirmPassword', type: 'password', isRequired: false, isFullLine: true, rules: [(value) => ({
        //     isValid: value.length >= 8 && value === this.model.password,
        //     errorMessage: 'Password confirmation does not match',
        //   })]
        // },
        { label: 'Phone number', keyName: 'phone', type: 'phone', isRequired: true, isFullLine: true, },
      ],
      // passwordRules: [
      //   (val) => ({
      //     isValid: /[a-z]/.test(val),
      //     errorMessage: '1 lowercase character',
      //   }),
      //   (val) => ({
      //     isValid: /\d/.test(val),
      //     errorMessage: '1 number',
      //   }),
      //   (val) => ({
      //     isValid: /[A-Z]/.test(val),
      //     errorMessage: '1 uppercase character',
      //   }),
      //   (val) => ({
      //     isValid: val.length >= 8,
      //     errorMessage: '8 characters minimum',
      //   }),
      // ],
    }
  },
  methods: {
    sub({isValid}) {
      console.log('isValid: ', isValid);
      this.isLoading = false
    },
    start(val) {
      // console.log('val: ', val);
      // console.log('startttttttttttt');
      // this.isLoading = true
    },
    failed() {
      // console.log('failed');
    },
   async sr(val) {
      console.log('heree');
      console.log('val: ', val);
      const res = await new Promise(resolve => {
      setTimeout(() => {
        resolve({
            isValid: val.includes('123'),
            errorMessage: 'Username must include 123',
          })
      }, 2000)
    })
      // await this.sr2(val)
      console.log('res: ', res);
      // return res
    return {
                isValid: true,
                errorMessage: 'That email address is already in use',
            }
    },
  //  async sr2(val) {
  //           return new Promise(resolve => {
  //     setTimeout(() => {
  //       resolve({
  //           isValid: val.includes('123'),
  //           errorMessage: 'Username must include 123',
  //         })
  //     }, 2000)
  //   })
  //   }
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

 .top .cc-img {
  height: 30px;
  justify-self: flex-end;
}

 p {
  margin: 0 0 5px 43px;
}

 .cc-details {
  background-color: #F1F5F9;
  padding: 40px;
  min-width: 536px;
}

 .ex-date .cvv-wraper input {
  height: 50.7px;
}

 .ex-date .cvv-wraper label {
  position: absolute;
  top: -26px;
  left: 0;
}

 .ex-date .cvv-wraper .ccimg {
  position: absolute;
  top: 35%;
  right: 11px;
  translate: 0 -50%;
}

 .ex-date label {
  font-size: 18px;
  color: rgb(123, 151, 172);
}

 .ex-date .ex-date-label {
  flex-basis: 100%;
}

 .select {
  flex-basis: 25%;
  max-width: 120px;
}

 .select.required .selected {
  outline: 1px solid #c10015;
  outline-offset: -1px;
}

.flex {
  display: flex;
}

.position-relative {
  position: relative;
}

.input {
    width: 50%;
    margin: 20px;
}
.select {
  flex-basis: 25%;
  max-width: 120px;
}

.select.required .selected {
  outline: 1px solid #c10015;
  outline-offset: -1px;
}
</style>
