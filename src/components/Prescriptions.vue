<template>
    <div>
        <el-container>
            <el-header class="presctiption_title">
                {{presctiption_title}}
            </el-header>
        </el-container>
        <el-container>
            <el-aside v-show="show_tools" style="width:390px;">
                <el-card class="box-card">
                    <div slot="header" class="clearfix">
                        <span>藥單設定</span>
                    </div>
                    <el-form ref="form" label-width="100px">
                        <el-form-item label="藥單名稱">
                            <el-input v-model="presctiption_title" placeholder="藥單名稱"></el-input>
                            </el-form-item>
                            <el-form-item label="每一行筆數">
                                <el-select v-model="item_perline" placeholder="每一行筆數">
                                    <el-option v-for="item in 5" :key="item + 1" :label="item + 1" :value="item + 1"></el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="排版方式">
                                <el-select v-model="direction_mode" placeholder="排版方式" >
                                    <el-option :key="0" label="直印" value="0"></el-option>
                                    <el-option :key="1" label="橫印" value="1"></el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="名稱">
                                <el-select v-model="presctiption.material" placeholder="名稱" filterable allow-create no-data-text="No Data">
                                    <el-option v-for="item in medicines" :key="item" :label="item" :value="item"></el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="劑量">
                                <el-input-number v-model="presctiption.dose" :step="1"></el-input-number>
                            </el-form-item>
                            <el-form-item label="單位">
                                <el-select v-model="presctiption.dose_unit" placeholder="每一行筆數">
                                    <el-option v-for="unit in units" :key="unit.id" :label="unit.value" :value="unit.value"></el-option>
                                </el-select>
                            </el-form-item>
                            <el-button-group>
                                <el-button @click="addItem" type="info" class="el-icon-plus">新增</el-button>
                                <el-button @click="print" type="primary" class="el-icon-printer">列印</el-button>
                                <el-button @click="clear" type="danger" class="el-icon-delete">清除藥單</el-button>
                            </el-button-group>
                    </el-form>
                </el-card>
            </el-aside>
            <el-main class="result_area pull-right" :class="(direction_mode=='0')?'direction':''">
                <span v-for="(row, index) in rows" v-bind:key="row.id" class="del_link" v-on:click="del_item(row)">
                    <span>{{row['material']}}</span>
                    <span class='value'>{{row['dose']}}</span>
                    <span class='unit'> {{row['dose_unit']}}</span>&nbsp;&nbsp;
                    <br v-if="index % item_perline == item_perline-1">
                </span>
            </el-main>
            <Modal :title="modal_title" :content="modal_content" :show_modal="show_modal" @child_close_modal="close_modal" />
        </el-container>
    </div>
</template>

<script>
    import Modal from './Modal'

    export default {
        name: 'Prescriptions',
        components: {
            Modal
        },
        data: function () {
            return {
                rows: [],
                presctiption_title: 'OO中藥行用簽',
                direction_mode: "0",
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
                }],
                medicines:["丁香","白芍","紅芍","銹鐵棒","薄荷"]
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
            },
            clear: function(){
                this.presctiption.material = '';
                this.presctiption.dose = 1;
                this.presctiption.dose_unit = '錢';
                this.presctiption.count = 0 ;
                this.rows = [] ;
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
        text-align: center;
        color: red;
        font-family: 標楷體;
        font-size: 20pt;
    }

    .del_link{
        cursor: pointer;
    }

    .pull-right{
        float:right;
    }
</style>