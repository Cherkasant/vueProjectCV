<template>
  <form class="card card-w30" @submit.prevent="add">
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select id="type" v-model="type">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>
    <div class="form-control">
      <label for="value">Значение</label>
      <textarea id="value" rows="3" v-model="text"></textarea>
    </div>

    <button
        class="btn primary"
        :disabled="textLength"
    >
      Добавить
    </button>
  </form>
</template>

<script>
export default {
  emits: ['check'],
  data() {
    return {
      text: '',
      type: 'title'
    }
  },
  name: "AppForm",
  methods: {
    add() {
      this.$emit('check', {
        type: this.type,
        text: this.text,
        id: Math.random() + Date.now()
      })
      this.type = "title";
      this.text = "";
    }
  },
  computed: {
    textLength() {
      return this.text.length < 3 ? true : false;
    },
  },
}
</script>

<style scoped>

</style>