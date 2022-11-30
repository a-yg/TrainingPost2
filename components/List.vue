<script>
import EditPost from '../pages/EditPost/_id.vue'
export default {
  props: [ 'lists'],
  data () {
    return {
      currentPage: 0,   // 現在のページ番号
      size: 5,         // 1ページに表示するアイテムの上限
      pageRange: 5,    // ページネーションに表示するページ数の上限
    }
  },
  components: {
    EditPost
  },
  methods: {
    onDelete(index) {
      if (confirm("Delete?")) {
        this.$emit('onDelete', index)
      }
    },
    onEdit(index) {
      const param = Object.keys(this.$props.lists)[index]
      console.log('eidt param', param)
      this.$router.push({path:`/editPost/${param}`});
      console.log(index, 'index2')
      // this.$emit('listEdit', index)
    },
    /**
     * ページ先頭に移動する
     */
    first () {
      this.currentPage = 0;
      this.selectHandler();
    },
    /**
     * ページ後尾に移動する
     */
    last () {
      this.currentPage = this.pages - 1;
      this.selectHandler();
    },
    /**
     * 1ページ前に移動する
     */
    prev () {
      if (0 < this.currentPage) {
        this.currentPage--;
        this.selectHandler();
      }
    },
    /**
     * 1ページ次に移動する
     */
    next () {
      if (this.currentPage < this.pages - 1) {
        this.currentPage++;
        this.selectHandler();
      }
    },
    /**
     * 指定したページに移動する
     * @param {number} index ページ番号
     */
    pageSelect (index) {
      this.currentPage = index - 1;
      this.selectHandler();
    },
    /**
     * ページを変更したときの処理
     */
    selectHandler () {
      // なんかの処理
    }
  },
  computed: {
    /**
     * ページ数を取得する
     * @return {number} 総ページ数(1はじまり)
     */
    pages () {
      return Math.ceil(Object.keys(this.lists).length / this.size);
    },
    /**
     * ページネーションで表示するページ番号の範囲を取得する
     * @return {Array<number>} ページ番号の配列
     */
    displayPageRange () {
      const half = Math.ceil(this.pageRange / 2);
      let start, end;

      if (this.pages < this.pageRange) {
        // ページネーションのrangeよりページ数がすくない場合
        start = 1;
        end = this.pages;
      
      } else if (this.currentPage < half) {
        // 左端のページ番号が1になったとき
        start = 1;
        end = start + this.pageRange - 1;

      } else if (this.pages - half < this.currentPage) {
        // 右端のページ番号が総ページ数になったとき
        end = this.pages;
        start = end - this.pageRange + 1;

      } else {
        // activeページを中央にする
        start = this.currentPage - half + 1;
        end = this.currentPage + half;
      }
    
      let indexes = [];
      for (let i = start; i <= end; i++) {
        indexes.push(i);
      }
      return indexes;
    },
    /**
     * 現在のページで表示するアイテムリストを取得する
     * @return {any} 表示用アイテムリスト
     */
    displayItems () {
      const head = this.currentPage * this.size;
      const list = Object.values(this.lists)
      return list.slice(head, head + this.size);
    },
    /**
     * 現在のページかどうか判定する
     * @param {number} page ページ番号
     * @return　{boolean} 現在のページならtrue
     */
    isSelected (page) {
      return page - 1 === this.currentPage;
    }
  },
}
</script>

<template>
  <div>
    <div id="app" class="container">
    <nav>
    <ul class="pagination">
      <li class="page-item">
        <a @click="first" class="page-link" href="#">&laquo;</a>
      </li>
      <li class="page-item">
        <a @click="prev" class="page-link" href="#">&lt;</a>
      </li>

      <li
        v-for="(page, index) in displayPageRange"
        :key="`page--${index}`"
        class="page-item"
        :class="{active: page-1 === currentPage}">
        <a @click="pageSelect(page)" class="page-link" href="#">{{ page }}</a>
      </li>

      <li class="page-item">
        <a @click="next" class="page-link" href="#">&gt;</a>
      </li>
      <li class="page-item">
        <a @click="last" class="page-link" href="#">&raquo;</a>
      </li>
    </ul>
    </nav>
    <ul class="list-group">
    <li v-for="(list, index) in displayItems" :key="`list--${index}`" class="list">
      <span>件名:{{ list.title }}</span>
        <span>本文:{{ list.text }}</span>
        <div>
          <button @click="onEdit(index)"><span>編集</span></button>
          <button @click="onDelete(index)"><span>削除</span></button>
        </div>
    </li>
    </ul>
</div>
    <!-- <div>
    <li v-for="(list, index) in this.lists" :key="list.id" class="list">
        <span>件名:{{ list.title }}</span>
        <span>本文:{{ list.text }}</span>
        <div>
          <button @click="onEdit"><span>編集</span></button>
          <button @click="onDelete(index)"><span>削除</span></button>
        </div>
    </li>
    </div> -->
  </div>
</template>

<style scoped>
.page-item {
  list-style: none;
}
.pagination {
  display: flex;
  justify-content: center;
  font-size: 15px;
  margin-bottom: 20px;
}
.page-link {
  color: gray;
  padding: 7px;
  text-decoration: none;
  border: solid 2px rgb(198, 195, 195);
}
.list-group {
  margin: 0;
  padding: 0;
}
.list {
  background-color: rgb(192, 189, 189);
  list-style: none;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 50px;
  padding: 15px;
  margin: 0 20px 10px 20px;
}
</style>