<template>
  <div class="home">
    <div class="input-section">
      <div class="input-fields">
        <input
          class="input"
          type="text"
          placeholder="Add todo item..."
          v-model="inputValue"
        />
      </div>
      <div>
        <button class="button" @click="postData">Add ToDo</button>
      </div>
      <div>
        <button class="button" @click="fetchData">Refresh list</button>
      </div>
    </div>
    <div v-if="loading">Loading...</div>
    <div class="entries" v-else>
      <div class="entry" v-for="{ done, name, id } in entries" :key="id">
        <router-link class="link" :to="`/item/${id}`">
          <h1>{{ name }}</h1>
        </router-link>

        <div class="status-information">
          Completed:
          <div :class="done ? 'done' : 'pending'">
            <span v-if="done">Yes</span>
            <span v-else>No</span>
          </div>
        </div>
        <div class="edit-section">
          <button class="button" @click="deleteData(id)">Delete</button>
          <button class="button" @click="putData(id, done)">
            <span v-if="done">Resume</span>
            <span v-else>Mark as done</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";

export type ItemType = {
  createdAt: string;
  name: string;
  done: boolean;
  id: string;
};

export default defineComponent({
  name: "HomeView",
  data: () => ({
    inputValue: "",
    loading: true,
    entries: [] as ItemType[],
  }),
  components: {},
  created() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      axios
        .get("https://620ce155b5736325939c5e08.mockapi.io/todos")
        .then(({ data }) => {
          this.entries = data;
          this.loading = false;
        });
    },
    postData() {
      if (this.inputValue === "") {
        return;
      }
      axios.post("https://620ce155b5736325939c5e08.mockapi.io/todos", {
        name: this.inputValue,
      });

      this.inputValue = "";
    },
    putData(id: string, completed: boolean) {
      axios.put(`https://620ce155b5736325939c5e08.mockapi.io/todos/${id}`, {
        done: !completed,
      });
    },
    deleteData(id: string) {
      axios.delete(`https://620ce155b5736325939c5e08.mockapi.io/todos/${id}`);
    },
  },
});
</script>

<style scoped lang="scss">
.home {
  display: grid;
  grid-template-columns: 400px 1fr;
}

.input-section {
  display: flex;
  flex-direction: column;
  padding: 30px 0 0 0;
  gap: 10px;
}

.input {
  font-size: 18px;
  line-height: 1.6;
}

.button {
  width: fit-content;
  padding: 5px 15px;
  background-color: #f6ce86;
  color: teal;
  font-family: "DejaVu Sans", sans-serif;
  border: 1px solid transparent;
  border-radius: 5px;
  transition: 0.2s;

  &:hover {
    cursor: pointer;
    opacity: 0.8;
  }

  &:active {
    cursor: pointer;
    background-color: gold;
  }
}

.entries {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  grid-gap: 15px;
}

.entry {
  padding: 5px 0;
  border: 1px solid slategray;
  border-radius: 5px;
}

.link {
  text-decoration: none;
  color: inherit;
}

.status-information {
  display: flex;
  justify-content: center;
  gap: 4px;
}

.pending {
  font-weight: bold;
  color: deeppink;
}

.done {
  font-weight: bold;
  color: limegreen;
}

.edit-section {
  display: flex;
  justify-content: center;
  gap: 10px;
  padding: 5px 0;
}
</style>
