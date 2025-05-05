<script setup lang="ts">
import { ref, computed } from 'vue';

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
import imagem11 from './assets/produto11.jpeg';
import imagem12 from './assets/produto12.jpeg';
import imagem13 from './assets/produto13.jpeg';
import imagem14 from './assets/produto14.jpeg';

interface Produto {
  id: number;
  nome: string;
  imagem: string;
  preco: number;
  quantidade?: number;
}

const produtos = ref<Produto[]>([
  { id: 1, nome: 'Chinelo', imagem: imagem1, preco: 49.9 },
  { id: 2, nome: 'Camisa branca', imagem: imagem2, preco: 59.9 },
  { id: 3, nome: 'Camisa branca para criança', imagem: imagem3, preco: 69.9 },
  { id: 4, nome: 'Camisa preta para criança', imagem: imagem4, preco: 79.9 },
  { id: 5, nome: 'Fantasia do Chamas', imagem: imagem5, preco: 89.9 },
  { id: 6, nome: 'Jogo de playstation', imagem: imagem6, preco: 99.9 },
  { id: 7, nome: 'Roupa de cama', imagem: imagem7, preco: 209.9 },
  { id: 8, nome: 'Mochila', imagem: imagem8, preco: 837.9 },
  { id: 9, nome: 'Mochila preta', imagem: imagem9, preco: 0.9 },
  { id: 10, nome: 'Mochila de rodinhas', imagem: imagem10, preco: 23.9 },
  { id: 11, nome: 'Perfume', imagem: imagem11, preco: 49.90 },
  { id: 12, nome: 'Relogio', imagem: imagem12, preco: 77.90 },
  { id: 13, nome: 'Ovo', imagem: imagem13, preco: 38.90 },
  { id: 14, nome: 'Notebook', imagem: imagem14, preco: 8.90 },
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
  const itemCarrinho = carrinho.value.find((item) => item.id === produto.id);
  if (itemCarrinho) {
    itemCarrinho.quantidade! += 1;
  } 
  
  else {
    carrinho.value.push({ ...produto, quantidade: 1 });
  }
};

const removerDoCarrinho = (produto: Produto) => {
  const index = carrinho.value.findIndex((item) => item.id === produto.id);
  if (index !== -1) {
    carrinho.value.splice(index, 1);
    alert(`Produto "${produto.nome}" removido do carrinho!`);
  }
};

const adicionarMaisUm = (produto: Produto) => {
  const itemCarrinho = carrinho.value.find((item) => item.id === produto.id);
  if (itemCarrinho) {
    itemCarrinho.quantidade! += 1;
  }
};

const removerUm = (produto: Produto) => {
  const itemCarrinho = carrinho.value.find((item) => item.id === produto.id);
  if (itemCarrinho && itemCarrinho.quantidade! > 1) {
    itemCarrinho.quantidade! -= 1;
  } else {
    removerDoCarrinho(produto);
  }
};

const precoTotal = computed(() =>
  carrinho.value.reduce((total, item) => total + item.preco * (item.quantidade || 0), 0)
);

const realizarCompra = () => {
  carrinho.value = [];
  alert('Compra realizada!');
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
      <button @click="adicionarAoCarrinho(produtoSelecionado)" class="add-carrinho">Adicionar ao Carrinho</button>
    </div>

    <div class="carrinho">
      <h2>Carrinho</h2>
      <ul>
        <li v-for="item in carrinho" :key="item.id">
          <img :src="item.imagem" :alt="item.nome" class="carrinho-imagem" />
          <span>{{ item.nome }}</span>
          <span>Quantidade: {{ item.quantidade }}</span>
          <span>Preço: R$ {{ (item.preco * item.quantidade!).toFixed(2) }}</span>
          <button @click="adicionarMaisUm(item)" class="add-mais-um">+</button>
          <button @click="removerUm(item)" class="remove-um">-</button>
          <button @click="removerDoCarrinho(item)" class="remove-carrinho">Remover</button>
        </li>
      </ul>
      <h3>Preço Total: R$ {{ precoTotal.toFixed(2) }}</h3>
      <button @click="realizarCompra" class="comprar">Comprar</button>
    </div>
  </div>
</template>

<style scoped>
.app {
  text-align: center;
  font-family: Arial, sans-serif;
  margin: 0 auto;
  padding: 20px;
  max-width: 1200px;
}

h1 {
  font-size: 2.5rem;
  color: #333;
  margin-bottom: 20px;
}

.carousel {
  position: relative;
  width: 80%;
  margin: 0 auto;
  overflow: hidden;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.carousel-track {
  display: flex;
  transition: transform 0.5s ease-in-out;
}

.carousel-item {
  min-width: 100%;
  box-sizing: border-box;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 10px;
  text-align: center;
}

.produto-img {
  width: 150px;
  height: 150px;
  object-fit: cover;
  margin-bottom: 10px;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
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
  border-radius: 50%;
  font-size: 1.5rem;
  transition: background-color 0.3s ease;
}

.carousel-button:hover {
  background-color: rgba(0, 0, 0, 0.8);
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
  background-color: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  display: inline-block;
  text-align: left;
  max-width: 600px;
}

.produto-detalhes h2 {
  font-size: 1.8rem;
  color: #333;
  margin-bottom: 10px;
}

.produto-detalhes h3 {
  font-size: 1.5rem;
  color: #555;
  margin-bottom: 10px;
}

.produto-detalhes p {
  font-size: 1.2rem;
  color: #666;
  margin-bottom: 15px;
}

.add-carrinho {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s ease;
}

.add-carrinho:hover {
  background-color: #218838;
}

.carrinho {
  margin-top: 20px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: left;
}

.carrinho h2 {
  font-size: 1.8rem;
  color: #333;
  margin-bottom: 15px;
}

.carrinho ul {
  list-style: none;
  padding: 0;
}

.carrinho li {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
}

.carrinho-imagem {
  width: 50px;
  height: 50px;
  object-fit: cover;
  margin-right: 10px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.remove-carrinho {
  padding: 5px 10px;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: background-color 0.3s ease;
}

.remove-carrinho:hover {
  background-color: #c82333;
}

.add-mais-um,
.remove-um {
  margin: 0 5px;
  padding: 5px 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: background-color 0.3s ease;
}

.add-mais-um:hover,
.remove-um:hover {
  background-color: #0056b3;
}

.comprar {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #ffc107;
  color: #333;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s ease;
}

.comprar:hover {
  background-color: #e0a800;
}
</style>
