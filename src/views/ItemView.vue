<template>
  <div class="item-section">
    <h1 v-if="loading">Loading...</h1>
    <div class="item" v-else>
      <h1>ToDo Item No. {{ item.id }}</h1>
      <h3>{{ item.name }}</h3>
      <div>
        Completed:
        <span v-if="item.done">Yes</span>
        <span v-else>No</span>
      </div>
      <div class="creation-information">
        Item created:
        <span>{{ new Date(item.createdAt).toLocaleString() }}</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import { ItemType } from "@/views/HomeView.vue";

export default defineComponent({
  name: "ItemView",
  data: () => ({
    loading: true,
    item: undefined as unknown as ItemType,
  }),
  components: {},
  created() {
    const routerId = this.$route.params.id;
    this.fetchItemData(routerId);
  },
  methods: {
    fetchItemData(id: string | string[]) {
      axios
        .get(`https://620ce155b5736325939c5e08.mockapi.io/todos/${id}`)
        .then(({ data }) => {
          this.item = data;
          this.loading = false;
        });
    },
  },
});
</script>

<style scoped lang="scss">
.item {
  display: grid;
  grid-gap: 20px;
  width: 60%;
  margin: 15px auto;
}

.creation-information {
  display: flex;
  font-style: italic;
}
</style>
