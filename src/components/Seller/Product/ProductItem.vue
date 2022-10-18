<template>
  <q-list bordered padding class="list q-py-sm" separator>
    <q-item class="q-ml-sm">
      <q-item-section top thumbnail>
        <q-checkbox v-model="val" dense="" />
      </q-item-section>
      <img
        class="image"
        :class="toggle === 'Disabled' ? 'disabled-img' : ''"
        :src="product.img[0].url"
      />

      <q-item-section>
        <q-item-label
          :class="toggle === 'Disabled' ? 'disable-text' : ''"
          class="text-subtitle1 text-bold"
        >
          {{ product.name }}
          <span class="text-weight-light text-subtitle2 q-ml-sm">{{
            product.qtyInStore
          }}</span>
        </q-item-label>
        <q-item-label class="q-my-xs">
          <q-toggle
            :label="toggle"
            color="green"
            dense
            size="1.7rem"
            :class="toggle === 'Disabled' ? 'text-secondary' : 'text-green'"
            false-value="Disabled"
            true-value="Enabled"
            v-model="toggle"
          />
          <span class="text-weight-light q-my-auto q-ml-xs"> In Stock</span>
        </q-item-label>
        <q-item-label caption>1 Option</q-item-label>
      </q-item-section>

      <q-item-section
        side
        top
        :class="toggle === 'Disabled' ? 'disable-text' : ''"
        class="text-h6 text-bold text-orange"
      >
        <q-item-label>N{{ product.price }}.00</q-item-label>
      </q-item-section>

      <q-item-section
        side
        top
        :class="toggle === 'Disabled' ? 'disable-text' : ''"
      >
        <q-item-label>
          <q-btn
            color="orange"
            outline
            label="Edit Product"
            no-caps
            :to="`/seller/product/${product._id}`"
            class="bordered-btn text-caption text-bold"
            :class="toggle === 'Disabled' ? 'disable-text' : ''"
          >
          </q-btn
        ></q-item-label>
      </q-item-section>

      <q-item-section side>
        <q-btn
          flat
          icon="chevron_right"
          :to="`/seller/product/${product._id}`"
          class="non-hover-btn"
        />
      </q-item-section>
    </q-item>
  </q-list>
</template>

<script>
export default {
  name: "Product Item",
  props: ["product"],
  data() {
    return {
      val: false,
      toggle: "Enabled",
    };
  },
  mounted() {
    console.log(this.product);
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

.chevron_right:hover {
  background: none !important;
  outline: 0 !important;
}
.image {
  height: 110px;
  width: 110px;
  margin-right: 3%;
}
</style>
