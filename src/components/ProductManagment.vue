<script setup>
import { createProduct, getProducts } from "../api/products";
</script>

<script>
export default {
  name: "ProductManager",
  data() {
    return {
      products: [],
      newProduct: {
        name: "",
        price: 0,
        inStock: false,
      },
    };
  },

  methods: {
    async loadProducts() {
      this.products = await getProducts();
    },

    async addProduct(product) {
      await createProduct(product);
      await this.loadProducts();
    },

    async submitCreateProduct() {
      await this.addProduct(this.newProduct);
    },
  },

  mounted() {
    this.loadProducts();
  },
};
</script>

<template>
  <main class="flex justify-center flex-col items-center min-h-screen">
    <h1 class="text-3x1">Termékek kezelése</h1>

    <table class="flex flex-col w-full p-4">
      <thead>
        <tr class="w-full flex justify-between">
          <th>azonosító</th>
          <th>név</th>
          <th>ár</th>
          <th>raktáron</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="product in products"
          :key="product.id"
          class="w-full flex justify-between my-2"
        >
          <td>{{ product.id }}</td>
          <td>{{ product.name }}</td>
          <td>{{ product.price }}</td>
          <td>{{ product.inStock ? "Igen" : "Nem" }}</td>
        </tr>
      </tbody>
    </table>

    <form
      @submit.prevent="submitCreateProduct"
      class="flex flex-col mt-3 border-2 border-stone-200 rounded-2xl p-5 my-5 gap-3 shadow-2xl shadow-stone-200"
    >
      <label for="name">Name: </label>
      <input
        class="border-2 border-stone-200 rounded-xl p-2 py-1"
        type="text"
        name="name"
        placeholder="Laptop..."
        v-model="newProduct.name"
      />

      <label for="price">Price: </label>
      <input
        type="number"
        name="price"
        placeholder="1 500 Ft"
        v-model.number="newProduct.price"
      />

      <div class="flex justify-between">
        <label for="inStock">Raktáron:</label>
        <input type="checkbox" name="inStock" v-model="newProduct.inStock" />
      </div>

      <button
        type="submit"
        class="border-2 border-stone-200 rounded-xl p-2 cursor-pointer"
      >
        Létrehozás
      </button>
    </form>
  </main>
</template>
