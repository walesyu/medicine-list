<template>
    <div>
        <div style="width:100%">
            <div class="presctiption_title">{{presctiption_title}}</div>
        </div>
        <div class="card-deck">
            <div class="col-md-5 col-sm-12" v-show="show_tools">
                <div class="card text-white bg-secondary mb-3">
                    <div class="card-header">
                        藥單設定
                    </div>
                    <div class="card-body">
                        <form>
                            <div class="form-group">
                                <label>藥單名稱 :</label>
                                <input type="text" v-model="presctiption_title" class="form-control"/>
                            </div>
                            <div class="form-group">
                                <label>每一行筆數 :</label>
                                <select v-model="item_perline" class="form-control">
                                    <option v-for="item in 5" :value="item +1 " v-bind:key="item">{{item +1}}</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label>排版方式 :</label>
                                <select v-model="direction_mode" class="form-control">
                                    <option value=0 selected>直印</option>
                                    <option value=1>橫印</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <table class="table">
                                    <thead>
                                        <tr>
                                            <th>名稱 </th>
                                            <th>劑量 </th>
                                            <th>單位</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr>
                                            <td>
                                                <input class='form-control' type='text' v-model="presctiption.material" />
                                            </td>
                                            <td>
                                                <input class='form-control' type='number' v-model="presctiption.dose">
                                            </td>
                                            <td>
                                                <select class='form-control' v-model="presctiption.dose_unit">
                                                    <option v-for="unit in units" v-bind:key="unit.id" :value='unit.value'>{{unit.value}}</option>
                                                </select>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                            <div class="btn-group">
                                <button type="button" class="btn btn-info" @click="addItem"><icon name="plus" ></icon>&nbsp;增加這一味藥</button>
                                <button type="button" @click="print" class='btn btn-primary'><icon name="print" ></icon>&nbsp;印啦印啦</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
            <div :class="(show_tools)?'col-md-5 col-sm-12':'col-md-12 col-sm-12'">
                <div class="card">
                    <div class="card-body result_area pull-right" :class="(direction_mode==0)?'direction':''">
                        <span v-for="(row, index) in rows" v-bind:key="row.id">
                            <span>{{row['material']}}</span>
                            <span class='value'>{{row['dose']}}</span>
                            <span class='unit'> {{row['dose_unit']}}</span>
                            <br v-if="index % item_perline == item_perline-1">
                            <a v-show="show_tools" href="#" class="del_link" v-on:click="del_item(row)"><icon name="trash" ></icon></a>&nbsp;&nbsp;
                        </span>
                    </div>
                </div>
            </div>
        </div>
        <Modal :title="modal_title" :content="modal_content" :show_modal="show_modal" @child_close_modal="close_modal" />
    </div>
</template>

<script>
    import Modal from './Modal'
    import "vue-awesome/icons";
    import Icon from "vue-awesome/components/Icon";

    export default {
        name: 'Prescriptions',
        components: {
            Modal,Icon
        },
        data: function () {
            return {
                rows: [],
                presctiption_title: 'OO中藥行用簽',
                direction_mode: 0,
                modal_title: '注意',
                modal_content: '',
                show_tools: true,
                show_modal: false,
                item_perline: 4,
                presctiption:{
                    material: '',
                    dose_unit: '錢',
                    dose: 1,
                    count: 0
                },
                units: [{
                    id: 1,
                    value: '錢'
                },
                {
                    id: 2,
                    value: '兩'
                },
                {
                    id: 3,
                    value: '斤'
                },
                {
                    id: 4,
                    value: '片'
                },
                {
                    id: 5,
                    value: '枚'
                }]
            };
        },
        methods: {
            addItem: function (params) {
                if (this.presctiption.material == "" || this.presctiption.dose == "" || this.presctiption.dose_unit == "") {
                    this.show_modal = true;
                    this.modal_content = '沒有填入藥材名稱！';
                    return false;
                }
                let data = {
                    material: this.presctiption.material,
                    dose: this.presctiption.dose,
                    dose_unit: this.presctiption.dose_unit,
                    id: this.presctiption.count
                }
                this.rows.push(data);
                this.presctiption.material = '';
                this.presctiption.dose = 1;
                this.presctiption.dose_unit = '錢';
                this.presctiption.count++;
            },
            print: function () {
                let that = this;
                this.show_tools = false;
                setTimeout(function () {
                    window.print();
                }, 10);
                setTimeout(function () {
                    that.show_tools = true;
                }, 1000);
            },
            close_modal: function () {
                this.show_modal = false;
            },
            del_item: function(item){
                var rows = this.rows;
                var confirm_del = confirm("是否確定要刪除" + item.material + "?");
                if (confirm_del) {
                    for (var i = 0; i < rows.length; i++) {
                        if (item.id == rows[i]["id"]) {
                            rows.splice(i, 1);
                            break;
                        }
                    }
                }
            }
        }
    }
</script>

<style scoped>
    .direction {
        writing-mode: tb-rl;
    }

    .result_area {
        font-size: 18pt;
        font-family: 標楷體;
        margin: 10px;
    }

    .unit {
        position: relative;
        left: 10px;
        font-size: 12pt;
        font-family: 標楷體;
    }

    .value {
        position: relative;
        left: 10px;
        font-size: 12pt;
        font-family: 細明體;
        writing-mode: lr;
    }

    #div_title {
        font-size: 18pt;
        color: Red;
        display: none;
    }

    .presctiption_title {
        margin: 0 auto;
        width: 220px;
        color: red;
        font-family: 標楷體;
        font-size: 20pt;
    }

    .del_link{
        color:red;
    }
</style>