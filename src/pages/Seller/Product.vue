<template>
  <q-page class="q-pa-xl">
    <div class="orange text-h4 text-bold q-my-sm">Products</div>
    <div class="row justify-between q-mb-md">
      <div class="row q-gutter-x-md">
        <q-btn
          flat
          label="Add New Product"
          class="bg-primary bordered-btn"
          icon="add"
          text-color="white"
          no-caps
          to="/seller/product/new"
        />
        <!-- <q-btn
          outline
          color="orange"
          class="bordered-btn"
          label="Bulk Edit All"
          no-caps
        />
        <q-btn
          outline
          label="Import or Export Products"
          class="bordered-btn"
          no-caps
        /> -->
      </div>

      <!-- <q-btn
        icon="delete_forever"
        label="Delete Product"
        no-caps
        outline
        color="red"
        class="bordered-btn"
      /> -->
    </div>

    <!-- <div class="row q-mb-md q-gutter-x-sm">
      <q-btn-dropdown
        color="orange"
        outline
        no-caps=""
        label="Filter"
        class="bordered-btn filter"
      >
        <q-list>
          <q-item clickable v-close-popup @click="onItemClick">
            <q-item-section>
              <q-item-label>Photos</q-item-label>
            </q-item-section>
          </q-item>

          <q-item clickable v-close-popup @click="onItemClick">
            <q-item-section>
              <q-item-label>Videos</q-item-label>
            </q-item-section>
          </q-item>

          <q-item clickable v-close-popup @click="onItemClick">
            <q-item-section>
              <q-item-label>Articles</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-btn-dropdown>

      <q-input
        placeholder="Search Your Product Name"
        class="search"
        dense
        outlined
      >
        <template v-slot:prepend>
          <q-icon name="search" />
        </template>
      </q-input>
    </div> -->

    <ProductItem
      v-for="product in products"
      :key="product"
      :product="product"
    />
  </q-page>
</template>

<script>
import { ref } from "vue";
import Addproduct from "components/Seller/Addproduct.vue";
import ProductItem from "src/components/Seller/Product/ProductItem.vue";

export default {
  name: "Products",
  components: {
    Addproduct,
    ProductItem,
  },
  data() {
    return {
      icon: ref(false),
      products: [],
      id: "",
      skeleton: ref(true),
      noProduct: ref(false),
    };
  },
  methods: {
    addProduct(product) {
      if (product !== undefined) {
        this.$store.dispatch("moduleExample/addProduct", product).then(() => {
          this.icon = false;
          this.getProducts();
        });
      }
    },
    getProducts() {
      this.$store
        .dispatch("moduleExample/getSellerProducts")
        .then((response) => {
          console.log(response);
          this.products =
            this.$store.getters["moduleExample/getSellerProducts"];
          this.skeleton = false;

          if (!response[0]) {
            this.skeleton = false;
            this.noProduct = true;
          }
        });
    },
    deleteProduct(id) {
      this.$store.dispatch("moduleExample/deleteProduct", id);
    },
  },
  mounted() {
    this.getProducts();
  },
};
</script>

<style>
.filter {
  width: 15% !important;
}
.search {
  width: 80% !important;
}
.hide-skeleton {
  display: none;
}
.addProduct {
  width: 100%;
  max-width: 100%;
}
@media screen and (min-width: 1024px) {
  .addProduct {
    width: 70% !important;
  }
}
p {
  display: inline;
}

.my-menu-link {
  color: blue;
}

.list-boxes {
  height: 50px;
  width: 100%;
  border: 2px solid blue;
  border-radius: 4px;
  padding: 3% 1.5%;
}

.container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 15px;
}
.product-title {
  width: 80%;
  text-overflow: ellipsis;
  display: inline;
  overflow-y: hidden;
}

.product-card {
  min-height: 370px;
  height: 300px;
}

@media screen and (max-width: 1175px) {
  .container {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media screen and (max-width: 900px) {
  .container {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media screen and (max-width: 831px) {
  .container {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media screen and (max-width: 660px) {
  .container {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media screen and (max-width: 430px) {
  .container {
    grid-template-columns: repeat(1, 80%);
    place-content: center;
  }
  .card {
  }
}

.card {
  width: 100%;
}

.title {
  visibility: visible;
}
</style>
