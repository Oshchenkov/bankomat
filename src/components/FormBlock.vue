<template>
    <b-container>
        <form @submit.prevent="$emit('submit', value)">
            <b-row>
                <b-col class="mb-3">
                    <b-form-input
                        @input="$emit('input', $event)"
                        :value="value"
                        type="number"
                        placeholder="Enter your value"
                    ></b-form-input>
                </b-col>
                <b-col class="mb-3">
                    <b-button type="submit" :disabled="!value" variant="success"
                        >Withdrawal</b-button
                    >
                </b-col>
            </b-row>
        </form>
    </b-container>
</template>

<script>
export default {
    name: 'FormBlock',

    props: {
        value: {
            type: [String, Number],
        },
    },
};
</script>

<style></style>
