<template>
    <b-container>
        <b-row>
            <b-col class="mb-3"
                ><b-table
                    striped
                    hover
                    :items="items"
                    :fields="fields"
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
};
</script>

<style>
table {
    margin-bottom: 0;
}
</style>
