<template>
  <div class="app-container">
    <el-input v-model="filterText" placeholder="Filter keyword" style="margin-bottom:30px;" />

    <el-tree
      ref="tree2"
      :data="data2"
      :props="defaultProps"
      :filter-node-method="filterNode"
      class="filter-tree"
      default-expand-all
    />

  </div>
</template>

<script>
import subject from '@/api/edu/subject'
export default {

  data() {
    return {
      filterText: '',
      data2: [], // 返回所有分类数据
      defaultProps: {
        children: 'children',
        label: 'title' // 对应返回值的key
      }
    }
  },
  watch: {
    filterText(val) {
      this.$refs.tree2.filter(val) // 这个最后也没说是什么
    }
  },
  created() {
    this.getAllSubjectList() // 页面进入加载
  },

  methods: {
    getAllSubjectList() {
      subject.getSubjectList()
        .then(response => {
          this.data2 = response.data.list
        })
    },
    filterNode(value, data) {
      if (!value) return true
      return data.title.toLowerCase().indexOf(value.toLowerCase()) !== -1 // title也是对应返回值的key
    }
  }
}
</script>
