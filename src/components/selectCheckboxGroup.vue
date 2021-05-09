<template>
  <div>
    <el-popover :width="popoverWidth" trigger="click">
      <el-checkbox :indeterminate="isIndeterminate" v-model="checkAll" @change="handleCheckAllChange">
        全选
      </el-checkbox>
      <div style="margin: 15px 0;"></div>
      <el-checkbox-group v-model="selectCheckboxGroupValue" @change="handleCheckedChange">
        <el-checkbox v-for="item in list" :label="item.value" :key="item.value" :style="`width: ${checkboxWidth}px`">
          {{item.label}}
        </el-checkbox>
      </el-checkbox-group>
      <el-select slot="reference" v-model="selectCheckboxGroupValue" multiple collapse-tags placeholder="请选择">
        <el-option hidden class="el-option" v-for="item in list" :key="item.value" :label="item.label" :value="item.value">
        </el-option>
      </el-select>
    </el-popover>
  </div>
</template>
<script>

export default {
  name: 'select_checkbox_group',
  props: {
    value: {
      type: Array,
      default() {
        return []
      }
    },
    list: {
      type: Array,
      default() {
        return []
      }
    },
    popoverWidth: {
      type: Number,
      default: 570
    },
    checkboxWidth: {
      type: Number,
      default: 50
    },
  },
  data() {
    return {
      selectCheckboxGroupValue: this.value,
      checkAll: this.value.length === this.list.length,
      isIndeterminate: this.value.length > 0 && this.value.length < this.list.length,
    }
  },
  watch: {
    selectCheckboxGroupValue: {
      handler: function(n) {
        this.$emit('input', n);
      }
    }
  },
  methods: {
    //全选
    handleCheckAllChange(val) {
      if (val) {
        let arr = []
        this.list.forEach(item => {
          arr.push(item.value)
        })
        this.selectCheckboxGroupValue = arr
      } else {
        this.selectCheckboxGroupValue = []
      }
      this.isIndeterminate = false;
    },

    // 一个个选
    handleCheckedChange(value) {
      let checkedCount = value.length;
      this.checkAll = checkedCount === this.list.length;
      this.isIndeterminate = checkedCount > 0 && checkedCount < this.list.length;
    },
  },
}
</script>
<style scoped>
/* .el-option {
  display: none;
} */
</style>

