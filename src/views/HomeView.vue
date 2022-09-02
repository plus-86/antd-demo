<template>
  <div>
    <a-table :columns="columns" :data-source="data">
      <template slot="operation" slot-scope="text, record, index, column">
        <a @click="showModal(text, record, index, column)">编辑</a>
      </template>
    </a-table>
    <a-modal v-model="visible" title="Basic Modal" @ok="handleOk">
      <a-form>
        <a-form-item label="字段名称">
          <a-input v-model="name" />
        </a-form-item>
        <a-form-item label="字段类型">
          <a-select @change="clearOption" v-model="type">
            <a-select-option value="单行文本"> 单行文本 </a-select-option>
            <a-select-option value="日期"> 日期 </a-select-option>
            <a-select-option value="单选下拉"> 单选下拉 </a-select-option>
          </a-select>
        </a-form-item>
        <a-form-item label="字段选项">
          <a-select v-if="type == '单行文本'" v-model="option">
            <a-select-option value="无"> 无 </a-select-option>
          </a-select>
          <a-radio-group v-if="type == '日期'" v-model="option">
            <a-radio value="年"> 年 </a-radio>
            <a-radio value="年-月"> 年-月 </a-radio>
            <a-radio value="年-月-日"> 年-月-日 </a-radio>
            <a-radio value="年-月-日 时：分"> 年-月-日 时：分 </a-radio>
          </a-radio-group>
          <a-select
            v-if="type == '单选下拉'"
            mode="multiple"
            v-model="option"
            style="width: 100%"
          >
            <a-select-option
              v-for="i in 25"
              :value="'选项' + i"
              :key="(i + 9).toString(36) + i"
            >
              {{ '选项' + i }}
            </a-select-option>
          </a-select>
        </a-form-item>
        <a-form-item required name="rd" label="是否必填">
          <a-radio-group
            v-model="isRequired"
            v-decorator="[
              'rd',
              { rules: [{ required: true, message: 'Please input your name' }] }
            ]"
          >
            <a-radio value="是"> 是 </a-radio>
            <a-radio value="否"> 否 </a-radio>
          </a-radio-group>
        </a-form-item>
      </a-form>
    </a-modal>
  </div>
</template>
<script>
const columns = [
  {
    title: '字段名称',
    dataIndex: 'name',
    key: 'name'
  },
  {
    title: '字段类型',
    dataIndex: 'type',
    key: 'type'
  },
  {
    title: '是否必填',
    dataIndex: 'isRequired',
    key: 'isRequired'
  },
  {
    title: '字段选项',
    key: 'options',
    dataIndex: 'options'
  },
  {
    title: '操作',
    key: 'operation',
    dataIndex: 'operation',
    scopedSlots: { customRender: 'operation' }
  }
]

const data = [
  {
    key: '1',
    name: '字段一',
    type: '单行文本',
    isRequired: '是',
    options: '无'
  },
  {
    key: '2',
    name: '字段二',
    type: '日期',
    isRequired: '否',
    options: '年-月-日'
  },
  {
    key: '3',
    name: '字段三',
    type: '单选下拉',
    isRequired: '是',
    options: ['选项1', '选项2']
  }
]

export default {
  data() {
    return {
      index: '',
      option: '',
      isRequired: '',
      name: '',
      type: '',
      visible: false,
      data,
      columns
    }
  },
  methods: {
    clearOption() {
      this.option = ''
      this.isRequired = ''
      if (this.type == '单选下拉') this.option = ['选项1', '选项2']
    },
    showModal(text, s, index, column) {
      this.index = index
      this.visible = true
      this.name = s.name
      this.type = s.type
      this.option = s.options
      this.isRequired = s.isRequired
    },
    handleOk() {
      if (!this.isRequired) return this.$message.warning('请选择是否必填项')
      this.visible = false
      let data = this.data[this.index]
      data.name = this.name
      data.type = this.type
      data.options = this.option
      data.isRequired = this.isRequired
    }
  }
}
</script>
