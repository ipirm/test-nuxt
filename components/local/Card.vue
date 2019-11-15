<template>
    <div class="card"  :style="{ backgroundColor: `${this.data.bgColor}` }">
        <a class="card-btn" href="#" @click.prevent="changeSize">
            <span>Настройки</span>
        </a>
        <textarea
                class="card-textarea"
                v-model="text"
                :style="{ width:  this.width + '%', height:  this.height + 'px',backgroundColor: `${this.data.bgColor}` }"
        >
        </textarea>
    </div>
</template>

<script>
    export default {
        props: {
            data: {
                type: Object,
                default: {}
            }
        },
        created(){
            this.widthCard()
        },
        data() {
            return {
                text: this.data.bText || '',
                width: this.data.settings[0].type === 'width' ? this.data.settings[0].fieldValue : 0,
                height: this.data.settings[1].type === 'height' ? this.data.settings[1].fieldValue : 0,
            }
        },
        methods: {
            changeSize() {
                this.$emit('update-cart', this.data.blockID);
            },
            widthCard() {
                if (this.data.blockID === 12345) {
                    this.$bus.$on('DATA_PUBLISHED', (payload) => {
                        if (payload.type === 'width') this.width = parseInt(payload.value)
                        else if (payload.type === 'height') this.height = parseInt(payload.value)
                    });
                }
            }
        }
    }
</script>

<style scoped lang="scss">
    .card {
        width: 100%;
        height: 50vh;

        &-btn {
            margin: 35px 35px 0 auto;
        }

        &-textarea {
            border: none;
            margin-top: auto;
            text-align: center;
            display: flex;
            justify-content: center;
            align-items: center;
            color: #282828;
            font-size: 16px;
            background-color: #f0fff0;
        }
    }
</style>
