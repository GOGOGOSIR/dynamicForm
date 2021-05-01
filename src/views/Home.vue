/* eslint-disable no-useless-escape */
<template>
  <div class="home">
    <el-form
      class="form"
      ref="form"
      :model="form"
      :rules="rules"
      label-width="80px"
      :validate-on-rule-change="false"
    >
      <el-form-item label="活动名称" prop="name">
        <el-input v-model="form.name"></el-input>
      </el-form-item>
      <el-form-item label="时间">
        <div class="flex-wrapper">
          <el-form-item prop="dateStart">
            <el-date-picker
              v-model="form.dateStart"
              type="date"
              placeholder="选择开始日期"
            >
            </el-date-picker>
          </el-form-item>
          <div style="margin: 0 10px">至</div>
          <el-form-item prop="dateStart">
            <el-date-picker
              v-model="form.dateEnd"
              type="date"
              placeholder="选择结束日期"
            >
            </el-date-picker>
          </el-form-item>
        </div>
      </el-form-item>
      <!-- 动态渲染 -->
      <template v-for="(item, index) in dynamicList">
        <el-form-item
          :key="index"
          v-if="item.type === 'TEXT'"
          :label="item.name"
          :prop="item.prop"
        >
          <div class="flex-wrapper">
            <el-input v-model="form[item.prop]"></el-input>
            <span style="margin-left: 10px">{{ item.suffix }}</span>
          </div>
          <p>{{ item.remark }}</p>
        </el-form-item>
        <el-form-item
          :key="index"
          v-if="item.type === 'SWITCH'"
          :label="item.name"
          :prop="item.prop"
        >
          <div class="flex-wrapper">
            <el-switch v-model="form[item.prop]"> </el-switch>
            <span style="margin-left: 10px">{{ item.suffix }}</span>
          </div>
          <p>{{ item.remark }}</p>
        </el-form-item>
        <el-form-item
          :key="index"
          v-if="item.type === 'RADIO'"
          :label="item.name"
          :prop="item.prop"
        >
          <div class="flex-wrapper">
            <el-radio-group v-model="form[item.prop]">
              <el-radio
                v-for="(optionItem, optionIndex) in item.options"
                :key="optionIndex"
                :label="optionItem.value"
                >{{ optionItem.label }}</el-radio
              >
            </el-radio-group>
            <span style="margin-left: 10px">{{ item.suffix }}</span>
          </div>
          <p>{{ item.remark }}</p>
        </el-form-item>
        <el-form-item
          :key="index"
          v-if="item.type === 'CHECKBOX'"
          :label="item.name"
          :prop="item.prop"
        >
          <div class="flex-wrapper">
            <el-checkbox-group v-model="form[item.prop]">
              <el-checkbox
                v-for="(optionItem, optionIndex) in item.options"
                :key="optionIndex"
                :label="optionItem.value"
                >{{ optionItem.label }}</el-checkbox
              >
            </el-checkbox-group>
            <span style="margin-left: 10px">{{ item.suffix }}</span>
          </div>
          <p>{{ item.remark }}</p>
        </el-form-item>
        <el-form-item
          :key="index"
          v-if="item.type === 'RANGE'"
          :label="item.name"
        >
          <div class="flex-wrapper">
            <div class="flex-wrapper">
              <el-form-item :prop="`${item.prop}RangeMin`">
                <el-input v-model="form[`${item.prop}RangeMin`]"></el-input>
              </el-form-item>
              <div style="margin: 0 10px">-</div>
              <el-form-item :prop="`${item.prop}RangeMax`">
                <el-input v-model="form[`${item.prop}RangeMax`]"></el-input>
              </el-form-item>
            </div>
            <span style="margin-left: 10px">{{ item.suffix }}</span>
          </div>
          <p>{{ item.remark }}</p>
        </el-form-item>
      </template>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">立即创建</el-button>
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>

const dynamicList = [{
  type: 'TEXT',
  name: 'input框',
  prop: 'age',
  value: '123',
  expression: JSON.stringify({
    reg: '^\\d+$',
    errMsg: '请输入数字'
  }),
  suffix: '个',
  remark: '备注'
}, {
  type: 'SWITCH',
  name: '开关',
  prop: 'switch',
  value: 'true',
  expression: ``,
  suffix: '个',
  remark: '备注'
}, {
  type: 'CHECKBOX',
  name: '多选框',
  prop: 'checkbox',
  value: '1#2',
  expression: JSON.stringify([{
    "label": "香蕉",
    "value": 1 // 这里是string
  }, {
    label: 'apple',
    value: 2 // 这里是string
  }, {
    label: '橙子',
    value: 3 // 这里是string
  }]),
  suffix: '个',
  remark: '备注'
}, {
  type: 'RADIO',
  name: '单选',
  prop: 'radio',
  value: 'man',
  expression: JSON.stringify([{
    label: '男',
    value: 'man' // 这里是string
  }, {
    label: '女',
    value: 'woman' // 这里是string
  }]),
  suffix: '人',
  remark: '备注'
}, {
  type: 'RANGE',
  name: '区间',
  prop: 'range',
  value: '100#200',
  expression: JSON.stringify({
    min: {
      reg: '^\\d+$',
      errMsg: '请输入数字'
    },
    max: {
      reg: '^\\d+$',
      errMsg: '请输入数字'
    }
  }),
  suffix: '元',
  remark: '备注'
}, {
  type: 'RANGE',
  name: '区间2',
  prop: 'range2',
  value: '',
  expression: JSON.stringify({
    min: {
      reg: '^\\d+$',
      errMsg: '请输入数字'
    },
    max: {
      reg: '^\\d+$',
      errMsg: '请输入数字'
    }
  }),
  suffix: '元',
  remark: '备注'
}]

