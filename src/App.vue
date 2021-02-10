<template>
    <div id="app">
        <b-container>
            <h1 class="my-5">
                Bankomat
            </h1>
        </b-container>
        <CurrencyTable :items="currencyData" :fields="fields" />
        <FormBlock v-model="inputCurrencyValue" @submit="checkCashWithdrawal" />

        <div class="error" v-show="hasError">
            Error
        </div>
        <ToastAlerts />
    </div>
</template>

<script>
import Vue from 'vue';
import { BootstrapVue } from 'bootstrap-vue';
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap-vue/dist/bootstrap-vue.css';
import CurrencyTable from './components/CurrencyTable';
import FormBlock from './components/FormBlock';
import ToastAlerts from './components/ToastAlerts';

Vue.use(BootstrapVue);

export default {
    name: 'App',

    components: {
        CurrencyTable,
        FormBlock,
        ToastAlerts,
    },

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
                    // variant: 'success',
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
                this.toggleError(false);

                this.calculateWithdrawalCount();
            } else {
                this.toggleError(true);
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
                        this.updateOperationCurrentSum({
                            count: resCount,
                            value: this.currencyData[i].value,
                        });

                        this.addOperationCount({
                            id: this.currencyData[i].id,
                            value: this.currencyData[i].value,
                            countWithdrawal: resCount,
                        });
                    } else if (resCount == 0) {
                        if (this.operationCashe.currentSum) {
                            break;
                        }
                        continue;
                    } else {
                        this.updateOperationCurrentSum({
                            count: this.currencyData[i].count,
                            value: this.currencyData[i].value,
                        });

                        this.addOperationCount({
                            id: this.currencyData[i].id,
                            value: this.currencyData[i].value,
                            countWithdrawal: this.currencyData[i].count,
                        });
                    }
                }
            }

            this.showWithdrawalResult();
        },

        updateOperationCurrentSum({ count, value }) {
            this.operationCashe.currentSum -= count * value;
        },

        addOperationCount({ id, value, countWithdrawal }) {
            this.operationCashe.operationCounts.push({
                id,
                value,
                countWithdrawal,
            });
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

        toggleError(status = false) {
            this.hasError = status;
        },

        showWithdrawalResult() {
            if (
                this.operationCashe.operationCounts.length &&
                !this.operationCashe.currentSum
            ) {
                this.subtractWithdrawalCount();
            } else {
                this.toggleError(true);
                this.cleanWithdrawalCountCashe();
            }
        },
    },
};
</script>
