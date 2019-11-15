<template>
    <div class="sidebar-input">
        <span>{{ this.inputProp.Name }}</span>
        <input v-model="size.value" @input="getData"/>
    </div>
</template>

<script>
    export default {
        props: {
            inputProp: {
                type: Object,
                default: ''
            }
        },
        data() {
            return {
                size: {
                    type: this.inputProp.type,
                    value: this.inputProp.fieldValue,
                }
            }
        },
        methods: {
            getData() {
                const regex = /\d+/g;
                let sens = this.size.value.match(regex);
                if (sens !== null) {
                    this.size.value = sens.join('');
                    this.$bus.$emit('DATA_PUBLISHED', this.size)
                }
            },
        }
    }
</script>

<style scoped lang="scss">
    .sidebar-input {
        display: flex;
        flex-direction: column;
        max-width: 150px;

        input {
            margin-top: 10px;
        }
    }
</style>
