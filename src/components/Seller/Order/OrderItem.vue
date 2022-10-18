<template>
  <q-list bordered padding class="list q-py-sm" separator>
    <q-item class="q-ml-sm">
      <q-item-section top thumbnail>
        <q-checkbox v-model="val" dense="" />
      </q-item-section>

      <q-item-section>
        <div class="q-mb-xl">
          <div class="q-mb-sm text-bold text-subtitle1">
            Order Id: #{{ order.txn_id }}
          </div>
          <q-item-label class="text-bold text-subtitle1 items-center row">
            {{ order.createdAt.split("T")[0] }}
            <span class="text-weight-light text-primary q-ml-md text-subtitle2">
              {{ order.shipStatus }}
            </span>
          </q-item-label>

          <q-item-label class="text-subtitle1"
            >Customer's mail for the order
          </q-item-label>
        </div>

        <div class="row">
          <img class="image" src="https://placeimg.com/500/300/nature" />
          <div>
            <q-item-label class="text-bold text-subtitle1">
              {{ cart }}
            </q-item-label>

            <q-item-label class="text-subtitle2">Price: N4 000.00</q-item-label>
            <q-item-label class="text-subtitle2"> Qty: 2 </q-item-label>
          </div>
        </div>
      </q-item-section>

      <q-item-section side top class="text-h6 text-bold text-secondary">
        <!-- <q-item-label>N8 000.00</q-item-label> -->
        <q-item-label>
          <q-btn-dropdown
            color="secondary"
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
          to="/seller/order_detail"
          class="non-hover-btn"
        />
      </q-item-section>
    </q-item>
  </q-list>
</template>

<script>
export default {
  name: "Order Item",
  props: ["order", "cart"],
  data() {
    return {
      date: "",
    };
  },
  async mounted() {
    // console.log(await this.order);
    // this.cart = JSON.parse(this.order.cart);
    // console.log(this.cart);
  },
};
</script>

<style scoped>
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
</style>
