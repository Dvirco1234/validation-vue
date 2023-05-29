<template>
    <form ref="form" class="form-validation" @submit.prevent="onSubmit">
        <slot></slot>
    </form>
</template>
<script>
export default {
    name: 'VForm',
    props: { type: Object },
    data() {
        return {
            // inputValidations: [],
            inputValidations: {},
            inputsOrder: [],
        }
    },
    created() { },
    mounted() {
        
        // console.log('this.inputsOrder: ', this.inputsOrder);
        // console.log('this.inputValidations: ', this.inputValidations);
    },
    methods: {
        // setInputValidations({ isValid, idx, ref, validate }) {
        //     this.inputValidations[idx] = { isValid, ref, validate }
        // },
        setInputValidations({ isValid, id, ref, validate }) {
            this.inputValidations[id] = { isValid, ref, validate }
        },
        setOrder(id) {
            this.inputsOrder.push(id)
        },
        // async isFormValid() {
        //     const isValid = !this.inputValidations.some(i => !i.isValid)
        //     if (!isValid) {
        //         const invalids = this.inputValidations.filter(i => !i.isValid)
        //         for (const i of invalids) {
        //             await i.validate()
        //         }
        //         if (invalids[0].ref) invalids[0].ref.focus()
        //     }
        //     return isValid
        // },
        async isFormValid() {
            let isValid = true
            let firstInvalid = null

            for (const id of this.inputsOrder) {
                const input = this.inputValidations[id]
                if (!input.isValid) {
                    isValid = false
                    await input.validate()
                    if (!firstInvalid) firstInvalid = input
                    // if (firstInvalid.ref) input.ref.focus()
                }
            }
            if (firstInvalid?.ref) firstInvalid.ref.focus()
            return isValid
        },
        async onSubmit(ev) {
            ev.preventDefault()
            ev.stopPropagation()
            const isValid = await this.isFormValid()
            // this.$emit('submit', isValid)
            // this.$emit('submitForm', isValid)
            this.$emit('submitForm', {event: ev, isValid})
        }
    },
    // mounted() {
        // console.log('this.$refs.form.children:', this.$refs.form.children)
        // console.log('this.$parent: ', this.$parent.$.components.InputValidation);
        // console.log('this.$parent: ', this.$refs);
        // console.log('this.$children: ', this.$children);
        // this.$parent.$.components.InputValidation.$on('check', (data) => {
        //     console.log('data: ', data);
        // })
    // },
}
</script>