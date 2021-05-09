<template>
  <div id="app">
    <div>{{value1}}</div>
    <MultipleSelectCheckbox v-model="value1" key="1" placeholder="请选择" collapse-tags :options="options" :popper-append-to-body="false"
                            style="width: 30%;">
    </MultipleSelectCheckbox>
    <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

    <div>{{value2}}</div>
    <MultipleSelectCheckbox v-model="value2" key="2" is-inline placeholder="请选择" collapse-tags :options="options" :popper-append-to-body="false"
                            style="width: 50%;">
    </MultipleSelectCheckbox>
    <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

    <div>{{value3}}</div>
    <MultipleSelectCheckbox v-model="value3" key="3" is-inline :text-width="100" placeholder="请选择" collapse-tags :options="options"
                            :popper-append-to-body="false" style="width: 80%;">
    </MultipleSelectCheckbox>
  </div>
</template>

<script>
import MultipleSelectCheckbox from "./components/MultipleSelectCheckbox"
export default {
  name: 'App',
  components: {
    MultipleSelectCheckbox
  },
  data() {
    return {
      value1: [],
      value2: [],
      value3: [],
      options: [
        {
          value: '选项1',
          label: '黄金糕11111111111111111111111'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }, {
          value: '选项6',
          label: '武汉热干面6666666666666666666'
        },
        {
          value: '选项7',
          label: '蔡甸牛肉面7777777777777777777'
        }
      ],
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  padding-inline-start: 100px;
}
</style>
