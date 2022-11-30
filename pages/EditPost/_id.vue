<script>
export default {
  data() {
    return {
      detail: {},
      titleValue: "",
      textValue: ""
    }
  },
  async mounted() {
    this.param = this.$route.params.id
    try {
      const response = await this.$axios.get(`https://vue-example-259bb-default-rtdb.firebaseio.com/surveys/${this.param}.json`);
      this.detail = response.data;
    } catch (error) {
        console.log("get失敗");
      }
  },
  methods: {
    async onOverwriteSave() {
      this.detail.title = this.titleValue
      this.detail.text = this.textValue
      const updateData = {title: this.detail.title, text: this.detail.text}
      this.param = this.$route.params.id,
      await this.$axios.put(`https://vue-example-259bb-default-rtdb.firebaseio.com/surveys/${this.param}.json`, updateData).then(() => {
        console.log('編集完了');
      })
      .catch(() => {
        console.log('失敗')
      });
    },
    onForm() {
      this.$router.push('/form')
    }
  }
}

</script>

<template>
  <div class="container">
    <h2>title</h2>
    <div class="title">
      <label for="">件名:</label>
      <input type="text" :value="this.detail.title" @input="titleValue = $event.target.value">
      <!-- v-modeでもOK、その場合thisは不要 -->
    </div>
    <div class="text">
      <label for="">本文:</label>
      <textarea :value="this.detail.text" @input="textValue = $event.target.value"></textarea>
    </div>
    <button @click="onOverwriteSave();onForm();">編集完了</button>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.title {
  margin: 20px;
  display: flex;
  flex-direction: column;
}
input {
  width: 300px;
}
.text {
  display: flex;
  flex-direction: column;
}
textarea {
  width: 300px;
  height: 200px;
  /* font-weight: normal; */
}
button {
  margin: 20px;
  width: 80px;
  height: 40px;
}
</style>