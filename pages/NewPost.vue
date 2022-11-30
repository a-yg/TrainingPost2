<script>
export default {
  data() {
    return {
      userTitle: "",
      userText: "",
    }
  },
  methods: {
    onSave() {
      if(!this.userTitle || !this.userText) return 
      this.$axios.post('https://vue-example-259bb-default-rtdb.firebaseio.com/surveys.json', {
        id: Math.random(),
        title: this.userTitle,
        text: this.userText,
      }).then(() => {
        alert('保存されました')
      })
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
      <input type="text" v-model="userTitle">
    </div>
    <div class="text">
      <label for="">本文:</label>
      <textarea v-model="userText"></textarea>
    </div>
    <button @click="onSave();onForm();">保存</button>
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