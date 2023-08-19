<template>
  <div class='main'>
    <h1 class='heading'>Список продуктов</h1>
    <ul class='list'>
      <li class='add-container list-item'>
        <input v-model='newProduct' @keyup.enter='addProduct' placeholder='Введите название продукта' class='add-input'>
      </li>
      <li v-for='(product, index) in products'
          :key='index'
          class='list-item'
          @click='crossOut(product)'>
        <span :class="{ 'crossed-out': product.crossed }">{{ product.name }}</span>
        <button @click.stop='removeProduct(index)'>Удалить</button>
      </li>
    </ul>
    <button class='clear-button' @click.stop='clearList'>Очистить</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newProduct: '',
      products: JSON.parse(localStorage.getItem('products')) || []
    };
  },
  methods: {
    addProduct() {
      if (this.newProduct.trim() !== '') {
        this.products.push({name: this.newProduct, crossed: false});
        this.newProduct = '';
        this.saveProducts();
      }
    },
    crossOut(product) {
      product.crossed = !product.crossed;
      this.products.sort((a, b) => (a.crossed === b.crossed) ? 0 : a.crossed ? 1 : -1);
      this.saveProducts();
    },
    removeProduct(index) {
      this.products.splice(index, 1);
      this.saveProducts();
    },
    saveProducts() {
      localStorage.setItem('products', JSON.stringify(this.products));
    },
    clearList() {
      localStorage.removeItem('products');
      this.products = [];
    }
  }
};
</script>

<style>

#app {
  margin: 0;
  padding: 0;
}

body {
  margin: 0;
  padding: 0;
  font-family: Helvetica, sans-serif;
  user-select: none;
}

.main {
  margin: 10px auto;
  padding: 0;
  display: flex;
  flex-direction: column;
  width: calc(100% - 20px);
  max-width: 400px;
  max-height: 100dvh;
}

.heading {
  padding: 0;
  margin: 0 0 20px 0;
  text-align: center;
  font-size: 32px;
}

.crossed-out {
  text-decoration: line-through;
  color: #ccc;
}

.list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.list-item {
  display: flex;
  align-items: center;
  margin: 0;
  font-size: 24px;
  justify-content: space-between;
  border-bottom: 1px #ccc solid;
  box-sizing: border-box;
  line-height: 1;
  padding: 10px 0;
}

.list-item:last-of-type {
  border: none;
}

.list-item button {
  display: none;
  border: none;
  background-color: unset;
  color: #ff0000;
  box-sizing: border-box;
}

.list-item:hover button {
  display: flex;
}

.add-container {
  display: flex;
}

.add-input {
  border: none;
  font-size: 24px;
  padding: 0;
  width: 100%;
  text-overflow: ellipsis;
}

.add-input:focus {
  border: none;
  outline: none;
}

.add-button {
  border: none;
  background: none;
  font-size: 16px;
}

.clear-button {
  background: unset;
  border: none;
  padding: 0;
  width: fit-content;
  margin: 0 auto;
  cursor: pointer;
}

.clear-button:hover {
  border-bottom: 1px #000 solid;
}

</style>
