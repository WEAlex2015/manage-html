<template>

    <div >
        <Upload
            multiple
            action="http://localhost:16666/test/postFile">
            <Button type="ghost" icon="ios-cloud-upload-outline">文档上传</Button>
        </Upload>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                
            }
        },
        mounted(){
        },
        methods: {
            
        },
        created() {
            let me = this;
        }
    }
</script>

<style lang="less">
 
</style>
