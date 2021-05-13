<template>
  <el-select v-model="selectValues" v-bind="$attrs" multiple class="multiple_select_checkbox" :class="isSetInlineBlock" :style="styleText"
             @visible-change="visibleChange" @change="changeSelect">
    <el-option v-if="options.length" label="全选" value="全选">
      <el-checkbox v-model="isSelectAll" @click.native.prevent>全选</el-checkbox>
    </el-option>
    <el-option v-for="item in options" :key="item[props.value]" :label="item[props.label]" :value="item[props.value]">
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
    }
  },
  data() {
    return {
      selectValues: [],
      isSelectAll: false,
      styleText: {
        "--text-width": this.textWidth + "px",
        "--text-max-width": "100px",
      }
    }
  },
  computed: {
    isSetInlineBlock() {
      return {
        "inline": this.isInline
      }
    }
  },
  mounted() {
    const elSelectDropdown = this.$el.querySelector(".el-select-dropdown")
    const multipleSelectCheckboxMaxWidth = this.$el.scrollWidth
    elSelectDropdown.style["max-width"] = multipleSelectCheckboxMaxWidth + "px"
    this.styleText["--text-max-width"] = multipleSelectCheckboxMaxWidth - 80 + "px"
  },
  watch: {
    // 监听（全选or全不选）
    value: {
      handler(arr) {
        this.isSelectAll = arr.length === this.options.length
        this.selectValues = arr
      }
    }
  },
  methods: {
    changeSelect(val) {
      // eslint-disable-next-line no-debugger
      debugger
      if (val.includes("全选")) {
        // 说明已经全选了，所以全不选
        if (val.length > this.options.length) {
          this.selectValues = []
        }
        // 反之，全选
        else {
          this.selectValues = this.options.map(item => item[this.props.value])
        }
      }
      this.$emit("input", this.selectValues)
    },
    // 判断元素是否溢出，溢出加上toolTip
    async visibleChange(visible) {
      await this.$nextTick()
      if (visible) {
        const els = this.$el.querySelectorAll(".el-select-dropdown .el-checkbox.el-tooltip .el-checkbox__label")
        els.forEach((el, index) => {
          this.$set(this.options[index], "isExceed", el.scrollWidth > el.clientWidth)
        })
      }
    }
  }
}
</script>

<style scoped>
.multiple_select_checkbox >>> .el-select-dropdown .el-checkbox.el-tooltip .el-checkbox__label {
  width: var(--text-width);
  max-width: var(--text-max-width);
  vertical-align: middle;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.multiple_select_checkbox.inline >>> .el-select-dropdown__item:not(:first-child) {
  display: inline-block;
}

.multiple_select_checkbox.inline >>> .el-select-dropdown__item.selected::after {
  content: "";
}
</style>