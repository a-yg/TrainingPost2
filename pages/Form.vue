<script>
import List from '../components/List.vue';

export default {
  data() {
    return {
      lists: []
    }
  },
  components: {
    List,
  },
  async mounted() {
    try {
      const response = await this.$axios.get(
        'https://vue-example-259bb-default-rtdb.firebaseio.com/surveys.json'
      );
      this.lists = response.data;
      console.log(this.lists, 'aaaaa')
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    onNewPost() {
      this.$router.push('/newpost')
    },
    async clickDelete(index) {
      const key = Object.keys(this.lists)[index]
      await this.$axios.delete(`https://vue-example-259bb-default-rtdb.firebaseio.com/surveys/${key}.json`).then(() => {
        console.log("削除成功");
        this.$delete(this.lists, key);
      })
      .catch(() => {
          console.log("失敗");
        });
    },
  }
}
</script>

<template>
  <div class="container">
    <h1>Title</h1>
    <div class="sub-container">
      <h3>explanation</h3>
      <button @click="onNewPost">新規作成</button>
    </div>
    <hr>
    <div>
      <ul>
        <List :lists="lists" @onDelete="clickDelete" />
      </ul>
    </div>
  </div>
</template>

<style scoped>

.sub-container {
  display: flex;
  justify-content: space-between;
}
ul {
  padding: 0;
}


</style>