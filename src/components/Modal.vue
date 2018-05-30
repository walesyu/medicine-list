<template>
    <transition name="modal" v-if="show_me">
        <div class="modal" style="display: block" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h4 class="modal-title">
                            {{title}}
                        </h4>
                    </div>
                    <div class="modal-body">
                        <icon name="user-md" scale="2"/>&nbsp;
                        {{content}}
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-outline-info" @click="close_modal"> 關閉 </button>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
    import "vue-awesome/icons";
    import Icon from "vue-awesome/components/Icon";
    export default {
        name: 'Modal',
        components:{
            Icon
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