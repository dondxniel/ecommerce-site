<template>
  <q-page class="q-px-xl q-py-sm bg-grey-2">
    <div class="secondary text-h4 text-bold q-my-sm">{{ product.name }}</div>

    <div class="main">
      <div class="column q-gutter-y-md">
        <div class="border-radius bg-white">
          <div class="images q-py-md q-px-lg">
            <div
              class="image border-radius flex flex-center"
              v-for="image in product.img"
              :key="image.key"
            >
              <q-img class="main_image" :src="image.url" />
            </div>

            <!-- <div
              class="input border-radius column flex-center relative-position"
            >
              <input type="file" ref="userAvatar" style="display: none" />
              <q-icon
                name="add"
                size="3.5rem"
                @click="$refs.userAvatar.click()"
                color="primary"
              />
              <div class="text-primary absolute-bottom text-center q-mb-md">
                Upload Images
              </div>
            </div> -->
          </div>
        </div>

        <div class="border-radius bg-white q-pa-lg">
          <div class="product-input">
            <div>
              <label class="text-grey-9">Name</label>
              <q-input class="q-mt-xs" outlined dense v-model="name" />
            </div>
            <div>
              <label class="text-grey-9">Quantity</label>
              <q-input class="q-mt-xs" outlined dense v-model="quantity" />
            </div>
            <div>
              <label class="text-grey-9">Weight</label>
              <q-input class="q-mt-xs" outlined dense />
            </div>
          </div>

          <div class="q-mt-lg textarea">
            <label for="">Description</label>
            <q-input
              type="textarea"
              autogrow
              class="description"
              placeholder="Please describe the product with details information..."
              outlined
              v-model="description"
            />
          </div>
        </div>
      </div>

      <div class="column q-gutter-y-md">
        <div class="product-price border-radius bg-white q-pa-md">
          <div class="text-h5 text-bold q-my-sm">Pricing</div>
          <q-input outlined v-model="price">
            <template v-slot:prepend>
              N <q-separator class="q-ml-md" vertical
            /></template>
          </q-input>
        </div>

        <div class="product-price border-radius bg-white q-pa-md">
          <div class="text-h5 text-bold q-my-sm">Category</div>
          <q-input
            outlined
            label="Category"
            readonly
            v-model="product.categories"
          >
          </q-input>
        </div>

        <div class="product-price border-radius bg-white q-pa-md">
          <div class="text-h5 text-bold q-my-sm">Discount</div>
          <q-input outlined v-model="discount">
            <template v-slot:append>
              <q-separator class="q-mr-md" vertical />
              %
            </template>
          </q-input>
        </div>
      </div>
    </div>
    <div class="row q-mt-lg">
      <q-space />
      <q-btn
        flat
        label="Update Product Details"
        class="bg-primary bordered-btn"
        text-color="white"
        @click="updateProductData()"
        no-caps
        size="1.1rem"
      />
      <q-space />
    </div>
  </q-page>
</template>

<script>
import { ref } from "vue";
export default {
  name: "details.vue",
  data() {
    return {
      imageModal: ref(false),
      newImage: null,
      slide: 1,
      tab: "Specifications",
      rating_point: 3.5,
      id: "",
      product: {},

      // Has to be a nested object
      images: [],
      name: null,
      quantity: null,
      description: null,
      price: null,
      discount: null,
    };
  },
  computed: {
    win_width() {
      return this.$q.screen.width - 59;
    },
    win_height() {
      return this.$q.screen.height - 0;
    },
  },
  methods: {
    changeImage(id, imageKey) {
      console.log(imageKey);
      let newImg = this.newImage;
      let formData = new FormData();
      formData.append("oldImgKey", imageKey);
      formData.append("img", newImg);
      this.$store.dispatch("moduleExample/updateImage", { id, formData });
    },
    updateProductData() {
      let product = {
        _id: this.id,
        productName: this.name,
        productPrice: this.price,
        productQty: this.quantity,
        productCategory: this.product.categories,
        productSubcategory: "nothing",
        productColor: this.description,
        productSize: this.discount,
      };
      this.$store.dispatch("moduleExample/updateSellerProductData", product);
      console.log(product);
    },
    getProduct() {
      this.$store
        .dispatch("moduleExample/getSingleSellerProduct", this.id)
        .then((response) => {
          console.log(response);
          this.product = response;
          let detail = this.product;
          this.images = response.img;
          this.description = detail.desc.color;
          this.name = detail.name;
          this.quantity = detail.qtyInStore;
          this.price = detail.price;
          this.description = detail.desc.color;
          this.discount = detail.desc.size;
        });
    },
  },
  mounted() {
    this.id = location.href.split("product/")[1];
    this.getProduct();
  },
};
</script>

<style scoped>
.main {
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: 25px;
}
.border-radius {
  border-radius: 10px;
}

.product-input {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr;
  gap: 8px;
}
.product-price {
  /* width: 30%; */
}

.images {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
}

.input,
.image {
  /* width: 150px; */
  height: 150px;
}
.input {
  border: 2px dashed #d56c33;
  cursor: pointer;
}
.input:hover {
  background: rgb(128, 128, 128, 0.1) !important;
}
.image {
  border: 2px solid rgb(128, 128, 128, 0.5);
  color: rgb(128, 128, 128, 0.5);
}
.description {
  height: 150px !important;
}
.main_image {
  height: 100%;
  width: 100%;
}

@media screen and (max-width: 830px) {
  .main {
    display: block;
  }
}
</style>
