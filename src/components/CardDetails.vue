<template>
    <div class="modal-backdrop" @click="close">
        <div class="modal" @click.stop="">
            <header>
                    <h3 contenteditable @blur="onEdit" v-html="card.title"></h3>
                <div class="close-button" @click="close">x</div>
            </header>
            <article>
                <label>
                    <textarea v-model="card.body"></textarea>
                </label>
            </article>
        </div>
    </div>
</template>

<script>
    const CardDetails = {
        name: 'CardDetails',
        props: ['card'],
        methods: {
            close() {
                this.$emit('close');
            },
            onEdit(evt) {
                this.card.title = evt.target.innerText;
            }
        },
    };
    export default CardDetails;
</script>

<style lang="scss" scoped>
    .modal-backdrop {
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        background-color: rgba(0, 0, 0, 0.3);
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .modal {
        min-height: auto;
        background: #FFFFFF;
        box-shadow: 2px 2px 20px 1px;
        overflow-x: auto;
        display: flex;
        flex-direction: column;
        margin-bottom: 15px;
        position: relative;
        align-items: center;
        padding: 30px 15px 40px;
        border-radius: 8px;
        width: 800px;
        height: calc(100% - 200px);
    }

    .modal header {
        font-size: 18px;
        width: 100%;
        word-wrap: break-word;
        padding-bottom: 20px;
    }

    .modal article {
        font-size: 18px;
        width: 100%;
        word-wrap: break-word;
    }

    .modal input, textarea {
        width: 100%;
    }

    .close-button {
        position: absolute;
        top: 6px;
        right: 15px;
        font-size: 22px;
        cursor: pointer;
    }

    .close-button:hover {
        opacity: 0.8;
    }


</style>