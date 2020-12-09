<template>
    <div class="app">
        <header>
            <a href="#">
                <h1 class="logo">Kanban board</h1>
            </a>
        </header>
        <main>
            <list v-for="({ title, cards }, index) in lists" :title="title" :cards="cards" :index="index" :key="index"></list>
            <list-draft></list-draft>
        </main>
        <card-details v-show="isCardDetailsVisible" @close="closeDetails" :card="activeCard"/>
    </div>
</template>

<script>
    import List from './List';
    import ListDraft from './ListDraft';

    import CardDetails from './CardDetails';

    import {mapState} from 'vuex';

    const App = {
        components: {
            List,
            ListDraft,
            CardDetails
        },
        computed: {
            ...mapState([
                'lists'
            ])
        },
        mounted() {
          this.$root.$on('showCardDetails', ({listIndex, cardIndex}) => {
              this.activeCard = this.lists[listIndex].cards[cardIndex];
              this.isCardDetailsVisible = true;
          })
        },
        data() {
            return {
                activeCard: {},
                isCardDetailsVisible: false,
                message: 'hello, world!',
            }
        },
        methods: {
            closeDetails() {
                this.isCardDetailsVisible = false;
            },
        },
    };

    export default App;
</script>

<style>
    html, body {
        width: 100%;
        height: 100%;
    }

    body {
        background: rgb(246,170,99);
        background: linear-gradient(171deg, rgba(246,170,99,1) 53%, rgba(217,224,39,1) 100%);
    }

    h1, h2, h3, h4 {
        margin: 0;
    }

    @font-face {
        font-family: 'Noto Sans Japanese';
        font-style: normal;
        font-weight: 400;
        src: url(https://fonts.gstatic.com/ea/notosansjapanese/v6/NotoSansJP-Bold.woff2) format('woff2'),
        url(https://fonts.gstatic.com/ea/notosansjapanese/v6/NotoSansJP-Bold.woff) format('woff'),
        url(https://fonts.gstatic.com/ea/notosansjapanese/v6/NotoSansJP-Bold.otf) format('opentype');
    }
</style>

<style lang="scss" scoped>
    .app {
        font-family: "Noto Sans Japanese", "Noto Sans", sans-serif;
        font-weight: 700;
        color: #242424;
        width: 100%;
        height: 100%;

        header {
            padding: 0 20px;
            display: flex;
            align-items: center;
            width: calc(100% - 40px);
            height: 100px;

            a {
                text-decoration: none;

                h1.logo {
                    font-family: 'Tahoma', cursive;
                    font-weight: normal;
                    font-size: 48px;
                    color: rgb(255, 220, 173);
                }
            }

            a:hover {
                h1.logo {
                    opacity: 0.7;
                }
            }
        }

        main {
            padding: 0 20px;
            display: flex;
            overflow-x: scroll;
            width: calc(100% - 40px);
            height: calc(100% - 100px);
        }
    }
</style>

