<template>
  <div id="table-page">
    <div class="box">
      <button href="javascript:;" class="prev" @click="goToPre" :disabled="firstPage">上一页</button>
      <template v-for="number in numbers">
        <span class="number" :class="{active: number === currentPage}" @click="goToPage(number)">{{number}}</span>
      </template>
      <button href="javascript:;" class="next"  :disabled="lastPage" @click="goToNext">下一页</button>
      <span class="skip">到第
        <input type="text" min="1" v-model="givenPage">页
        <button type="button" class="btn" @click="goToGivenPage()">确定</button>
      </span>
    </div>
  </div>

</template>

<script>
  export default {
    name: 'Page',
    data () {
      return {
        totalPages: null,
        currentPage: null,
        givenPage: null,
        paging: null
      }
    },
    props: {
      queryParams: {
        type: 'Object'
      },
      url: {
        type: 'String'
      }
    },
    computed: {
      numbers: function () {
        let spr = '...'
        let numbers
        if (this.totalPages < 5 || (this.currentPage === 3 && this.totalPages === 5)) {
          numbers = this.totalPages
        } else {
          if (this.currentPage < 3) {
            numbers = [1, 2, 3, spr, this.totalPages]
          } else if (this.currentPage > this.totalPages - 2) {
            numbers = [1, spr, this.totalPages - 2, this.totalPages - 1, this.totalPages]
          } else {
            if (this.currentPage === 3) {
              numbers = [1, 2, 3, 4, spr, this.totalPages]
            } else if (this.currentPage === 4 && this.totalPages === 6) {
              numbers = [1, spr, 3, 4, 5, 6]
            } else {
              numbers = [1, spr, this.currentPage - 1, this.currentPage, this.currentPage + 1, spr, this.totalPages]
            }
          }
        }
        return numbers
      },
      firstPage: function () {
        return this.currentPage === 1
      },
      lastPage: function () {
        return this.currentPage === this.totalPages
      }
    },
    methods: {
      goToPage: function (number) {
        if (!Number(number)) return
        this.currentPage = number
        this.search()
      },
      goToGivenPage: function () {
        if (this.givenPage <= 0 || this.givenPage > this.totalPages) return
        this.currentPage = Number(this.givenPage)
        this.search()
      },
      goToPre: function () {
        if (this.firstPage) return
        this.currentPage -= 1
        this.search()
      },
      goToNext: function () {
        if (this.lastPage) return
        this.currentPage += 1
        this.search()
      },
      search: function () {
        this.$emit('pagingEvent')
      },

    }
  }
</script>

<style scoped>
  .number {
    diplay: inline-block;
    height: 26px;
    line-height: 26px;
    padding: 0 12px;
    font-size: 12px;
    cursor: pointer;
  }
  .active {
    background-color: aqua;
  }

</style>
