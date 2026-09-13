<template>
  <div class="shopping-list">
    <h1>{{ title }}</h1>

    <div class="add-item-form">
      <input
        v-model="newItem"
        @keyup.enter="addItem"
        placeholder="Add a shopping item"
      />

      <button @click="addItem" :disabled="newItem.length === 0">
        Add Item
      </button>
    </div>

    <p v-if="items.length === 0">
      Your shopping list is empty.
    </p>

    <ul v-else>
      <li
        v-for="item in items"
        :key="item.id"
        :class="{ purchased: item.purchased }"
      >
        <span @click="togglePurchased(item)">
          {{ item.name }}
        </span>

        <button @click="removeItem(item.id)">
          Remove
        </button>
      </li>
    </ul>

    <p>Total Items: {{ totalItems }}</p>
    <p>Items Purchased: {{ purchasedItems }}</p>
    <p>Items Remaining: {{ remainingItems }}</p>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      title: "My Shopping List",
      newItem: "",
      items: [
        {
          id: 1,
          name: "Milk",
          purchased: false
        },
        {
          id: 2,
          name: "Bread",
          purchased: false
        },
        {
          id: 3,
          name: "Eggs",
          purchased: false
        }
      ]
    };
  },

  computed: {
    totalItems() {
      return this.items.length;
    },

    purchasedItems() {
      return this.items.filter(item => item.purchased).length;
    },

    remainingItems() {
      return this.items.filter(item => !item.purchased).length;
    }
  },

  methods: {
    addItem() {
      if (this.newItem.trim() === "") {
        return;
      }

      this.items.push({
        id: Date.now(),
        name: this.newItem,
        purchased: false
      });

      this.newItem = "";
    },

    removeItem(id) {
      this.items = this.items.filter(item => item.id !== id);
    },

    togglePurchased(item) {
      item.purchased = !item.purchased;
    }
  }
};
</script>

<style scoped>
body {
  font-family: Arial, sans-serif;
  background-color: #f5f5f5;
}

.shopping-list {
  max-width: 600px;
  margin: 50px auto;
  background: white;
  padding: 30px;
  border-radius: 10px;
}

h1 {
  text-align: center;
}

.add-item-form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  flex: 1;
  padding: 10px;
}

button {
  padding: 10px 15px;
  cursor: pointer;
}

ul {
  padding: 0;
}

li {
  list-style: none;
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

li span {
  cursor: pointer;
}

.purchased span {
  text-decoration: line-through;
  opacity: 0.5;
}
</style>