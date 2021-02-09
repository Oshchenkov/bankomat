<template>
    <div id="app">
        <b-container class="bv-example-row">
            <b-row>
                <b-col class="mb-3"
                    ><b-table
                        striped
                        hover
                        :items="currencyData"
                        :fields="fields"
                    ></b-table
                ></b-col>
            </b-row>
            <b-row>
                <b-col class="mb-3">
                    <b-form-input
                        v-model="inputCurrencyValue"
                        type="number"
                        placeholder="Enter your value"
                    ></b-form-input>
                </b-col>
                <b-col class="mb-3">
                    <b-button
                        :disabled="!inputCurrencyValue"
                        @click="checkCashWithdrawal"
                        variant="success"
                        >Button</b-button
                    >
                </b-col>
            </b-row>
        </b-container>
        <div class="error" v-show="hasError">
            Error
        </div>
    </div>
</template>

<script>
import Vue from 'vue';
import { BootstrapVue } from 'bootstrap-vue';
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap-vue/dist/bootstrap-vue.css';

Vue.use(BootstrapVue);

export default {
    name: 'App',
    components: {},
    data() {
        return {
            inputCurrencyValue: null,
            hasError: null,
            // Note 'isActive' is left out and will not appear in the rendered table
            fields: [
                {
                    key: 'value',
                    sortable: true,
                },
                {
                    key: 'count',
                    label: 'Current count',
                    sortable: true,
                    variant: 'success',
                },
            ],

            currencyData: [
                {
                    id: 1,
                    value: 500,
                    count: 20,
                },
                {
                    id: 2,
                    value: 200,
                    count: 20,
                },
                {
                    id: 3,
                    value: 100,
                    count: 20,
                },
                {
                    id: 4,
                    value: 50,
                    count: 20,
                },
                {
                    id: 5,
                    value: 20,
                    count: 20,
                },
                {
                    id: 6,
                    value: 10,
                    count: 20,
                },
            ],

            operationCashe: {
                currentSum: null,
                operationCounts: [],
            },
        };
    },

    methods: {
        checkCashWithdrawal() {
            if (Number.isInteger(+this.inputCurrencyValue)) {
                this.hasError = false;
            } else {
                this.hasError = true;
            }
        },
        calculateWithdrawalCount() {
            this.operationCashe.currentSum = +this.inputCurrencyValue;

            for (let i = 0; i < this.currencyData.length; i++) {
                if (this.currencyData[i].count) {
                    const resCount = Math.floor(
                        this.operationCashe.currentSum /
                            this.currencyData[i].value
                    );

                    if (resCount <= this.currencyData[i].count) {
                        this.operationCashe.currentSum -=
                            resCount * this.currencyData[i].value;

                        this.operationCashe.operationCounts.push({
                            id: this.currencyData[i].id,
                            value: this.currencyData[i].value,
                            countWithdrawal: resCount,
                        });
                    } else if (resCount == 0) {
                        if (this.operationCashe.currentSum) {
                            break;
                        } else {
                            continue;
                        }
                    } else {
                        this.operationCashe.currentSum -=
                            this.currencyData[i].count *
                            this.currencyData[i].value;

                        this.operationCashe.operationCounts.push({
                            id: this.currencyData[i].id,
                            value: this.currencyData[i].value,
                            countWithdrawal: this.currencyData[i].count,
                        });
                    }
                }
            }

            if (
                this.operationCashe.operationCounts.length &&
                !this.operationCashe.currentSum
            ) {
                this.subtractWithdrawalCount();
            } else {
                this.hasError = true;
                this.cleanWithdrawalCountCashe();
            }
        },

        subtractWithdrawalCount() {
            this.operationCashe.operationCounts.forEach((withdrawalObj) => {
                this.currencyData.forEach((currencyObj, index) => {
                    if (withdrawalObj.id === currencyObj.id) {
                        this.currencyData[index].count -=
                            withdrawalObj.countWithdrawal;
                    }
                });
            });

            this.cleanWithdrawalCountCashe();
        },

        cleanWithdrawalCountCashe() {
            this.operationCashe.operationCounts = [];
        },
    },
};
</script>
