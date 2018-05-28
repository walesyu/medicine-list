<template>
    <div>
        <div style="width:100%">
            <div id="div_title" style="margin: 0 auto;width: 120px;">{presctiption_title}</div>
        </div>
        <div class="row">
            <div class="col-sm-12" id="tool_area">
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
                <div class="row" style="border: solid 1px black;">
                    <div class="col-sm-12 pull-right" id="result_area">
                        <span v-for="row in rows" v-bind:key="row.id">
                            {{row['material']}}
                            <span class='value'>{{row['dose']}}</span>
                            <span class='unit'> {{row['dose_unit']}}</span>
                       </span>
                    </div>
                </div>
                <button class="btn btn-info" @click="addItem()">新增列</button>
                <button id='btn_print' class='btn btn-primary'>印啦印啦</button>
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
      material : '',
      dose : 0,
      material : '',
      dose_unit:'',
      units: [
        {
          id: 1,
          value: '兩'
        },
        {
          id: 2,
          value: '錢'
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
        let data = {
            material : this.material,
            dose : this.dose,
            dose_unit : this.dose_unit
        }
        this.rows.push(data);
        console.log(this.rows);
    }
  }
}
</script>

<style scoped>
#result_area {
  writing-mode: tb-rl;
}

.unit {
  position: relative;
  left: 10px;
}

.value {
  position: relative;
  left: 10px;
  writing-mode: lr;
}

#div_title {
  font-size: 18pt;
  color: Red;
  display: none;
}
</style>
