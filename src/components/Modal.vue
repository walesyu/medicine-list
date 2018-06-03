<template>
    <el-dialog :visible.sync="show_me" width="30%">
    <span slot="title">{{title}}</span>
    <span class="el-icon-warning">{{content}}</span>
    <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="close_modal">關閉</el-button>
    </span>
    </el-dialog>
</template>

<script>
    export default {
        name: 'Modal',
        components:{
        },
        props: {
            title: String,
            content: String,
            show_modal: Boolean
        },
        data: function () {
            return {
                show_me: this.showModal
            }
        },
        watch:{
            show_modal:function(newVal, oldVal){
                this.show_me = newVal;
            }
        },
        methods: {
            close_modal: function () {
                this.$emit("child_close_modal", false);
                this.show_me = false;
            }
        }
    }
</script>

<style scoped>

</style>