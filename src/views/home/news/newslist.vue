<template>
    <div >
        <Select v-model="type" size="large" style="width:200px" class="type-select" @on-select="listTypeSelect">
            <Option v-for="item in list" :value="item.value" :key="item.value">{{ item.label }}</Option>
        </Select>
    
        <Table border :columns="columns" :data="data"></Table>
        <div style="margin: 10px;overflow: hidden">
        <div style="float: right;">
            <Page :total="pagecount" :current="1" @on-change="changePage"></Page>
        </div>
    </div>
    </div>
</template>

<script>
    import UE from '../../../components/ue/ue.vue'; //引入编辑器
    import { mapGetters } from 'vuex';
    export default {

        components: {
            UE
        },

        computed: mapGetters({
            newsListData: 'newsListData',
            delNewsData: 'delNewsData',
            newsCountData: 'newsCountData',
            
        }),

        data () {
            return {
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
                pagecount: 0,
                columns: [
                    {
                        title: '标题',
                        key: 'title',
                    },
                    {
                        title: '简介',
                        key: 'desc'
                    },
                    {
                        title: '发布时间',
                        key: 'time'
                    },
                    {
                        title: '操作',
                        key: 'action',
                        width: 150,
                        align: 'center',
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'primary',
                                        size: 'small'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.show(params.index)
                                        }
                                    }
                                }, '预览'),
                                h('Button', {
                                    props: {
                                        type: 'error',
                                        size: 'small'
                                    },
                                    on: {
                                        click: () => {
                                            this.remove(params)
                                        }
                                    }
                                }, '删除')
                            ]);
                        }
                    }
                ],
                data: [
                    
                ]
            }
        },

       

        mounted(){

        },

        watch:{
            "newsListData": function (val) {
                let me = this;
                me.data = [];
                if(val && val.data){
                    let list = val.data;
                    list.map(it => {
                        console.info(it);
                        me.data.push(it);
                    });
                }
            },
            "newsCountData": function (val) {
                let me = this;
                if(val && val.data){
                    me.pagecount = val.data;
                }
            },
            // "delNewsData": function (val) {
            //     let me = this;
            //     if(val.data == 1){
            //         me.$Notice.open({
            //             title: '提醒',
            //             desc: '删除新闻成功 '
            //         });
            //         me.$emit("eventFunc", 'back');
            //     }else {
            //         me.$Notice.open({
            //             title: '提醒',
            //             desc: '删除新闻失败 '
            //         });
            //     }
            // },

        },

        methods: {
            show (index) {
                this.$Modal.info({
                    title: 'User Info',
                    content: `标题:${this.data[index].title}<br>简介：${this.data[index].description}<br>时间:${this.data[index].time}`
                })
            },
            remove (param) {
                let me = this;
                let id = param.row.id;
                // me.$store.dispatch('delNews', {reqData: id});
            },
            listTypeSelect (type) {
                let me = this;
                debugger;
                console.info(me.type);
            },
            changePage(index) {
                let me = this;
                me.$store.dispatch('getNewsList', {reqData: {'type': me.type, 'page': index}});
            }
        },

        created() {
            let me = this;
            me.$store.dispatch('getNewsCount', {reqData: me.type});
            me.$store.dispatch('getNewsList', {reqData: {'type': me.type, 'page': 1}});
        }
    }
</script>

<style lang="less">
    .editor-container {
        padding-top: 20px;
    }
</style>
