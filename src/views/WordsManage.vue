<template>
  <section class="comment_list">
    <words-search-panel></words-search-panel>
    <words-search-list :tableData="tableData"></words-search-list>
  </section>
</template>

<script>
import wordsSearchPanel from '@/components/words_manage/wordsSearchPanel'
import wordsSearchList from '@/components/words_manage/wordsSearchList'
import api from '@/api'
export default {
  components: {
    wordsSearchPanel,
    wordsSearchList
  },
  data () {
    return {
      pageIndex: 1,
      pageSize: 10,
      list: [],
      total: 0
    }
  },
  computed: {
    tableData () {
      return {
        pageIndex: this.pageIndex,
        pageSize: this.pageSize,
        list: this.list,
        total: this.total
      }
    }
  },
  methods: {
    listComments (data) {
      const params = Object.assign({
        pageIndex: this.pageIndex,
        pageSize: this.pageSize,
        state: 1
      }, data)
      api.listComments(params).then(res => {
        if (res.data && res.data.list) {
          this.total = res.data.total
          this.list = res.data.list
        } else {
          this.list = []
        }
      }).catch(err => this.$message.error(err.message))
    }
  },
  created () {
    this.listComments()
    this.$eventhub.$on('listComments', data => this.listComments(data))
  }
}
</script>

<style lang="scss" scoped>
</style>
