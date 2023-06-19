<template>
    <section>
        <label :for="id" :style="{ color: txtColor }">{{ label }}<span v-if="required && label && !hideAsterisk"
                :style="{ color: errorColor }">*</span></label>
        <div class="input-wrapper" :class="{ mb: !checklist }" :style="{ '--placeholder-color': placeholderColor }">
            <textarea v-if="textareaRows && readonly" :rows="textareaRows" :value="modelValue" readonly class="readonly"
                :placeholder="placeholder"></textarea>
            <textarea v-else-if="textareaRows" :rows="textareaRows" :ref="'ref' + id" :id="id" :name="id" :placeholder="placeholder"
                @input="handleInput($event.target.value)" @blur="validate()" :value="modelValue"
                :style="{ 'outline-color': showErrorMessage ? errorColor : '' }"></textarea>

            <input v-else-if="readonly" type="text" :value="modelValue" readonly class="readonly" :placeholder="placeholder" :class="{'no-focus': preventFocus}">

            <input v-else :ref="'ref' + id" :id="id" :name="id" :type="isPasswordShown || type === 'cc' ? 'text' : type"
                :placeholder="placeholder" :maxlength="inputMaxLength" @input="handleInput($event.target.value)" @blur="validate()"
                :value="modelValue" autocomplete="off"
                :style="{ 'outline-color': showErrorMessage ? errorColor : showValidIcon && isValid ? validColor : '', 'padding-inline-start': showPasswordIcon && type === 'password' ? '42px' : '' }" />
            <div class="valid-icon-wrapper">
                <span v-if="!showErrorMessage && isValid && showValidIcon" class="icon success-icon">
                    <svg xmlns="http://www.w3.org/2000/svg" height="24" width="24">
                        <path
                            d="m10.6 16.6 7.05-7.05-1.4-1.4-5.65 5.65-2.85-2.85-1.4 1.4ZM12 22q-2.075 0-3.9-.788-1.825-.787-3.175-2.137-1.35-1.35-2.137-3.175Q2 14.075 2 12t.788-3.9q.787-1.825 2.137-3.175 1.35-1.35 3.175-2.138Q9.925 2 12 2t3.9.787q1.825.788 3.175 2.138 1.35 1.35 2.137 3.175Q22 9.925 22 12t-.788 3.9q-.787 1.825-2.137 3.175-1.35 1.35-3.175 2.137Q14.075 22 12 22Z"
                            :fill="validColor" />
                    </svg>
                </span>
                <span v-if="showErrorMessage" class="icon error-icon" :title="errorMessage">
                    <svg xmlns="http://www.w3.org/2000/svg" height="24" width="24">
                        <path
                            d="M12 17q.425 0 .713-.288Q13 16.425 13 16t-.287-.713Q12.425 15 12 15t-.712.287Q11 15.575 11 16t.288.712Q11.575 17 12 17Zm-1-4h2V7h-2Zm1 9q-2.075 0-3.9-.788-1.825-.787-3.175-2.137-1.35-1.35-2.137-3.175Q2 14.075 2 12t.788-3.9q.787-1.825 2.137-3.175 1.35-1.35 3.175-2.138Q9.925 2 12 2t3.9.787q1.825.788 3.175 2.138 1.35 1.35 2.137 3.175Q22 9.925 22 12t-.788 3.9q-.787 1.825-2.137 3.175-1.35 1.35-3.175 2.137Q14.075 22 12 22Z"
                            :fill="errorColor" />
                    </svg>
                </span>
            </div>
            <div class="show-password-icon" v-if="showPasswordIcon && type === 'password'" @click.stop="togglePasswordShown">
                <span v-if="!isPasswordShown" class="icon password-icon open-eye-icon">
                    <!-- OPEN EYE -->
                    <svg xmlns="http://www.w3.org/2000/svg" width="24.75" height="16.5" viewBox="0 0 24.75 16.5">
                        <path id="Icon_awesome-eye" data-name="Icon awesome-eye"
                            d="M24.6,12.123A13.782,13.782,0,0,0,12.375,4.5,13.784,13.784,0,0,0,.15,12.123a1.39,1.39,0,0,0,0,1.254A13.782,13.782,0,0,0,12.375,21,13.784,13.784,0,0,0,24.6,13.377,1.39,1.39,0,0,0,24.6,12.123ZM12.375,18.938a6.188,6.188,0,1,1,6.188-6.188A6.188,6.188,0,0,1,12.375,18.938Zm0-10.313a4.1,4.1,0,0,0-1.088.163,2.056,2.056,0,0,1-2.875,2.875,4.116,4.116,0,1,0,3.962-3.037Z"
                            transform="translate(0 -4.5)" :fill="primaryColor" />
                    </svg>
                </span>
                <span v-else class="icon password-icon close-eye-icon">
                    <!-- CLOSE EYE -->
                    <svg xmlns="http://www.w3.org/2000/svg" width="23.105" height="19.8" viewBox="0 0 23.105 19.8">
                        <path id="Icon_ionic-md-eye-off" data-name="Icon ionic-md-eye-off"
                            d="M13.805,8.666a5.237,5.237,0,0,1,5.254,5.213,5.014,5.014,0,0,1-.376,1.908l3.068,3.042a12.326,12.326,0,0,0,3.6-4.95A12.443,12.443,0,0,0,9.613,6.789l2.269,2.253A5.2,5.2,0,0,1,13.805,8.666ZM3.3,5.825,5.7,8.2l.485.48a12.268,12.268,0,0,0-3.934,5.2A12.461,12.461,0,0,0,18.4,20.819l.443.438L21.926,24.3l1.335-1.325L4.632,4.5Zm5.806,5.759L10.737,13.2a2.954,2.954,0,0,0-.082.675A3.135,3.135,0,0,0,13.805,17a2.963,2.963,0,0,0,.681-.082l1.629,1.614a5.227,5.227,0,0,1-7.564-4.661A5.15,5.15,0,0,1,9.108,11.585Zm4.527-.81,3.31,3.284.021-.165a3.135,3.135,0,0,0-3.15-3.125Z"
                            transform="translate(-2.25 -4.5)" :fill="primaryColor" />
                    </svg>
                </span>
            </div>
            <div v-if="!isChecklist" class="error-message" :class="{ hide: !showErrorMessage }" :style="{ color: errorColor }">{{ errorMessage
            }}</div>
        </div>
        <article v-if="checklist" class="checklist" :class="{ grid: isChecklistGrid }" :style="{ color: txtColor }">
            <div v-for="(check, idx) in checklist" :key="`${checklist.errorMessage}-${idx}`" class="list flex align-center">
                <svg v-if="check.isValid" xmlns="http://www.w3.org/2000/svg" height="20" width="20">
                    <path
                        d="m8.938 13 4.958-4.938L12.833 7l-3.895 3.875-1.771-1.75-1.063 1.063ZM10 18q-1.646 0-3.104-.625-1.458-.625-2.552-1.719t-1.719-2.552Q2 11.646 2 10q0-1.667.625-3.115.625-1.447 1.719-2.541Q5.438 3.25 6.896 2.625T10 2q1.667 0 3.115.625 1.447.625 2.541 1.719 1.094 1.094 1.719 2.541Q18 8.333 18 10q0 1.646-.625 3.104-.625 1.458-1.719 2.552t-2.541 1.719Q11.667 18 10 18Z"
                        :fill="validColor" />
                </svg>
                <svg v-else xmlns="http://www.w3.org/2000/svg" height="20" width="20">
                    <path
                        d="M7.062 14 10 11.062 12.938 14 14 12.938 11.062 10 14 7.062 12.938 6 10 8.938 7.062 6 6 7.062 8.938 10 6 12.938ZM10 18q-1.646 0-3.104-.625-1.458-.625-2.552-1.719t-1.719-2.552Q2 11.646 2 10q0-1.667.625-3.115.625-1.447 1.719-2.541Q5.438 3.25 6.896 2.625T10 2q1.667 0 3.115.625 1.447.625 2.541 1.719 1.094 1.094 1.719 2.541Q18 8.333 18 10q0 1.646-.625 3.104-.625 1.458-1.719 2.552t-2.541 1.719Q11.667 18 10 18Z"
                        :fill="errorColor" />
                </svg>
                <span>{{ check.errorMessage }}</span>
            </div>
        </article>
    </section>
