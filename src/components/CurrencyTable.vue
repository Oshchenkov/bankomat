<template>
    <b-container>
        <b-row>
            <b-col class="mb-3"
                ><b-table
                    striped
                    hover
                    :items="getItemsSum"
                    :fields="getFieldsSum"
                ></b-table
            ></b-col>
        </b-row>
    </b-container>
</template>

<script>
export default {
    name: 'CurrencyTable',

    props: {
        items: {
            type: Array,
            required: true,
            default() {
                return [];
            },
        },
        fields: {
            type: Array,
            required: true,
            default() {
                return [];
            },
        },
    },

    computed: {
        getItemsSum() {
            return this.items.map((item) => {
                return {
                    id: item.id,
                    value: item.value,
                    count: item.count,
                    sum: +item.value * +item.count,
                };
            });
        },

        getFieldsSum() {
            const compFields = this.fields;
            compFields.push({
                key: 'sum',
                label: 'Current sum',
                sortable: true,
            });
            return compFields;
        },
    },
};
</script>

<style>
table {
    margin-bottom: 0;
}
</style>
