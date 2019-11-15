<template>
    <div class="sidebar">
        <div class="d-flex">
            <span>Настройки</span>
            <a class="card-btn" href="#" @click.prevent="saveData">
                <span>Сохранить</span>
            </a>
            <a class="close" href="#" @click.prevent="hideSidebar">
                <span style="background-color: red;height: 40px;width: 40px; display: flex"></span>
            </a>
        </div>
        <input-custom
                v-if="data.blockID === 12345"
                v-for="(item, index) in data.settings"
                :key="index"
                :inputProp="item"
                class="mt-4"
        />
        <select class="sidebar-select" v-if="data.blockID === 12346" v-model="selected_answer">
            <option value="" selected>Выберите ваш ответ</option>
            <option value="yes">Да</option>
            <option value="no">Нет</option>
        </select>
    </div>
</template>

<script>
    import InputCustom from '~/components/elements/InputCustom'
    import {mapActions} from "vuex";

    export default {
        props: {
            data: {
                type: Object,
                default: {}
            }
        },
        data() {
            return {
                selected_answer: ''
            }
        },
        components: {
            InputCustom
        },
        methods: {
            ...mapActions('elements', ['setSize']),
            ...mapActions('elements', ['setAnswer']),
            hideSidebar() {
                this.$emit('update-cart')
            },
            saveData() {
                let size;
                this.$bus.$on('DATA_PUBLISHED', (payload) => {
                    if (payload.type === 'width') {
                        size = {
                            width: parseInt(payload.value),
                        }
                    } else if (payload.type === 'height') {
                        size = {
                            height: parseInt(payload.value),
                        }
                    }
                });
                this.$store.commit('elements/SET_SIZE', size);
                this.$store.dispatch('elements/setAnswer',this.selected_answer)
            }
        }
    }
</script>

<style scoped lang="scss">
    .sidebar {
        position: absolute;
        z-index: 99;
        background-color: rgba(169, 169, 169, 0.8);
        width: 25vw;
        height: 100vh;
        padding: 20px 15px;

        &-select {
            width: 150px;
            height: 50px;
        }

        .d-flex {
            display: flex;
            justify-content: space-between;
            height: 100px;
            align-items: center;
        }
    }
</style>
