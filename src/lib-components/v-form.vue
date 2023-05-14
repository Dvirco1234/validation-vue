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
            inputValidations: [],
        }
    },
    created() { },
    methods: {
        setInputValidations({ isValid, idx, ref, validate }) {
            this.inputValidations[idx] = { isValid, ref, validate }
        },
        // isFormValid() {
        //     const isValid = !this.inputValidations.some(i => !i.isValid)
        //     if (!isValid) {
        //         const invalids = this.inputValidations.filter(i => !i.isValid)
        //         invalids.forEach(i => i.validate())
        //         if(invalids[0].ref) invalids[0].ref.focus()
        //     }
        //     return isValid
        // },
        async isFormValid() {
            const isValid = !this.inputValidations.some(i => !i.isValid)
            if (!isValid) {
                const invalids = this.inputValidations.filter(i => !i.isValid)
                for (const i of invalids) {
                    await i.validate()
                }
                if (invalids[0].ref) invalids[0].ref.focus()
            }
            return isValid
        },
        async onSubmit(ev) {
            ev.preventDefault()
            ev.stopPropagation()
            const isValid = await this.isFormValid()
            // this.$emit('submit', isValid)
            this.$emit('submitForm', isValid)
        }
    },
    mounted() {
        console.log('this.$refs.form.children:', this.$refs.form.children)
        // console.log('this.$parent: ', this.$parent.$.components.InputValidation);
        // console.log('this.$parent: ', this.$refs);
        // console.log('this.$children: ', this.$children);
        // this.$parent.$.components.InputValidation.$on('check', (data) => {
        //     console.log('data: ', data);
        // })
    },
}
</script>