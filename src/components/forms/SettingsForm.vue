<template>
    <div class="settings-form">
        <f-card class="f-card-double-padding">
            <f-form ref="form" center-form @f-form-change="onFormChange">
                <fieldset>
                    <legend class="not-visible">Settings form</legend>
                    <div class="form-body">
                        <f-select
                            name="currency"
                            label="Currency"
                            select-size="large"
                            :data="currencies"
                            :value="$store.state.currency"
                        />

                        <f-select
                            name="fraction_digits"
                            label="Decimal Points"
                            select-size="large"
                            :data="fractionDigits"
                            :value="$store.state.fractionDigits.toString()"
                        />

                        <f-select name="language" label="Language" select-size="large" :data="language" value="en-US" />

                        <!--
                        <f-input
                            name="defi_slippage_reserve"
                            :value="$store.state.defiSlippageReserve.toString()"
                            label="DeFi Slippage Reserve"
                            type="number"
                            min="0"
                            max="100"
                            step="0.1"
                            field-size="large"
                            validate-on-input
                            :validator="checkDefiSlippageReserve"
                        >
                            <template #bottom="sProps">
                                <f-message v-show="sProps.showErrorMessage" type="error" role="alert" with-icon>
                                    Value must be between 0% and 100%
                                </f-message>
                            </template>
                        </f-input>
                        -->
                        <br />
                        <f-dark-mode-switch />
                    </div>
                </fieldset>
            </f-form>
        </f-card>
    </div>
</template>

<script>
import FCard from '../core/FCard/FCard.vue';
import FForm from '../core/FForm/FForm.vue';
import FSelect from '../core/FSelect/FSelect.vue';
import appConfig from '../../../app.config.js';
import FDarkModeSwitch from '@/components/core/FDarkModeSwitch/FDarkModeSwitch.vue';

export default {
    name: 'SettingsForm',

    components: { FDarkModeSwitch, FSelect, FForm, FCard },

    data() {
        return {
            language: [{ value: 'en-US', label: 'English' }],
            fractionDigits: [
                { value: '1', label: '1' },
                { value: '2', label: '2' },
                { value: '3', label: '3' },
                { value: '4', label: '4' },
                { value: '5', label: '5' },
                { value: '6', label: '6' },
            ],
            darkModeOn: false,
        };
    },

    computed: {
        /**
         * Data for currency select.
         *
         * @return {[]}
         */
        currencies() {
            const selectData = [];
            const { currencies } = appConfig.settings;

            currencies.forEach((_currency) => {
                selectData.push({
                    value: _currency,
                    label: _currency,
                });
            });

            return selectData;
        },
    },

    created() {
        this.darkModeOn = this.$store.state.darkMode;

        /*
        this._useDarkColorScheme = window.matchMedia('(prefers-color-scheme: dark)');

        if (this._useDarkColorScheme) {
            this._useDarkColorScheme.addEventListener('change', this.onDarkMode);
        }
        */
    },

    /*
    beforeDestroy() {
        if (this._useDarkColorScheme) {
            this._useDarkColorScheme.removeEventListener('change', this.onDarkMode);
        }
    },
    */

    methods: {
        /**
         * @param {string} _value
         * @return {boolean}
         */
        checkDefiSlippageReserve(_value) {
            let ok = false;
            const value = parseFloat(_value);

            if (!isNaN(value)) {
                ok = value >= 0 && value <= 100;
            }

            return ok;
        },

        onFormChange(_event) {
            const { detail } = _event;
            const appNode = this.$root.$children[0];

            if (detail.eTarget.name === 'currency') {
                if (appNode) {
                    appNode.setCurrency(detail.value);
                }
            } else if (detail.eTarget.name === 'fraction_digits') {
                if (appNode) {
                    appNode.setFractionDigits(parseInt(detail.value));
                }
                /*
            } else if (detail.eTarget.name === 'defi_slippage_reserve') {
                if (appNode && this.checkDefiSlippageReserve(detail.value)) {
                    appNode.setDefiSlippageReserve(parseFloat(detail.value));
                }
                */
            } else if (detail.eTarget.name === 'language') {
                alert('not implemented yet');
            }
        },

        onDarkMode(_event) {
            this.darkModeOn = _event.matches;
            console.log(this.darkModeOn);
        },
    },
};
</script>
