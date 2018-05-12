<template>

    <div >
        <Select v-model="type" size="large" style="width:200px" class="type-select" @on-select="typeSelect">
            <Option v-for="item in list" :value="item.value" :key="item.value">{{ item.label }}</Option>
        </Select>
        <button @click="getUEContent()">获取内容</button>
        <Input v-model="title" size="large" placeholder="标题"></Input>
        <Input v-model="desc" size="large" placeholder="简介"></Input>
        <!-- <Input v-model="type" type="textarea" placeholder="富文本"></Input> -->
        <div class="editor-container">
            <UE :id=curEditor :defaultMsg=defaultMsg :config=config ref="ue"></UE>
        </div>

        <Button type="primary" @click="postNews">发布</Button>
    </div>
</template>

<script>
    import UE from '../../../components/ue/ue.vue'; //引入编辑器
    import { mapGetters } from 'vuex';
    export default {
        components: {
            UE
        },

        data () {
            return {
                curEditor: 'ue',
                defaultMsg: '这里是UE测试',
                config: {
                  initialFrameWidth: null,
                  initialFrameHeight: 350
                },
                list: [
                    {
                        value: 'hangye',
                        label: '行业新闻'
                    },
                    {
                        value: 'qiye',
                        label: '企业新闻'
                    },
                    {
                        value: 'biao',
                        label: '中标公告'
                    },
                    {
                        value: 'notes',
                        label: '通知公告'
                    },
                ],
                type: '',
                title: '',
                content: '',
                desc: '',
                news: {
                    content: '',
                    title: '',
                    type: '',
                    description: ''
                }
            }
        },

        computed: mapGetters({
            newsAddData: 'newsAddData',
            
        }),

        watch:{
            "newsAddData": function (val) {
                let me = this;
                // alert(val);
                if(val.data == 1){
                    me.$Notice.open({
                        title: '提醒',
                        desc: '新增新闻动态成功 '
                    });
                    me.$emit("eventFunc", 'back');
                }else {
                    me.$Notice.open({
                        title: '提醒',
                        desc: '新增新闻动态失败 '
                    });
                }
            }
        },

        mounted(){

        },

        methods: {
            typeSelect (type) {
                console.info(type);
            },
            getUEContent() {
                let me = this;
                me.content = me.$refs.ue.getUEContent();
                // alert(content);
                console.log(me.content)
            },
            postNews() {
                let me = this;
                me.getUEContent();
                console.info(me.content);
                console.info(me.type);
                console.info(me.title);

                me.news = {
                    content: me.content,
                    type: me.type,
                    title: me.title,
                    description: me.desc
                }

                me.$store.dispatch('addNews', {reqData: me.news});

            },
        },

        created() {
            let me = this;
        }
    }
</script>

<style lang="less">
    .type-select {
        position: relative;
        z-index: 2;
    }
    .editor-container {
        position: relative;
        z-index: 1;
        padding-top: 20px;
    }
</style>
