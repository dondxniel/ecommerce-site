<template>
  <q-page class="q-px-xl q-py-sm bg-grey-2">
    <div class="secondary text-h4 text-bold q-my-sm">Order Id</div>

    <div class="main">
      <div class="column q-gutter-y-md">
        <div class="border-radius bg-white q-px-md q-py-lg">
          <div class="text-bold text-h6">Order Id: #{{ order.txn_id }}</div>
          <div class="text-subtitle1">Order Date: 11/11/2022</div>
          <q-btn-dropdown
            color="secondary"
            outline
            label="Order Status"
            no-caps=""
            class="bordered-btn text-caption text-bold q-mt-md"
          >
            <q-list>
              <q-item clickable v-close-popup @click="onItemClick">
                <q-item-section>
                  <q-item-label>Shipping</q-item-label>
                </q-item-section>
              </q-item>

              <q-item clickable v-close-popup @click="onItemClick">
                <q-item-section>
                  <q-item-label>Delivered</q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
          </q-btn-dropdown>
        </div>

        <div class="border-radius bg-white q-pa-lg">
          <div class="q-my-sm text-bold text-h6">Order details</div>

          <img class="image" :src="itemImage" />

          <div class="row q-my-sm">
            <div class="text-bold">Item :</div>
            <div class="q-ml-sm text-subtitle2 text-weight-light">
              {{ itemName }}
            </div>
          </div>
          <div class="row q-my-sm">
            <div class="text-bold">Quantity :</div>
            <div class="q-ml-sm text-subtitle2 text-weight-light">
              {{ itemQuantity }}
            </div>
          </div>
          <div class="row q-my-sm">
            <div class="text-bold">Price per Item :</div>
            <div class="q-ml-sm text-subtitle2 text-weight-light">
              N {{ itemPrice }}
            </div>
          </div>
          <div class="row q-my-sm">
            <div class="text-bold">Total Price :</div>
            <div class="q-ml-sm text-subtitle2 text-weight-light">
              N {{ itemPrice * itemQuantity }}
            </div>
          </div>
          <div class="row q-my-sm">
            <div class="text-bold">Transaction ref :</div>
            <div class="q-ml-sm text-subtitle2 text-weight-light">
              {{ order.txn_ref }}
            </div>
          </div>
          <div class="row q-my-sm">
            <div class="text-bold">Transaction Status :</div>
            <div class="q-ml-sm text-subtitle2 text-weight-light">
              {{ order.txn_status }}
            </div>
          </div>
        </div>
      </div>

      <div class="column q-gutter-y-md">
        <div class="product-price border-radius bg-white q-pa-md">
          <div class="text-subtitle1 text-bold q-my-sm">Customer Details</div>
          <div>{{ buyerFullname }}</div>
          <div>{{ buyerEmail }}</div>
          <div>{{ order.shippingAddress }}</div>
        </div>

        <div class="product-price border-radius bg-white q-pa-md">
          <div class="text-subtitle1 text-bold q-my-sm">Shipping timeline</div>
          pending 1:00pm <br />
          delivered 2:00pm
        </div>
      </div>
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
      buyer: null,
      order: {},
      cart: null,
      itemImage: "",
      itemPrice: "",
      itemName: "",
      itemQuantity: "",
      buyerFullname: "",
      buyerEmail: "",

      // Has to be a nested object
      images: [],
      description: null,
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
        productName: this.product.name,
        productPrice: this.product.price,
        productQty: this.product.qtyInStore,
        productCategory: this.product.categories,
        productSubcategory: "nothing",
        productColor: this.description,
        productSize: this.discount,
      };
      this.$store.dispatch("moduleExample/updateSellerProductData", product);
      console.log(product);
    },
    getSingleSellerOrder() {
      this.$store
        .dispatch("moduleExample/getSingleSellerOrder", this.id)
        .then((response) => {
          console.log(response);
          this.order = response;
          this.itemImage = JSON.parse(response.cart)[0].product.img[0].url;
          this.itemName = JSON.parse(response.cart)[0].product.name;
          this.itemPrice = JSON.parse(response.cart)[0].product.price;
          this.itemQuantity = JSON.parse(response.cart)[0].quantity;
          this.getSinglePerson(response.buyer);
        });
    },
    getSinglePerson(id) {
      this.$store
        .dispatch("moduleExample/getSingleSeller", id)
        .then((response) => {
          console.log(response);
          this.buyerFullname = response.fullname;
          this.buyerEmail = response.email;
        });
    },
  },
  mounted() {
    this.id = location.href.split("order/")[1];
    this.getSingleSellerOrder();
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
  grid-template-columns: repeat(4, 1fr);
  gap: 15px;
}

.image {
  width: 90px;
  height: 90px;
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
