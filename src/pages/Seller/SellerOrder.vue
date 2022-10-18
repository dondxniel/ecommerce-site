<template>
  <q-page class="q-px-xl">
    <div class="orange text-h4 text-bold q-my-sm">Orders</div>

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

    <!-- <div class="row justify-between q-mb-md">
      <q-space />

      <q-btn
        icon="delete_forever"
        label="Delete Order"
        no-caps
        outline
        color="red"
        class="bordered-btn"
      />
    </div> -->

    <q-list
      bordered
      padding
      class="list q-py-sm"
      v-for="order in orders"
      :key="order._id"
      separator
    >
      <q-item class="q-ml-sm">
        <q-item-section top thumbnail>
          <q-checkbox v-model="val" dense="" />
        </q-item-section>

        <q-item-section>
          <div class="q-mb-md">
            <div class="q-mb-sm text-bold text-subtitle1">
              Order Id: #{{ order.txn_id }}
            </div>
            <q-item-label class="text-bold text-subtitle1 items-center row">
              {{ order.createdAt.split("T")[0] }}
              <span
                class="text-weight-light text-primary q-ml-md text-subtitle2"
              >
                {{ order.shipStatus }}
              </span>
            </q-item-label>
          </div>

          <div class="row">
            <img
              class="image"
              :src="JSON.parse(order.cart)[0].product.img[0].url"
            />
            <div>
              <q-item-label class="text-bold text-subtitle1">
                {{ JSON.parse(order.cart)[0].product.name }}
              </q-item-label>

              <q-item-label class="text-subtitle2"
                >Price:
                {{ JSON.parse(order.cart)[0].product.price }}</q-item-label
              >
              <q-item-label class="text-subtitle2">
                Qty: {{ JSON.parse(order.cart)[0].quantity }}
              </q-item-label>
            </div>
          </div>
        </q-item-section>

        <q-item-section side top class="text-h6 text-bold text-orange">
          <q-item-label>
            <q-btn-dropdown
              color="orange"
              outline
              label="Order Status"
              no-caps
              class="bordered-btn text-caption text-bold"
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
            </q-btn-dropdown></q-item-label
          >
        </q-item-section>

        <q-item-section side top>
          <q-item-label> </q-item-label>
        </q-item-section>

        <q-item-section side>
          <q-btn
            flat
            icon="chevron_right"
            size="1.8rem"
            :to="`/seller/order/${order._id}`"
            class="non-hover-btn"
          />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
import { ref } from "vue";
import OrderDialog from "src/components/Seller/OrderDialog.vue";
import OrderItem from "src/components/Seller/Order/OrderItem.vue";

export default {
  components: {
    OrderDialog,
    OrderItem,
  },
  data() {
    return {
      orders: [],
      open: ref(false),
      order: {},
      cart: {},
      buyer: "",
      skeleton: ref(true),
      cart: [],
      val: false,
    };
  },

  methods: {
    getOrders() {
      this.$store.dispatch("moduleExample/getSellerOrders").then((response) => {
        console.log(response);
        this.orders = response;
        if (!response[0]) {
          this.skeleton = false;
        }
        this.skeleton = false;
        console.log(this.orders);
      });
    },
    item(item) {
      let a = JSON.parse(item);
      return a.quantity;
    },
  },
  mounted() {
    this.getOrders();
  },
};
</script>

<style scoped>
.filter {
  width: 15% !important;
}
.search {
  width: 80% !important;
}
.hide-skeleton {
  display: none;
}
.heading {
  font-size: 1.3rem;
}

.list:hover {
  background-color: rgb(0, 0, 0, 0.1);
}

.disable-text {
  color: grey !important;
}

.disabled-img {
  -webkit-filter: grayscale(100%);
  filter: grayscale(100%);
}

.image {
  height: 50px;
  width: 70px;
  margin-right: 3%;
}

@media screen and (min-width: 771px) {
  .heading {
    display: none;
  }
}

@media screen and (max-width: 601px) {
  .padding {
    padding: 0 5%;
  }
}

@media screen and (max-width: 421px) {
  .heading {
    font-size: 1.1rem;
  }

  .head_text {
    font-size: 0.84rem !important;
  }

  .body_text {
    font-size: 0.7rem;
  }

  .icon {
    font-size: 0.7rem !important;
  }
}
</style>
