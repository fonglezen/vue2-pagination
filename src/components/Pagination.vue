<template>
    <div class="pagination">
        <ul>
            <li class="page-item" :class="{'disabled': prevabled}" @click="prevpage">
              <a class="page-link">Prev</a>
            </li>

            <li class="page-item" :class="{'active':page.value == nowpage + 1}" v-for="(page, index) in pages" @click="jumpPage(page.value)" :key="index"><a class="page-link">{{page.value}}</a></li>

            <li class="page-item" :class="{'disabled': nextabled}" @click="nextpage">
              <a class="page-link">Next</a>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
  data() {
    return {
      pages: [
        {
          value: 1
        }
      ],
      nowpage: 0,
      total_page: 1
    };
  },

  props: ["pageSize"],

  computed: {
    prevabled() {
      return this.nowpage == 0;
    },
    nextabled() {
      return this.nowpage == this.total_page - 1;
    }
  },

  mounted() {
    this.$on("getOffset", function(callback) {
      this.getOffset(callback);
    });

    this.$on("countPage", function(count) {
      this.countPage(count);
    });
  },

  methods: {
    //   得到当前页码条数开始的index
    getOffset(callback) {
      const offset = this.pageSize * this.nowpage;
      callback(offset);
    },

    // 上一页
    // 执行跳页
    prevpage() {
      if (this.nowpage > 0) {
        this.jumpPage(this.nowpage);
      }
    },

    // 下一页
    // 执行跳页
    nextpage() {
      if (this.nowpage < this.total_page - 1) {
        this.jumpPage(this.nowpage + 2);
      }
    },

    // 执行跳页事件，通过回调方法的方式调用跳页后的行为
    jumpPage(page) {
      this.nowpage = page - 1;
      this.reCountPage(parseInt(page));

      // 執行父組件的getList方法
      this.$emit("getlist");
    },

    // 初始化，计算页码
    countPage(count) {
      var _t = this;

      var total_page = parseInt(count / _t.pageSize);
      if (count > _t.pageSize && count % _t.pageSize != 0) {
        total_page += 1;
      }

      if (total_page == 0) {
        total_page = 1;
      }
      _t.total_page = total_page;
      _t.reCountPage(1);
    },

    /* 点击页码之后，重新计算需要显示的页码 */
    reCountPage(nowpage) {
      var _t = this;
      // nowpage 当前点击的页码
      // 如果小于6，则显示1-末尾页，
      // 如果大于等于6，则以当前的数为中心，前面显示5个数，后面显示4个数
      if (nowpage < 6) {
        _t.pages = [];
        var end_length = _t.total_page;
        if (end_length > 10) {
          end_length = 10;
        }

        for (var i = 0; i < end_length; i++) {
          _t.pages.push({
            value: i + 1
          });
        }
      } else {
        _t.pages = [];
        for (var i = 0; i < 5; i++) {
          _t.pages.push({
            value: nowpage - (5 - i)
          });
        }

        _t.pages.push({
          value: nowpage
        });

        var end_page = nowpage + 5;
        if (end_page > _t.total_page) {
          end_page = _t.total_page;
        }
        var end_length = end_page - nowpage;
        for (var i = 0; i < end_length; i++) {
          _t.pages.push({
            value: nowpage + i + 1
          });
        }
      }
    }
  }
};
</script>

<style scoped>
.pagination ul{
  padding-left: 0;
}
.page-item{
  float: left;
  padding:0 1em;
  line-height: 2em;
  list-style-type: none;
  border: 1px solid #eee;
}
.page-item.active {
  color: #fff;
  background: #007bff;
}

.page-item.disabled {
  color: #999;
  cursor: not-allowed;
}
</style>