</template>
  
<script>
export default {
    name: 'VInput',
    props: {
        id: { type: String, required: true },
        // idx: { type: Number, required: true },
        type: { type: String, default: 'text' },
        placeholder: { type: String, default: '' },
        label: { type: String, default: '' },
        // invalidTerm: { type: String, default: '' },
        readonly: { type: Boolean, default: false },
        preventFocus: { type: Boolean, default: false },
        focus: { type: Boolean, default: false },
        autofocus: { type: Boolean, default: false },
        showValidIcon: { type: Boolean, default: false },
        showPasswordIcon: { type: Boolean, default: false },
        required: { type: Boolean, default: false },
        isChecklist: { type: Boolean, default: false },
        isDigitsOnly: { type: Boolean, default: false },
        hideAsterisk: { type: Boolean, default: false },
        isChecklistGrid: { type: Boolean, default: false },
        // TODO: add option for string and then parseInt
        maxLength: { type: Number, default: Infinity },
        minLength: { type: Number, default: 0 },
        modelValue: String,
        rules: { type: Array, default: () => [] },
        asyncRule: { type: Function, default: null },
        submitRule: { type: Function, default: null },
        validateOnSubmitOnly: { type: Boolean, default: false },
        textareaRows: { type: Number, default: 0 },
        // errorMessage: { type: String, default: 'This field is required' },
        errorColor: { type: String, default: '#c10015' },
        validColor: { type: String, default: '#7CB261' },
        // primaryColor: { type: String, default: '#005faa' },
        primaryColor: { type: String, default: '#7b97ac' },
        txtColor: { type: String, default: '#7B97AC' },
        placeholderColor: { type: String, default: '#7B97AC' },

    },
    emits: ['update:modelValue', 'checkIsVaild', 'check'],
    data() {
        return {
            inputValue: this.modelValue,
            rulesFormat: [],
            isBlured: false,
            isValid: false,
            showErrorMessage: false,
            errorMessage: '',
            checklist: null,
            isPasswordShown: false,
            inputMinLength: this.minLength,
            inputMaxLength: this.maxLength,
            cardType: '',
            validationOptsMap: {
                required: (val) => ({
                    isValid: val && val.length ? true : false,
                    errorMessage: this.label ? '' : 'Required Field',
                }),
                length: (val) => ({
                    isValid: val.length >= this.inputMinLength && val.length <= this.inputMaxLength,
                    errorMessage: 'Invalid length',
                }),
                minLength: (val) => ({
                    isValid: val.length >= this.inputMinLength,
                    errorMessage: `${this.inputMinLength} characters minimum`,
                }),
                maxLength: (val) => ({
                    isValid: val.length <= this.inputMaxLength,
                    errorMessage: `${this.inputMaxLength} characters maximum`,
                }),
                email: (val) => ({
                    isValid: (/^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/.test(val)),
                    errorMessage: 'Please enter a valid email address',
                }),
                cc: (val) => ({
                    isValid: this.isCcValid(val),
                    errorMessage: 'Invalid credit card number',
                }),
                // invalidTerm: (val) => ({
                //     isValid: this.invalidTerm !== val,
                // }),
            },
        }
    },
    created() {
        this.formatRules()
        if (this.isChecklist) this.validateChecklist()
    },
    async mounted() {
        if (this.autofocus) this.applyFocus()
        const { isValid } = await this.checkValidation()
        if (this.required && this.$parent.setInputValidations && !this.readonly) {
            this.$parent.setInputValidations({ isValid, id: this.id, ref: this.$refs['ref' + this.id], validate: this.validate, hasSubmitRule: this.submitRule ? true : false })
            this.$parent.setOrder(this.id)
        }
    },
    methods: {
        async handleInput(value) {
            if (this.isDigitsOnly) value = value.replace(/\D/g, '')
            if (this.type === 'cc') value = this.formatCreditCard(value)
            if (this.asyncRule) this.isBlured = true
            this.$refs['ref' + this.id].value = value
            this.inputValue = value
            this.$emit('update:modelValue', value)
            let isValid = false
            // try {
            if (this.isBlured || value.length === this.inputMaxLength) ({ isValid } = await this.validate())
            else ({ isValid } = await this.checkValidation())
            // } catch (error) {
            //     isValid = false
            // }
            // if (this.required && this.$parent.setInputValidations && !this.readonly) this.$parent.setInputValidations({ isValid, id: this.id, ref: this.$refs['ref' + this.id], validate: this.validate, hasSubmitRule: this.submitRule ? true : false })
            if (this.required && this.$parent.setInputValidations && !this.readonly) this.$parent.setInputValidations({ isValid, id: this.id })
        },
        async validate() {
            this.isBlured = true
            if (this.isChecklist) return this.validateChecklist()
            // if (this.invalidTerm) this.rulesFormat.push(this.validationOptsMap.invalidTerm)
            const { isValid, errorMessage } = await this.checkValidation()
            this.errorMessage = errorMessage || `Please enter ${this.label.toLowerCase() || 'a valid value'}`
            this.showErrorMessage = !isValid
            this.isValid = isValid
            return { isValid }
        },
        async checkValidation() {
            let validationResult = this.rulesFormat.reduce((acc, validationRule) => {
                if (!acc.isValid) return acc
                const { isValid, errorMessage } = validationRule(this.inputValue)
                return { isValid, errorMessage }
            }, { isValid: true, errorMessage: '' })
            if (this.asyncRule) validationResult = await this.asyncRule(this.inputValue).catch(err => {
                validationResult = { isValid: false }
                console.error(err)
                // throw err
            })
            // if (this.submitRule && this.$parent.isSubmitting) validationResult = await this.submitRule(this.inputValue).catch(err => {
            //     console.error(err)
            //     validationResult = {isValid: false}
            // })
            if (this.submitRule) {
                const shouldValidate = !this.validateOnSubmitOnly ? this.$parent.hasSubmitted : this.$parent.isSubmitting
                if (shouldValidate) validationResult = await this.submitRule(this.inputValue).catch(err => {
                    validationResult = { isValid: false }
                    console.error(err)
                    // throw err
                })
            }
            // return this.isChecklist ? { ...validationResult, errorMessage: '' } : validationResult
            return validationResult
        },
        validateChecklist() {
            this.isBlured = true
            this.checklist = this.rulesFormat.map(validationRule => validationRule(this.inputValue))
            return { isValid: !this.checklist.some(li => !li.isValid) }
        },
        togglePasswordShown() {
            this.isPasswordShown = !this.isPasswordShown
        },
        formatRules() {
            let rules = []
            if (this.rules?.length) rules = this.rules.map(opt => {
                if (typeof opt === 'string') {
                    if (opt.includes('Length')) {
                        const [ruleName, lengthValue] = opt.split(':')
                        ruleName === 'minLength' ? this.inputMinLength = parseInt(lengthValue) : this.inputMaxLength = parseInt(lengthValue)
                        return this.validationOptsMap[ruleName]
                    }
                    return this.validationOptsMap[opt]
                }
                return opt
            })
            if (this.required && !this.isChecklist) rules.push(this.validationOptsMap['required'])
            if (this.minLength) rules.push(this.validationOptsMap['minLength'])
            if (this.maxLength !== Infinity) rules.push(this.validationOptsMap['maxLength'])
            rules = rules.filter(r => r)
            this.rulesFormat = [...new Set(rules)]
        },
        formatCreditCard(val) {
            let input = val.replace(/\D/g, "")
            if (/^4/.test(input)) {
                this.cardType = 'visa'
                this.inputMaxLength = 19
            } else if (/^5[1-5]/.test(input)) {
                this.cardType = 'mastercard'
                this.inputMaxLength = 19
            } else if (/^3[47]/.test(input)) {
                this.cardType = 'amex'
                this.inputMaxLength = 17
            } else if (/^6(?:011|5)/.test(input)) {
                this.cardType = 'discover'
                this.inputMaxLength = 19
            }

            let formattedInput = ''
            if (this.cardType === "amex") {
                formattedInput = input.replace(/^(\d{4})(\d{1,6})(\d{1,5})?/, "$1 $2 $3").trim()
            } else {
                formattedInput = input.replace(/(\d{4})/g, "$1 ").trim()
            }
            return formattedInput
        },
        isCcValid(val) {
            if (!val) return false
            if (val.length !== this.inputMaxLength) return false
            const input = val.replace(/\s/g, '')
            switch (this.cardType) {
                case 'visa':
                case 'mastercard':
                case 'discover':
                    return this.validateLuhnAlgorithm(input)
                case 'amex':
                    return /^3[47][0-9]{13}$/.test(input)
                default:
                    return false
            }
        },
        validateLuhnAlgorithm(input) {
            let sum = 0
            let double = false
            for (let i = input.length - 1; i >= 0; i--) {
                let digit = parseInt(input.charAt(i))
                if (double) {
                    digit *= 2
                    if (digit > 9) {
                        digit -= 9
                    }
                }
                sum += digit
                double = !double
            }
            return sum % 10 === 0
        },
        applyFocus() {
            if (!this.$refs['ref' + this.id]) return
            this.$refs['ref' + this.id].focus()
        },
    },
    computed: {
        hasMultipleRequirements() {
            return this.rulesFormat.length > 1
        },
    },
    watch: {
        modelValue(newValue) {
            this.inputValue = newValue
        },
        readonly(isReadonly) {
            if (isReadonly) return
            if (this.required && this.$parent.setInputValidations && !this.readonly) {
                // this.$parent.setInputValidations({ isValid: this.isValid, id: this.id, ref: this.$refs['ref' + this.id], validate: this.validate, hasSubmitRule: this.submitRule ? true : false })
                this.$parent.setInputValidations({ isValid: this.isValid, id: this.id, ref: this.$refs['ref' + this.id], validate: this.validate, hasSubmitRule: this.submitRule ? true : false })
                this.$parent.setOrder(this.id)
            }
            if (this.focus) setTimeout(() => this.applyFocus())
        },
        focus(isFocus) {
            if (isFocus) this.applyFocus()
        },
        async required(required) {
            if (!required) {
                this.$parent.setInputValidations({ isValid: true, id: this.id, ref: this.$refs['ref' + this.id], validate: this.validate, hasSubmitRule: this.submitRule ? true : false })
                this.showErrorMessage = false
            } else {
                const { isValid } = await this.checkValidation()
                this.$parent.setInputValidations({ isValid, id: this.id, ref: this.$refs['ref' + this.id], validate: this.validate, hasSubmitRule: this.submitRule ? true : false })
            }
        },
    },
}
</script>
<style scoped>
.input-wrapper {
    display: flex;
    align-items: center;
    position: relative;
    font-size: 20px;
}

