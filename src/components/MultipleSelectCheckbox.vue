<template>
  <el-select v-model="selectValues" :popper-class="'multiple_select_popper_' + order" v-bind="$attrs" v-on="$listeners" multiple
             :class="'multiple_select_checkbox_' + order" @visible-change="visibleChange" @change="changeSelect">
    <el-option v-if="options_.length" label="全选" value="全选">
      <el-checkbox v-model="isSelectAll" @click.native.prevent>全选</el-checkbox>
    </el-option>
    <el-option v-for="item in options_" :key="item[props.value]" :label="item[props.label]" :value="item[props.value]">
      <el-tooltip :disabled="!item.isExceed" :content="item[props.label]" placement="top" effect="light">
        <el-checkbox :value="selectValues.includes(item[props.value])" @click.native.prevent>{{item[props.label]}}</el-checkbox>
      </el-tooltip>
    </el-option>
  </el-select>
</template>

<script>
export default {
  name: "MultipleSelectCheckbox",
  inheritAttrs: false,// 似乎设不设置都可以
  props: {
    value: {
      type: Array,
      default: () => []
    },
    // 下拉选项
    options: {
      type: Array,
      default: () => []
    },
    // 选项键值对
    props: {
      type: Object,
      default: () => {
        return {
          label: "label",
          value: "value"
        }
      }
    },
    // 选项是否独占一行
    isInline: {
      type: Boolean,
      default: false
    },
    // 选项的宽度
    textWidth: {
      type: Number,
    },
    // 组件唯一标识 （避免同一个页面引用多次，发生耦合）
    order: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      selectValues: this.value,
      isSelectAll: false,
      multipleSelectCheckboxMaxWidth: 0,
      options_: []
    }
  },
  mounted() {
    const multipleSelectCheckbox = document.querySelector(`.multiple_select_checkbox_${this.order}.el-select`)
    const elSelectDropdown = document.querySelector(`.multiple_select_popper_${this.order}.el-select-dropdown`)
    const multipleSelectCheckboxMaxWidth = multipleSelectCheckbox.scrollWidth
    this.multipleSelectCheckboxMaxWidth = multipleSelectCheckboxMaxWidth
    elSelectDropdown.style["max-width"] = multipleSelectCheckboxMaxWidth + "px"
  },
  watch: {
    // 监听（全选or全不选）
    value: {
      handler(arr) {
        this.selectValues = arr
        this.isSelectAll = arr.length === this.options_.length
      }
    },
    options: {
      handler(arr) {
        this.options_ = JSON.parse(JSON.stringify(arr))
        this.isSelectAll = this.selectValues.length === this.options_.length
      },
      immediate: true
    }
  },
  methods: {
    changeSelect(val) {
      if (val.includes("全选")) {
        // 说明已经全选了，所以全不选
        if (val.length > this.options_.length) {
          this.selectValues = []
        }
        // 反之，全选
        else {
          this.selectValues = this.options_.map(item => item[this.props.value])
        }
      }
      this.$emit("input", this.selectValues)
    },
    // 判断元素是否溢出，溢出加上toolTip
    async visibleChange(visible) {
      await this.$nextTick()
      if (visible) {
        const maxLableWidth = this.multipleSelectCheckboxMaxWidth - 80
        const labels = document.querySelectorAll(`.multiple_select_popper_${this.order}.el-select-dropdown .el-checkbox.el-tooltip .el-checkbox__label`)
        labels.forEach((label, index) => {
          // eslint-disable-next-line no-prototype-builtins
          if (!this.options_[index].hasOwnProperty("isExceed")) {
            this.$set(this.options_[index], "isExceed", label.scrollWidth - 10 > (this.textWidth || maxLableWidth))
            label.style["width"] = this.textWidth + "px"
            label.style["max-width"] = (this.textWidth || maxLableWidth) + "px"
            label.style["vertical-align"] = "middle"
            label.style["overflow"] = "hidden"
            label.style["text-overflow"] = "ellipsis"
            label.style["white-space"] = "nowrap"
          }
        })
        const items = document.querySelectorAll(`.multiple_select_popper_${this.order} .el-select-dropdown__item:not(:first-child)`)
        items.forEach(item => {
          item.style.display = this.isInline ? "inline-block" : "block"
        })
      }
    }
  }
}
</script>

<style scoped>
[class*="multiple_select_popper"].el-select-dropdown.is-multiple .el-select-dropdown__item.selected::after {
  content: "";
}
</style>