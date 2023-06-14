<template>
    <article class="select-wrapper">
        <label v-if="!isInsideLabel && selectLabel" class="select-label">{{ selectLabel }}<span v-if="required"
                style=" color: #c10015;">*</span></label>
        <div class="options-wrapper" v-click-outside="closeOptions" :class="{ above: isOptionsAbove }">
            <!-- <div class="selected" @click="toggleOptions" v-click-outside="closeOptions" :class="{ open: isOptionsShow }" -->
            <div class="selected" @click="toggleOptions" :class="{ open: isOptionsShow }" ref="selectElement"
                :style="{ 'background-color': bgColor, borderColor: isValid ? '' : errorColor }">
                <div class="flex flex-column flex-1">
                    <label v-if="isInsideLabel && selectLabel" class="select-label">{{ selectLabel }}</label>
                    <input v-if="isOptionsShow && isSearch" ref="searchInput" class="value" type="text" @click.stop
                        :placeholder="selectedOption.label" v-model="searchTerm">
                    <span v-else class="value" :class="{ placeholder: !selectedOption.key }">{{ selectedOption.label }}</span>
                    <!-- <input v-show="isOptionsShow && isSearch" ref="searchInput" class="value" type="text" @click.stop :placeholder="selectedOption.label" v-model="searchTerm" > -->
                </div>
                <svg :style="{ 'rotate': isOptionsShow ? '180deg' : '' }" xmlns="http://www.w3.org/2000/svg" height="24" width="24"
                    viewBox="0 0 24 24">
                    <path d="m12 15.375-6-6 1.4-1.4 4.6 4.6 4.6-4.6 1.4 1.4Z" :fill="isValid ? iconColor : errorColor" />
                </svg>
                <div class="error-message" :class="{ hide: isValid }" :style="{ color: errorColor }">Please choose {{ selectLabel ?
                    selectLabel.toLowerCase() : '' }}</div>
            </div>
            <div class="options" :class="{ hide: !isOptionsShow, open: isOptionsShow }" ref="optionsDropdown">
                <div v-for="(option, index) in optionsFormat" :key="option.key + index" class="option"
                    :class="{ 'seperator-option': option.key === '$seperator' }" @click="selectOption(option)">
                    <span v-if="option.key === '$seperator'"></span>
                    <span v-else>{{ option.label }}</span>
                </div>
                <div v-if="!optionsFormat.length && isSearch" class="option no-options" @click.stop>No results</div>
            </div>
        </div>
    </article>
</template>
  
<script>

export default {
    name: 'VSelect',
    props: {
        id: { type: String, required: true },
        options: {
            type: Array,
            default: () => []
        },
        iconColor: {
            type: String,
            default: '#005FAA'
        },
        primaryColor: { type: String, default: '#005faa' },
        errorColor: { type: String, default: '#c10015' },
        selectLabel: { type: String, default: '' },
        isInsideLabel: { type: Boolean, default: false },
        bgColor: { type: String, default: '#fff' },
        defaultOption: {
            type: Object,
            default: () => ({ label: 'Choose', key: '' })
        },
        required: { type: Boolean, default: false },
        isSearch: { type: Boolean, default: false },
        // value: {
        //     type: String,
        //     default: ''
        // },
        modelValue: String,
    },
    data() {
        return {
            selectedOption: this.defaultOption,
            isOptionsShow: false,
            searchTerm: '',
            isValid: true,
            hasOpened: false,
            isOptionsAbove: false,
        }
    },
    mounted() {
        this.selectedOption = this.optionsFormat.find((option) => option.key === this.modelValue) || this.defaultOption
        if (this.required && this.$parent.setInputValidations) {
            this.$parent.setInputValidations({ isValid: this.selectedOption.key, id: this.id, ref: this.$refs['ref' + this.id], validate: this.validate })
            this.$parent.setOrder(this.id)
        }
        // this.$refs.selectElement.addEventListener('scroll', this.updateOptionsPosition)
        window.addEventListener('scroll', this.updateOptionsPosition)
    },
    methods: {
        toggleOptions() {
            this.hasOpened = true
            this.updateOptionsPosition()
            this.isOptionsShow = !this.isOptionsShow
        },
        closeOptions() {
            if (!this.isOptionsShow) return // Because this function called when clickOutside. If i remove this condition validation will be this.isValid === true
            this.isOptionsShow = false
            this.searchTerm = ''
            if (this.hasOpened) this.validate()
            if (this.required && this.$parent.setInputValidations) this.$parent.setInputValidations({ isValid: this.isValid, id: this.id, validate: this.validate })
        },
        selectOption(option) {
            this.selectedOption = option
            this.closeOptions()
            this.$emit('update:modelValue', option.key)
        },
        validate() {
            if (!this.selectedOption.key && this.required) this.isValid = false
            else this.isValid = true
        },
        updateOptionsPosition() {
            const selectRect = this.$refs.selectElement.getBoundingClientRect()
            const spaceAbove = selectRect.top
            const spaceBelow = window.innerHeight - selectRect.bottom
            const optionsDropdown = this.$refs.optionsDropdown
            this.isOptionsAbove = spaceBelow < 240
            const height = spaceBelow < 240 ? spaceAbove - 40 : spaceBelow - 40
            optionsDropdown.style.maxHeight = `${height > 500 ? 500 : height}px`
        },
    },
    computed: {
        selectedValue() {
            return this.selectedOption.key
        },
        optionsFormat() {
            let options = []
            if (!this.options || !this.options.length) return options
            if (typeof this.options[0] === 'object') options = this.options
            else options = this.options.map(o => ({ label: o, key: o }))

            if (this.searchTerm) {
                const regex = new RegExp(this.searchTerm, 'i')
                options = options.filter(o => regex.test(o.label))
                const uniques = []
                const seenLabels = new Set()
                options.forEach(o => {
                    if (!seenLabels.has(o.label)) {
                        seenLabels.add(o.label)
                        uniques.push(o)
                    }
                })
                return uniques
            }
            return options
        },
    },
    directives: {
        ClickOutside: {
            mounted(el, { value: cb, arg }) {
                el.clickOutside = (ev) => {
                    if (!el.contains(ev.target)) {
                        cb(arg)
                    }
                }
                setTimeout(() => {
                    document.addEventListener('click', el.clickOutside)
                }, 0)
            },
            unmounted(el) {
                document.removeEventListener('click', el.clickOutside)
            },
        }
    },
    beforeUnmount() {
        window.removeEventListener('scroll', this.updateOptionsPosition)
    },
}
</script>
<style>
::-webkit-scrollbar {
    width: 6px;
    height: 3px;
}