.input-wrapper.mb {
    margin-bottom: 20px;
}

label {
    font-size: 18px;
    font-family: sans-serif;
}

.input-wrapper input,
textarea {
    flex: 1;
    padding: 8px 12px;
    border-radius: 4px;
    border: none;
    outline: 1px solid #ccc;
    outline-offset: -1px;
    font-size: 16px;
    resize: none;
}

/* .input-wrapper input.no-focus:focus {
    outline: 1px solid #ccc;
} */

.input-wrapper input::placeholder,
textarea::placeholder {
    color: var(--placeholder-color);
}

.input-wrapper input:focus:not(.no-focus) {
    outline-color: #005FAA;
}

.input-wrapper .valid-icon-wrapper {
    position: absolute;
    right: 10px;
    display: flex;
    align-items: center;
    height: 100%;
}

.input-wrapper .valid-icon-wrapper:dir(rtl) {
    right: auto;
    left: 10px;
}

.input-wrapper .show-password-icon {
    position: absolute;
    left: 10px;
    display: flex;
    align-items: center;
    height: 100%;
    cursor: pointer;
}

.input-wrapper .icon-placeholder {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    border: 1px solid #ccc;
}

.input-wrapper .success-icon {
    color: #5cb85c;
}

.input-wrapper .error-icon {
    color: #d9534f;
    /* cursor: pointer; */
}

.error-message {
    color: #d9534f;
    /* margin-top: 2px; */
    font-size: 14px;
    position: absolute;
    /* top: 100%; */
    left: 0;
    bottom: -22px;
    transition: .4s;
    font-family: sans-serif;
}

.error-message.hide {
    translate: 0 10px;
    opacity: 0;
    pointer-events: none;
}

span.icon {
    display: flex;
    justify-content: center;
    align-items: center;
}

.checklist {
    margin-top: 5px;
}

.checklist.grid {
    display: grid;
    grid-template-columns: max-content 1fr;
    column-gap: 20px;
    row-gap: 2px;
}

.checklist .list {
    gap: 2px;
}

.checklist span {
    translate: 0 1px;
    font-size: 16px;
    font-family: sans-serif;
}

.flex {
    display: flex;
}

.align-center {
    align-items: center;
}
</style>