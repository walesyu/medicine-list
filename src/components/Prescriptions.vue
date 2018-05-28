<template>
    <div>
        <div style="width:100%">
            <div class="presctiption_title">{{presctiption_title}}</div>
        </div>
        <div class="row" v-show="show_tools">
            <div class="col-sm-12" >
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
                                    <input class='form-control' type='text' v-model="material"/>
                                </td>
                                <td>
                                    <input class='form-control' type='number' v-model="dose">
                                </td>
                                <td>
                                    <select class='form-control' v-model="dose_unit">
                                        <option  v-for="unit in units"  v-bind:key="unit.id" :value='unit.value'>{{unit.value}}</option>
                                    </select>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </form>
                <form class="form-inline">
                    <div class="form-group">
                        <label>&nbsp;</label>
                        <button type="button" class="btn btn-info" @click="addItem">新增列</button>
                    </div>
                    <div class="form-group">
                        <label>每一行筆數 :</label>
                        <select v-model="item_perline" class="form-control">
                            <option v-for="item in 5" :value="item +1 ">{{item +1}}</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>&nbsp;</label>
                        <button type="button" @click="print" class='btn btn-primary'>印啦印啦</button>
                    </div>
                </form>
            </div>
        </div>
        <div class="row"  style="border: solid 1px black;" >
            <div class="col-sm-11 result_area pull-right" >
                <span :class="(index % item_perline == 0)?' ':''" v-for="(row,index) in rows" v-bind:key="row.id" >
                        {{row['material']}}
                        <span class='value'>{{row['dose']}}</span>
                        <span class='unit'> {{row['dose_unit']}}</span>&nbsp;&nbsp;
                        <br v-if="index % item_perline == item_perline-1">
                </span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Prescriptions',
  data () {
    return {
      rows: [],
      presctiption_title: 'OO中藥行用簽',
      show_tools : true,
      dose : 1,
      item_perline:4,
      material : '',
      dose_unit:'錢',
      units: [
        {
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
    addItem: function(params) {
        if(this.material =="" || this.dose=="" || this.dose_unit==""){
            return false;
        }
        let data = {
            material : this.material,
            dose : this.dose,
            dose_unit : this.dose_unit
        }
        this.rows.push(data);
        this.material = '';
        this.dose = 1;
        this.dose_unit = '錢';
    },
    print: function(){
        setTimeout(function() {
            this.show_tools = true;
            console.log(this.show_tools);
        }, 1000);
        this.show_tools = false;
        
        setTimeout(function() {
             window.print();
        }, 10);
    }
  },mounted:function(){
      setTimeout(function() {
            this.show_tools = false;
             console.log(this.show_tools);
        }, 1000);
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

    .presctiption_title{
        margin: 0 auto;
        width: 220px;
        color: red;
        font-family: 標楷體;
        font-size:20pt;
    }
</style>
