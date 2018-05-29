<template>
    <div>
        <div style="width:100%">
            <div class="presctiption_title">{{presctiption_title}}</div>
        </div>
        <div class="row" v-show="show_tools">
            <div class="col-sm-12">
                <form>
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
                                    <input class='form-control' type='text' v-model="material" />
                                </td>
                                <td>
                                    <input class='form-control' type='number' v-model="dose">
                                </td>
                                <td>
                                    <select class='form-control' v-model="dose_unit">
                                        <option v-for="unit in units" v-bind:key="unit.id" :value='unit.value'>{{unit.value}}</option>
                                    </select>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </form>
                <form class="form-inline">
                    <div class="form-group">
                        <label>&nbsp;</label>
                        <button type="button" class="btn btn-info" @click="addItem">增加這一味藥</button>
                    </div>
                    <div class="form-group">
                        <label>藥單名稱 :</label>
                        <input type="text" v-model="presctiption_title" />
                        <label>每一行筆數 :</label>
                        <select v-model="item_perline" class="form-control">
                            <option v-for="item in 5" :value="item +1 " v-bind:key="item">{{item +1}}</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <button type="button" @click="print" class='btn btn-primary'>印啦印啦</button>
                    </div>
                </form>
            </div>
        </div>
        <div class="row">
            <div class="col-sm-11 result_area pull-right">
                <span v-for="(row,index) in rows" v-bind:key="row.id">
                    <span>{{row['material']}}</span>
                    <span class='value'>{{row['dose']}}</span>
                    <span class='unit'> {{row['dose_unit']}}</span>&nbsp;&nbsp;
                    <br v-if="index % item_perline == item_perline-1">
                </span>
            </div>
        </div>
        <Modal :title="modal_title" :content="modal_content" :showModal="showModal" @childCloseModal="close_modal" />
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
                modal_title: '注意',
                modal_content: '',
                show_tools: true,
                showModal: false,
                dose: 1,
                item_perline: 4,
                material: '',
                dose_unit: '錢',
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
                }
                ]
            };
        },
        methods: {
            addItem: function (params) {
                if (this.material == "" || this.dose == "" || this.dose_unit == "") {
                    this.showModal = true;
                    this.modal_content = '沒有填入藥材名稱！';
                    return false;
                }

                let data = {
                    material: this.material,
                    dose: this.dose,
                    dose_unit: this.dose_unit
                }
                this.rows.push(data);
                this.material = '';
                this.dose = 1;
                this.dose_unit = '錢';
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
                this.showModal = false;
            }
        }
    }
</script>

<style scoped>
    .result_area {
        writing-mode: tb-rl;
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
        writing-mode: lr;
        font-size: 12pt;
        font-family: 細明體;
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
</style>