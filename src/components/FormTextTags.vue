<template>
    <div
            :class="{
            'form-item--filled': !isEmpty,
            'form-item--error': formErrors.length && showFormErrors,
            'form-item--tags': tagsList.length > 0,
            'form-item--tags-filled': tagsList.length > 0 && (focus || !hideTextOnBlur),
        }"
            class="form-item"
    >
        <div class="form-item__field">
            <slot name="prepend"></slot>

            <div class="form-item__wrapper">
                <label
                        v-if="showTags"
                        :key="specialId"
                        :for="uid"
                        class="form-item__tags"
                >
                    <div
                            v-for="(s, i) in localValue"
                            :key="i"
                            class="form-item__tag"
                    >
                        <div class="form-item__tag-text">
                            {{ s }}
                        </div>
                        <div
                                class="form-item__tag-remove"
                                @mousedown="removeTag(i)"
                        >
                            x
                        </div>
                    </div>
                </label>

                <input
                        :id="uid"
                        ref="input"
                        v-model.trim="text"
                        :disabled="disabled"
                        :placeholder="placeholder"
                        class="form-item__input"
                        type="text"
                        @blur.prevent="onBlur"
                        @focus="onFocus"
                        @keydown.enter.prevent="addTag"
                >
                <label
                        :for="uid"
                        class="form-item__label"
                >
                    <slot name="label">
                        Label
                    </slot>

                    <slot name="append">
                        <div class="form-item__readonly-icon form-item__readonly-icon--right">
                            <span class="icon icon-caret-down"></span>
                        </div>
                    </slot>
                </label>
            </div>
        </div>

        <div v-if="showFormErrors">
            <!--eslint-disable vue/no-v-html-->
            <div
                    v-for="(error, index) in formErrors"
                    :key="index"
                    class="form-item__error"
                    v-html="error"
            ></div>
            <!--eslint-enable vue/no-v-html-->
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            data: {
                type: Array,
                default: () => ([]),
            },
            placeholder: {
                type: String,
                default: '',
            },
            disabled: {
                type: Boolean,
                default: false,
            },
            blurOnAdd: {
                type: Boolean,
                default: false,
            },
            /**
             * Also v-model
             */
            value: {
                type: [String, Array],
                default: '',
            },
            formErrors: {
                type: [Array, Object],
                default: () => ([]),
            },
            hideTextOnBlur: {
                type: Boolean,
                default: false,
            },
            clearTextOnBlur: {
                type: Boolean,
                default: false,
            },
        },
        data() {
            let text = '';

            if (typeof this.value === 'string') {
                text = this.value;
            }

            return {
                focus: false,
                text,
                showFormErrors: false,
                localValue: this.value,
                // eslint-disable-next-line no-underscore-dangle
                specialId: this._uid,
            };
        },
        computed: {
            uid() {
                // eslint-disable-next-line no-underscore-dangle
                return `form-item-${this._uid}`;
            },
            tagsList() {
                return Array.isArray(this.localValue) ? this.localValue : [this.localValue];
            },
            isEmpty() {
                return this.text === '';
            },
            showTags() {
                return this.tagsList.length > 0;
            },
        },
        watch: {
            formErrors() {
                this.showFormErrors = true;
            },
            value(value) {
                this.localValue = value;
            },
        },
        methods: {

            addTag() {
                const currentItem = this.text;
                this.$emit('entered', currentItem);
                if (currentItem) {
                    this.$emit('input', [...this.tagsList, currentItem]);
                }
                this.clear();
                if (this.blurOnAdd) {
                    this.close();
                }
            },
            onFocus() {
                this.focus = true;

                if (!this.hideTextOnBlur) {
                    this.onInput();
                }

                if (!this.blurOnAdd) {
                    document.addEventListener('click', this.offClick);
                }
            },
            onBlur() {
                if(this.clearTextOnBlur) {
                    this.clear();
                }
                if (this.blurOnAdd) {
                    this.focus = false;
                }
            },
            offClick(ev) {
                if (!this.$el.contains(ev.target)) {
                    this.closeWithOffClick();
                }
            },
            clear() {
                this.text = '';
            },
            close() {
                if (this.blurOnAdd) {
                    this.$refs.input.blur();
                } else {
                    this.closeWithOffClick();
                }
            },
            closeWithOffClick() {
                this.focus = false;
                this.$refs.input.blur();
                document.removeEventListener('click', this.offClick);
            },
            removeTag(index) {
                const newValue = [
                    ...this.localValue.slice(0, index),
                    ...this.localValue.slice(index + 1),
                ];
                this.$emit('input', newValue);
            },
        },
    };
</script>

<style lang="less">
    @import '../less/form-text-tags.less';
</style>
