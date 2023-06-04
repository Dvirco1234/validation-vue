<template>
    <form ref="form" class="form-validation" @submit.prevent="onSubmit">
        <slot></slot>
    </form>
</template>
<script>
export default {
    name: 'VForm',
    // props: {
    //     beforeSubmitRule: {
    //         type: Function,
    //         default: null
    //     }
    // },
    data() {
        return {
            // inputValidations: [],
            inputValidations: {},
            inputsOrder: [],
            isSubmitting: false,
            hasSubmitted: false,
        }
    },
    methods: {
        setInputValidations({ isValid, id, ref, validate, hasSubmitRule }) {
            this.inputValidations[id] = { isValid, ref, validate, hasSubmitRule }
        },
        setOrder(id) {
            this.inputsOrder.push(id)
        },
        async isFormValid() {
            let isValid = true
            let firstInvalid = null

            for (const id of this.inputsOrder) {
                const input = this.inputValidations[id]
                if (!input.isValid || input.hasSubmitRule) {
                    isValid = false
                    await input.validate()
                    if (!firstInvalid) firstInvalid = input
                }
            }
            if (firstInvalid?.ref) firstInvalid.ref.focus()
            return isValid
        },
        async onSubmit(ev) {
            ev.preventDefault()
            ev.stopPropagation()
            this.$emit('submitStart', ev)
            this.hasSubmitted = true
            this.isSubmitting = true
            const isValid = await this.isFormValid()
            // this.$emit('submit', isValid)
            // this.$emit('submitForm', isValid)
            this.$emit('submitForm', { event: ev, isValid })
            this.isSubmitting = false
        }
    },
}
</script>