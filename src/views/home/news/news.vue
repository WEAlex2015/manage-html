<template>

    <div >
        新闻动态 
        <Button type="primary" @click="addnews">新增新闻动态</Button>
        <component :is="nowComp" @eventFunc="eventFunc"></component>
    </div>
</template>

<script>
    
    import newsadd from "./newsadd.vue";
    import newsmodify from "./newsmodify.vue";
    import newslist from "./newslist.vue";

    export default {
        components: {
            'newsadd': newsadd,
            'newsmodify': newsmodify,
            'newslist': newslist,
        },

        data () {
            return {
                nowComp: 'newslist'
            }
        },

        computed: {
          
        },

        mounted(){

        },

        methods: {
            addnews() {
                let me = this;
                me.nowComp = 'newsadd'
            },
            eventFunc(type, id, args) {
                let me = this;
                switch (type) {
                    case 'back':
                        me.nowComp = "newslist";
                        break;
                    default:
                        break;
                }
            },
        },

        created() {
            let me = this;
        }
    }
</script>

<style lang="less">
    .editor-container {
        padding-top: 20px;
    }
</style>
