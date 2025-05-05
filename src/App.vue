<template>
  <div class="app">
    <h1>Loja</h1>
    <div class="carousel">
      <button @click="prevSlide" class="carousel-button prev">❮</button>
      <div class="carousel-track">
        <div v-for="produto in produtos" :key="produto.id" class="carousel-item"
          :style="{ transform: `translateX(-${indiceAtual * 100}%)` }">
          <img :src="produto.imagem" :alt="produto.nome" class="product-image" />
          <h2>{{ produto.nome }}</h2>
          <p>{{ produto.descricao }}</p>
        </div>
      </div>
      <button @click="nextSlide" class="carousel-button next">❯</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface Produto {
  id: number;
  nome: string;
  imagem: string;
  descricao: string;
}

const produtos = ref<Produto[]>([
  { id: 1, nome: 'Produto 1', imagem: 'https://via.placeholder.com/150', descricao: 'Descrição do Produto 1' },
  { id: 2, nome: 'Produto 2', imagem: 'https://via.placeholder.com/150', descricao: 'Descrição do Produto 2' },
  { id: 3, nome: 'Produto 3', imagem: 'https://via.placeholder.com/150', descricao: 'Descrição do Produto 3' },
  { id: 4, nome: 'Produto 4', imagem: 'https://via.placeholder.com/150', descricao: 'Descrição do Produto 4' },
  { id: 5, nome: 'Produto 5', imagem: 'https://via.placeholder.com/150', descricao: 'Descrição do Produto 5' },
  { id: 6, nome: 'Produto 6', imagem: 'https://via.placeholder.com/150', descricao: 'Descrição do Produto 6' },
  { id: 7, nome: 'Produto 7', imagem: 'https://via.placeholder.com/150', descricao: 'Descrição do Produto 7' },
  { id: 8, nome: 'Produto 8', imagem: 'https://via.placeholder.com/150', descricao: 'Descrição do Produto 8' },
  { id: 9, nome: 'Produto 9', imagem: 'https://via.placeholder.com/150', descricao: 'Descrição do Produto 9' },
  { id: 10, nome: 'Produto 10', imagem: 'https://via.placeholder.com/150', descricao: 'Descrição do Produto 10' },
]);

const indiceAtual = ref(0);

const nextSlide = () => {
  indiceAtual.value = (indiceAtual.value + 1) % produtos.value.length;
};

const prevSlide = () => {
  indiceAtual.value =
    (indiceAtual.value - 1 + produtos.value.length) % produtos.value.length;
};
</script>

<style scoped>
.app {
  text-align: center;
  font-family: Arial, sans-serif;
}

.carousel {
  position: relative;
  width: 80%;
  margin: 0 auto;
  overflow: hidden;
}

.carousel-track {
  display: flex;
  transition: transform 0.5s ease-in-out;
}

.carousel-item {
  min-width: 100%;
  box-sizing: border-box;
  padding: 20px;
}

.product-image {
  width: 150px;
  height: 150px;
  object-fit: cover;
  margin-bottom: 10px;
}

.carousel-button {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
  border: none;
  padding: 10px;
  cursor: pointer;
}

.carousel-button.prev {
  left: 10px;
}

.carousel-button.next {
  right: 10px;
}
</style>
