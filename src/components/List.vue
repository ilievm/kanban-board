<template>
    <div class="list" draggable="true" @dragstart="onDragStart" @drop="onDrop" @dragover.prevent>
        <div class="close-button" @click="removeList">
            X
        </div>
        <div class="title">
            {{ title }}
        </div>

        <div class="cards" v-if="cardCount > 0">
            <card v-for="(card, index) in cards" :title="card.title" :key="index" :index="index"></card>
        </div>
        <card-draft></card-draft>
    </div>
</template>

<script>
    import {mapMutations} from 'vuex';
    import Card from './Card';
    import CardDraft from './CardDraft';

    import * as types from '../store/mutation-types';

    const List = {
        props: ['title', 'cards', 'index'],
        components: {
            Card,
            CardDraft
        },
        computed: {
            cardCount() {
                return this.cards.length;
            }
        },
        methods: {

            removeList() {
                    this.$store.commit(types.REMOVE_LIST, {
                        listIndex: this.index
                    });
            },
            onDragStart({dataTransfer}) {
                dataTransfer.setData("application/json", JSON.stringify({
                    source: 'list',
                    from: {
                        listIndex: this.index,
                    }
                }));
            },
            onDrop: function ({dataTransfer}) {
                const {source, from} = JSON.parse(dataTransfer.getData("application/json"));

                const to = {
                    listIndex: this.index,
                    cardIndex: 0,
                };
                if (source === 'list') {
                    this.swapLists({from, to});
                } else {
                    this.moveCardToList({from, to});
                }
            },
            ...mapMutations({
                swapLists: types.SWAP_LISTS,
                moveCardToList: types.MOVE_CARD_TO_LIST
            })
        }
    };

    export default List;
</script>

<style lang="scss" scoped>
    .list {
        margin: 0 10px auto;
        position: relative;
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        min-width: 290px;
        width: 290px;
        background-color: #cde3f7;
        border-radius: 8px;
        padding: 15px;

        .close-button {
            position: absolute;
            top: 6px;
            right: 14px;
            font-size: 28px;
            cursor: pointer;
        }

        .close-button:hover {
            opacity: 0.8;
            background-color: #ecd59d;
        }

        .title {
            margin: 5px 0;
            font-size: 24px;
            width: calc(100% - 20px);
            word-break: break-word;
        }

        .cards {
            margin-top: 15px;
            width: 100%;
        }
    }
</style>
