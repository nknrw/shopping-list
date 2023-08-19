<template xmlns='http://www.w3.org/1999/html'>
  <div class='main'>
    <h1 class='heading'>Список продуктов</h1>
    <div class='add-container'>
      <input v-model='newProduct' @keyup.enter='addProduct' placeholder='Введите название продукта' class='add-input'>
    </div>
    <ul class='list'>
      <li v-for='(product, index) in products'
          :key='index'
          class='list-item'
          @click='crossOut(product)'>
        <span :class="{ 'crossed-out': product.crossed }" class='list-item_text'>{{ product.name }}</span>
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
        this.products.unshift({name: this.newProduct, crossed: false});
        this.newProduct = '';
        this.saveProducts();
      }
    },
    crossOut(product) {
      product.crossed = !product.crossed;
      this.products.sort((a, b) => (a.crossed === b.crossed) ? 0 : a.crossed ? 1 : -1);
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

html {
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
  margin: 0 auto;
  padding: 0;
  display: flex;
  flex-direction: column;
  width: calc(100% - 20px);
  max-width: 400px;
}

.heading {
  padding: 0;
  margin: 20px 0;
  text-align: center;
  font-size: 32px;
}

.crossed-out {
  text-decoration: line-through;
  color: #808080;
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
  width: 100%;
  border-bottom: 1px #808080 solid;
  box-sizing: border-box;
  line-height: 1;
  padding: 10px 0;
  cursor: pointer;
}

.list-item_text {
  width: 100%;
  inline-size: 100%;
  overflow-wrap: break-word;
  hyphens: auto;
}

.list-item:last-of-type {
  border: none;
}

.add-container {
  display: flex;
}

.add-input {
  border: none;
  font-size: 24px;
  padding: 10px 0;
  width: 100%;
  text-overflow: ellipsis;
  border-bottom: 1px #808080 solid;
  background-color: transparent;
}

.add-input:focus {
  outline: none;
}

.clear-button {
  background: unset;
  border: none;
  padding: 0;
  width: fit-content;
  margin: 20px auto;
  cursor: pointer;
}

</style>