export default {
  name: 'Home',
  data () {
    return {
      dynamicList: [],
      form: {
        name: '',
        dateStart: new Date(),
        dateEnd: ''
      },
      rules: {
        name: [
          { required: true, message: '请输入活动名称', trigger: 'blur' },
          {
            validator: (rule, value, callback) => {
              const reg = /^\d+$/
              if (reg.test(value)) {
                callback()
              } else {
                callback('请输入数字')
              }
            }, trigger: 'blur'
          }
        ],
        dateStart: [{
          validator: (rule, value, callback) => {
            this.validatorDate(rule, value, callback, 'dateEnd')
          },
          trigger: 'change'
        }],
        dateEnd: [{
          validator: (rule, value, callback) => {
            this.validatorDate(rule, value, callback, 'dateStart')
          },
          trigger: 'change'
        }]
      }
    }
  },
  methods: {
    validatorDate (rule, value, callback, type) {
      if (value && this.form[type]) {
        const targetTime = value.getTime()
        const compareTime = this.form[type].getTime()
        if (type === 'dateEnd') {
          if (targetTime > compareTime) {
            callback('开始时间不能大于结束时间')
          } else {
            callback()
          }
        } else {
          if (targetTime < compareTime) {
            callback('结束时间不能小于开始时间')
          } else {
            callback()
          }
        }
      } else {
        callback()
      }
    },
    // 动态创建form
    createDynamicForm () {
      setTimeout(() => {
        dynamicList.forEach((item) => {
          const type = item.type.trim()
          const expression = item.expression.trim()
          const value = item.value.trim()
          const prop = item.prop.trim()
          // 设置单选框和多选框的options
          if (['RADIO', 'CHECKBOX'].includes(type)) {
            if (expression) {
              const expressionObj = JSON.parse(expression)
              if (Array.isArray(expressionObj)) {
                item.options = expressionObj.map(i => ({ label: i.label, value: i.value.toString() }))
              } else {
                item.options = []
              }
            } else {
              item.options = []
            }
          }
          // 设置form
          let effectValue = ''
          if (type === 'CHECKBOX') {
            if (value) {
              effectValue = value.split('#').map(item => item.toString())
            } else {
              effectValue = []
            }
          } else if (type === 'SWITCH') {
            effectValue = value === 'true' ? true : false
          } else if (type === 'RANGE') {
            const strList = value.split('#').map(item => item.toString())
            this.$set(this.form, `${prop}RangeMin`, strList[0] || '')
            this.$set(this.form, `${prop}RangeMax`, strList[1] || '')
          } else {
            effectValue = value || ''
          }
          this.$set(this.form, prop, effectValue)
          // 设置rule
          if (['TEXT', 'RANGE'].includes(type)) {
            if (expression) {
              const expressionObj = JSON.parse(expression)
              if (Object.prototype.toString.call(expressionObj) === '[object Object]') {
                if (type === 'TEXT') {
                  const regStr = expressionObj.reg
                  const errMsg = expressionObj.errMsg
                  this.setRule(prop, regStr, errMsg)
                } else {
                  const minObj = expressionObj.min
                  const maxObj = expressionObj.max
                  const minRegStr = minObj.reg
                  const minErrMsg = minObj.errMsg
                  const maxRegStr = maxObj.reg
                  const maxErrMsg = maxObj.errMsg
                  this.setRule(`${prop}RangeMin`, minRegStr, minErrMsg)
                  this.setRule(`${prop}RangeMax`, maxRegStr, maxErrMsg)
                }
              }
            }
            // 动态设置了rule会触发其校验 可用 validate-on-rule-change 替换
            // this.$nextTick(() => {
            //   this.$refs.form.clearValidate()
            // })
          }
        })
        this.dynamicList = dynamicList
      }, 200)
      console.log(this.rules)
    },
    setRule (prop, regStr, errMsg) {
      if (regStr && errMsg) {
        const reg = new RegExp(regStr)
        this.$set(this.rules, prop, [{
          validator: (rule, value, callback) => {
            if (!value || reg.test(value)) {
              callback()
            } else {
              callback(errMsg)
            }
          }, trigger: 'blur'
        }])
      }
    },
    getCallBackParams () {
      const params = {}
      for (const item of this.dynamicList) {
        const type = item.type.trim()
        const prop = item.prop.trim()
        if (type === 'RANGE') {
          const minValue = this.form[`${prop}RangeMin`] || ''
          const maxValue = this.form[`${prop}RangeMax`] || ''
          params[prop] = `${minValue}#${maxValue}`
        } else {
          params[prop] = this.form[prop]
        }
      }
      return params
    },
    onSubmit () {
      console.log(this.form, '==form==')
      console.log(this.getCallBackParams(), '回调函数的参数')
      this.$refs.form.validate((valid) => {
        if (valid) {
          alert('submit!');
        } else {
          console.log('error submit!!');
          return false;
        }
      })
    }
  },
  mounted () {
    this.createDynamicForm()
  }
}
</script>

<style lang="scss" scoped>
.form {
  width: 600px;
  margin: 60px auto;
  .flex-wrapper {
    width: 100%;
    display: flex;
    align-items: center;
    line-height: 1;
    min-height: 40px;
  }
}
</style>
