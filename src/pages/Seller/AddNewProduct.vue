<template>
  <q-page class="q-px-xl q-py-sm bg-grey-2">
    <div class="secondary text-h4 text-bold q-my-sm">Add New Product</div>

    <div class="main">
      <div class="column q-gutter-y-md">
        <div class="border-radius bg-white">
          <div class="images q-py-md q-px-lg">
            <div
              class="input border-radius column flex-center relative-position"
            >
              <input
                class="form-control form-control-lg"
                ref="fileInput"
                multiple
                type="file"
                @input="selectImgFile"
                style="display: none"
              />
              <!-- <v-input
                type="file"
                multiple
                ref="userAvatar"
                style="display: none"
                v-model="pictures"
              /> -->
              <q-icon
                name="add"
                size="3.5rem"
                @click="$refs.fileInput.click()"
                color="primary"
              />
              <div class="text-primary absolute-bottom text-center q-mb-md">
                Upload Images
              </div>
            </div>

            <div
              v-show="!images[0]"
              v-for="n in 3"
              :key="n"
              class="image border-radius flex flex-center"
            >
              Product Image
            </div>

            <div
              v-show="images[0]"
              v-for="image in images"
              :key="image"
              @click="chooseFile"
              :style="{ 'background-image': `url(${image})` }"
              class="image border-radius flex flex-center"
            ></div>
          </div>
        </div>

        <div class="border-radius bg-white q-pa-md">
          <div class="product-input">
            <div>
              <label class="text-grey-9">Name</label>
              <q-input class="q-mt-xs" v-model="productName" outlined dense />
            </div>
            <div>
              <label class="text-grey-9">Quantity</label>
              <q-input
                class="q-mt-xs"
                v-model.number="quantity"
                outlined
                dense
              />
            </div>
            <div>
              <label class="text-grey-9">Weight</label>
              <q-input class="q-mt-xs" outlined dense />
            </div>
          </div>

          <div class="q-mt-lg">
            <label for="">Description</label>
            <q-input
              v-model="description"
              type="textarea"
              placeholder="Please describe the product with details information..."
              outlined
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
          <div class="text-caption">
            Please select a cetegpry that fits this product
          </div>
          <q-select
            outlined
            v-model="category"
            :options="categories"
            label="Categories..."
          />
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
        @click="addProduct()"
        no-caps
        size="1.1rem"
      />
      <q-space />
    </div>
  </q-page>
</template>

<script>
import { ref } from "vue";
import { Notify } from "quasar";
export default {
  data() {
    return {
      filePreview: null,
      arr: [],
      images: [],
      colors: [],
      categories: [],
      productName: ref(""),
      price: ref(""),
      quantity: ref(""),
      category: ref(""),
      subcategory: ref("nothing"),
      img: ref(null),
      description: ref(""),
      discount: ref(""),
    };
  },
  methods: {
    chooseFile() {
      // this.$refs.fileInput.click();
    },
    selectImgFile() {
      let fileInput = this.$refs.fileInput;
      let imgFile = fileInput.files;
      for (let i = 0; i < imgFile.length; i++) {
        this.arr.push(imgFile[i]);
      }

      for (let i = 0; i < this.arr.length; i++) {
        let reader = new FileReader();
        let a;
        reader.onload = (e) => {
          a = e.target.result;
          if (!this.images.includes(a)) {
            this.images.push(a);
          }
        };
        reader.readAsDataURL(this.arr[i]);
        this.$emit("fileInput", this.arr[i]);
      }
      console.log(this.arr);
      console.log(this.images);
    },
    addProduct() {
      console.log("hello");
      let formData = new FormData();
      formData.append("productName", this.productName);
      formData.append("price", this.price);
      formData.append("quantity", this.quantity);
      formData.append("category", this.category);
      formData.append("subcategory", this.subcategory);
      for (let i = 0; i < this.arr.length; i++) {
        formData.append("img", this.arr[i]);
      }
      formData.append("color", this.description);
      formData.append("size", this.discount);
      console.log(formData);
      if (
        this.productName !== "" &&
        this.price !== "" &&
        this.quantity !== ""
      ) {
        this.$store
          .dispatch("moduleExample/addProduct", { formData })
          .then((response) => {
            console.log(response);
            // this.icon = false;
            // this.getProducts();
          });
      } else {
        Notify.create({
          message: "You can't leave any field empty.",
          color: "red",
        });
      }
    },
    getCategories() {
      this.$store.dispatch("moduleExample/getCategories").then((response) => {
        for (let item of response.categories) {
          this.categories.push(item.name);
        }
      });
    },
  },
  mounted() {
    this.getCategories();
  },
};
</script>

<style scoped lang="css">
.container {
  max-width: 600px;
}
.previewBlock {
  display: block;
  cursor: pointer;
  width: 350px;
  height: 200px;
  margin: 0 auto 20px;
  background-position: center center;
  background-size: cover;
}
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

.input,
.image {
  /* width: 150px; */
  height: 150px;
  background-position: center;
  background-size: contain;
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
</style>
