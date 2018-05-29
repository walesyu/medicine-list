<template>
    <transition name="modal" v-if="show_modal">
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
                        <button type="button" class="btn btn-outline-info" @click="closeModal"> 關閉 </button>
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
        props: ["title", "content", "showModal"],
        data: function () {
            return {
                show_modal: this.showModal
            }
        },
        watch:{
            showModal:function(newVal, oldVal){
                this.show_modal = newVal;
            }
        },
        methods: {
            closeModal: function () {
                this.$emit("childCloseModal", false);
                this.show_modal = false;
            }
        }
    }
</script>

<style scoped>

</style>