/* Track */
::-webkit-scrollbar-track {
    background: #f1f1f1;
}

/* Handle */
::-webkit-scrollbar-thumb {
    background: #bebebe;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
    background: #a7acaf;
}

.select-wrapper {
    position: relative;
    overflow: visible;
    width: 100%;
    margin-bottom: 20px;
}

.select-wrapper .options-wrapper {
    position: relative;
    overflow: visible;
}

.select-wrapper .options-wrapper .selected {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border: 1px solid #ccc;
    padding: 8px;
    cursor: pointer;
    min-width: 120px;
    border-radius: 4px;
    transition: rotate .3s;
}

.select-wrapper .options-wrapper .selected.open {
    border-radius: 4px 4px 0 0;
    border: 1px solid #005faa;
}

.select-wrapper .options-wrapper .selected .flex-column {
    row-gap: 2px;
}

.select-wrapper .options-wrapper .selected .select-label {
    color: #7B97AC;
    font-size: 13px;
    line-height: 1;
}

.select-wrapper .options-wrapper .selected .value {
    color: #002644;
    font-size: 18px;
    line-height: 1;
}

.select-wrapper .options-wrapper .selected .value.placeholder {
    color: #75757E;
}

.select-wrapper .options-wrapper .selected input.value {
    padding: 0;
    width: 100%;
    border: none;
    outline: none;
}

.select-wrapper .options-wrapper .selected.position-relative {
    position: relative;
}

.select-wrapper .options-wrapper .selected .error-message {
    font-size: 14px;
    position: absolute;
    left: 0;
    bottom: -22px;
    transition: .4s;
    font-family: sans-serif;
}

.select-wrapper .options-wrapper .selected .error-message.hide {
    translate: 0 10px;
    opacity: 0;
    pointer-events: none;
}

.select-wrapper .options-wrapper .selected svg {
    transition: rotate 0.2s ease 0s;
}

.select-wrapper .options-wrapper .options {
    box-sizing: border-box;
    position: absolute;
    width: 100%;
    border: 1px solid #ccc;
    background-color: #fff;
    z-index: 30;
    max-height: 200px;
    overflow-y: auto;
    border-radius: 0 0 4px 4px;
    transition: transform .2s ease-in-out;
    transform-origin: top;
    padding-bottom: 3px;
}

.select-wrapper .options-wrapper .options.open {
    border: 1px solid #005faa;
    border-top: none;
    transform: scaleY(1);
}

.select-wrapper .options-wrapper .options.hide {
    transform: scaleY(0);
}

.select-wrapper .options-wrapper.above .selected.open {
    border-radius: 0 0 4px 4px;
    /* border-top: none; */
}

.select-wrapper .options-wrapper.above .options {
    top: 0;
    translate: 0 calc(-100% - 1px);
    transform-origin: bottom;
    border-radius: 4px 4px 0 0;
    padding-top: 3px;
    border: 1px solid #005faa;
    border-bottom: none;
}

.select-wrapper .options-wrapper .options .option {
    padding: 8px;
    cursor: pointer;
}

.select-wrapper .options-wrapper .options .option:hover {
    background-color: #f5f5f5;
}

.select-wrapper .options-wrapper .options .option.seperator-option {
    pointer-events: none;
    border-bottom: 1px dashed #002644;
    padding-bottom: 0;
    margin: 0 8px 8px;
}

.select-wrapper .options-wrapper .options .option.no-options {
    color: #9e9e9e;
    cursor: default;
}

.select-wrapper .options-wrapper .options .option.no-options:hover {
    background-color: #fff;
}
</style>
