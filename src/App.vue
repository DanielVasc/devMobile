<script setup lang="ts">
import { ref } from 'vue';

import imagem1 from './assets/produto1.webp';
import imagem2 from './assets/produto2.webp';
import imagem3 from './assets/produto3.webp';
import imagem4 from './assets/produto4.webp';
import imagem5 from './assets/produto5.webp';
import imagem6 from './assets/produto6.webp';
import imagem7 from './assets/produto7.webp';
import imagem8 from './assets/produto8.webp';
import imagem9 from './assets/produto9.webp';
import imagem10 from './assets/produto10.webp';

interface Produto {
  id: number;
  nome: string;
  imagem: string;
  preco: string;
}

const produtos = ref<Produto[]>([
  { id: 1, nome: 'Chinelo', imagem: imagem1, preco: 'R$ 49,90' },
  { id: 2, nome: 'Camisa branca', imagem: imagem2, preco: 'R$ 59,90' },
  { id: 3, nome: 'Camisa branca para criança', imagem: imagem3, preco: 'R$ 69,90' },
  { id: 4, nome: 'Camisa preta para criança', imagem: imagem4, preco: 'R$ 79,90' },
  { id: 5, nome: 'Fantasia do Chamas', imagem: imagem5, preco: 'R$ 89,90' },
  { id: 6, nome: 'Jogo de playstation', imagem: imagem6, preco: 'R$ 99,90' },
  { id: 7, nome: 'Roupa de cama', imagem: imagem7, preco: 'R$ 209,90' },
  { id: 8, nome: 'Mochila', imagem: imagem8, preco: 'R$ 837,90' },
  { id: 9, nome: 'Mochila preta', imagem: imagem9, preco: 'R$ 00,90' },
  { id: 10, nome: 'Mochila de rodinhas', imagem: imagem10, preco: 'R$ 23,90' },
]);

const indiceAtual = ref(0);
const produtoSelecionado = ref<Produto | null>(null);
const carrinho = ref<Produto[]>([]);


const proxSlide = () => {
  indiceAtual.value = (indiceAtual.value + 1) % produtos.value.length;
};

const antSlide = () => {
  indiceAtual.value =
    (indiceAtual.value - 1 + produtos.value.length) % produtos.value.length;
};

const selecionarProduto = (produto: Produto) => {
  produtoSelecionado.value = produto;
};

const adicionarAoCarrinho = (produto: Produto) => {
  carrinho.value.push(produto);
  alert(`Produto "${produto.nome}" adicionado ao carrinho!`);
};
</script>

<template>
  <div class="app">
    <h1>Loja</h1>
    <div class="carousel">
      <button @click="antSlide" class="carousel-button ant">❮</button>
      <div class="carousel-track">
        <div
          v-for="produto in produtos"
          :key="produto.id"
          class="carousel-item"
          :style="{ transform: `translateX(-${indiceAtual * 100}%)` }"
          @click="selecionarProduto(produto)"
        >
          <img :src="produto.imagem" :alt="produto.nome" class="produto-img" />
          <h2>{{ produto.nome }}</h2>
        </div>
      </div>
      <button @click="proxSlide" class="carousel-button prox">❯</button>
    </div>

    <div v-if="produtoSelecionado" class="produto-detalhes">
      <h2>Detalhes do Produto</h2>
      <img :src="produtoSelecionado.imagem" :alt="produtoSelecionado.nome" class="produto-img" />
      <h3>{{ produtoSelecionado.nome }}</h3>
      <p>{{ produtoSelecionado.preco }}</p>
      <button @click="adicionarAoCarrinho(produtoSelecionado)" class="add-carrinho">
        Adicionar ao Carrinho
      </button>
    </div>

    <div class="carrinho">
      <h2>Carrinho</h2>
      <ul>
        <li v-for="item in carrinho" :key="item.id">
          <img :src="item.imagem" :alt="item.nome" class="cart-image" />
          <span>{{ item.nome }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

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

.produto-img {
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

.carousel-button.ant {
  left: 10px;
}

.carousel-button.prox {
  right: 10px;
}

.produto-detalhes {
  margin-top: 20px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  display: inline-block;
  text-align: left;
}

.add-carrinho {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.add-carrinho:hover {
  background-color: #218838;
}

.carrinho {
  margin-top: 20px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  text-align: left;
}

.carrinho ul {
  list-style: none;
  padding: 0;
}

.carrinho li {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.cart-image {
  width: 50px;
  height: 50px;
  object-fit: cover;
  margin-right: 10px;
}
</style>
