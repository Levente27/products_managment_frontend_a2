<script setup>
import { getProducts } from '../api/products';
</script>

<script>
    export default {
        name: "ProductManager",
        data()
        {
            return {
                products: []
            }
        },

        methods: {
            async loadProducts(){
                this.products = await getProducts();
            },
        },

        mounted(){
            this.loadProducts()
        }
    }
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
                <tr v-for="product in products" :key="product.id" class="w-full flex justify-between my-2">
                    <td>{{ product.id }}</td>
                    <td>{{ product.name }}</td>
                    <td>{{ product.price }}</td>
                    <td>{{ product.inStock ? "Igen" : "Nem" }}</td>
                </tr>
            </tbody>
        </table>
    </main>
</template>