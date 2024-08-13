<template>
  <div class="container column">
    <AppForm @check="add"></AppForm>
    <AppCard :info="info"></AppCard>
  </div>
  <div class="container">
    <p>
      <button class="btn primary" @click="getComments">
        Загрузить комментарии
      </button>
    </p>
    <div class="loader" v-if="loading"></div>
    <AppComments :comments="comments" v-else></AppComments>
  </div>
</template>

<script>
import AppComments from "@/components/AppComments";
import AppCard from "@/components/AppCard";
import axios from "axios";
import AppForm from "@/components/AppForm";

export default {
  emits: ["check"],
  data() {
    return {
      comments: [],
      loading: false,
      info: [],
    };
  },
  name: "App",
  components: {
    AppForm,
    AppComments,
    AppCard,
  },
  methods: {
    add(block) {
      this.info.push(block)
      this.saveBlock(block)
    },
    async getComments() {
      this.loading = true;
      const {data} = await axios.get(
          "https://jsonplaceholder.typicode.com/comments?_limit=10"
      );
      this.comments = data;
      this.loading = false;
    },
    async saveBlock(block) {
      try {
        await axios.post('https://http-8c495-default-rtdb.firebaseio.com/blocks.json',
            {...block}
        )
      } catch (e) {
        console.log(e.message)
      }
    },
    async loadBlocks() {
      try {
        const {data} = await axios.get('https://http-8c495-default-rtdb.firebaseio.com/blocks.json')
        if (!data) {
          throw new Error('Список блоков пуст')
        }
        this.info = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
        console.log(this.info)
      } catch (e) {
        console.log(e.message)
      }
    }
  },
  mounted() {
    this.loadBlocks()
  }
};
</script>

<style lang="scss">
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>
