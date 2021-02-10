<template>
    <div>
        <b-button variant="success" @click="makeToast({variant: 'success'})" class="mb-2"
            >Success</b-button
        >
        <b-button variant="danger" @click="makeToast({variant: 'danger'})" class="mb-2"
            >Danger</b-button
        >
    </div>
</template>

<script>
export default {
    name: 'ToastAlerts',
    data() {
        return {
            toastCount: 0,
        };
    },
    methods: {
        makeToast({ append = false, variant = null }) {
            this.toastCount++;
            this.$bvToast.toast(`This is toast number ${this.toastCount}`, {
                title: `Variant ${variant || 'default'}`,
                autoHideDelay: 5000,
                appendToast: append,
                variant: variant,
                solid: true,
            });
        },

        
    },
};
</script>

<style></style>
