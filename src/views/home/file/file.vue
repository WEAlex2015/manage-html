<template>

    <div >
        <Upload
            multiple
            action="http://47.106.177.128:16666/file/uploadfile">
            <Button type="ghost" icon="ios-cloud-upload-outline">文档上传</Button>
        </Upload>

        <Table border :columns="columns" :data="data"></Table>

    </div>
</template>

<script>
    export default {
        data () {
            return {
                columns: [
                    {
                        title: '标题',
                        key: 'title',
                    },
                    {
                        title: '发布时间',
                        key: 'time'
                    },
                    {
                        title: '操作',
                        key: 'action',
                        width: 200,
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
                                            this.show(params)
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
        methods: {
            show (param) {
                this.$Modal.success({
                    title: '预览',
                    width: '80%',
                    closable: true,
                    content: param.row.content
                })
            },
            remove (param) {
                let me = this;
                let id = param.row.id;
                // me.$store.dispatch('delNews', {reqData: id});
            },
            initTable () {
                let me = this;
                me.$store.dispatch('getFileCount');
                me.$store.dispatch('getFileList', {reqData: {'page': 1}});
            }
        },
        created() {
            let me = this;
            me.initTable();
        }
    }
</script>

<style lang="less">
 
</style>
