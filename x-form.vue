<!--
config 格式字段说明：

  {
    params: [
      {
        type: "input",  // input 输入框；date 日期选择器；select 选择器；为空不显示
        param: "name",  // 提交表单时，对应字段
        label: "姓名",  // 提示标题，默认为空，可为空
        defValue: "",   // 默认值，默认为空，可为空
        placeholder: "请输入姓名",  // 默认提示内容，默认为空，可为空
        options: [  //  type为 select 时，需要传入对应选择内容，label 显示文字；value 对应值
          {label: "男", value: "1"},
          {label: "女", value: "0"}
        ],
      }
    ],
    defData: {},  // 默认表单数据，默认为空，可为空，不为空时会替换掉 formModel 中配置 defValue 字段值
    onSubmit(obj) // 点击确认执行函数
    onCancel(obj) // 点击取消执行函数
  }
-->
<template>
  <el-form v-if="submitForm" :model="submitForm" label-width="80px">

    <template v-for="cols in config.params">
      <el-row>
        <template v-for="model in cols">
          <el-col :span="24 / cols.length">
            <!-- input -->
            <template v-if="model.type == 'input'">
              <el-form-item :label="model.label">
                <el-input v-model="submitForm[model.param]" :placeholder="model.placeholder"></el-input>
              </el-form-item>
            </template>

            <!-- date -->
            <template v-else-if="model.type == 'date'">
              <el-form-item :label="model.label">
                <el-date-picker type="date" v-model="submitForm[model.param]" @change="datePickerFmt(model.param)" :placeholder="model.placeholder||'选择日期'" style="width: 100%"></el-date-picker>
                <!--:picker-options="pickerOptions">-->
              </el-form-item>
            </template>

            <!-- select -->
            <template v-else-if="model.type == 'select'">
              <el-form-item :label="model.label">
                <el-select v-model="submitForm[model.param]" :placeholder="model.placeholder||'请选择'" style="width: 100%">
                  <div v-for="option in model.options">
                    <el-option :label="option.label" :value="option.value"></el-option>
                  </div>
                </el-select>
              </el-form-item>
            </template>
          </el-col>
        </template>
      </el-row>
    </template>


    <div style="text-align: center">
      <el-button @click="config.onCancel">取消</el-button>

      <el-button type="primary" @click="config.onSubmit(submitForm)">确定</el-button>
    </div>

  </el-form>
</template>

<script>

  export default {
    methods: {
      datePickerFmt(param) {
        if (typeof(this.submitForm[param]) != "number") {
          this.submitForm[param] = Date.parse(this.submitForm[param]);
        }
      }
    },
    data() {
      return {
        submitForm: this.config.data
      }
    },
    props: {
      config: {
        type: Object,
        default() {
          return {}
        }
      }
    }
  }

</script>