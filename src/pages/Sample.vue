<template>
  <q-page class="q-px-xl q-py-sm bg-grey-2">
    <div class="secondary text-h4 text-bold q-my-sm">Add New Product</div>

    <div class="main">
      <div class="column q-gutter-y-md">
        <div class="border-radius bg-white">
          <q-uploader
            url="http://localhost:4444/upload"
            label="Upload Images"
            multiple
            batch
            style="max-width: 300px"
          />
          <!-- <div class="images q-py-md q-px-lg">

            <div
              class="input border-radius column flex-center relative-position"
            >
              <v-input
                type="file"
                multiple
                ref="userAvatar"
                style="display: none"
                v-model="pictures"
              />
              <q-icon
                name="add"
                size="3.5rem"
                @click="$refs.userAvatar.click()"
                color="primary"
              />
              <div class="text-primary absolute-bottom text-center q-mb-md">
                Upload Images
              </div>
            </div>

            <div class="image border-radius flex flex-center">Main Image</div>
            <div class="image border-radius flex flex-center">
              Different Angle
            </div>
            <div class="image border-radius flex flex-center">Close Up</div>
            <div class="image border-radius flex flex-center">Detailed</div>
          </div> -->
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
export default {
  data() {
    return {
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
    addProduct() {
      let formData = new FormData();
      formData.append("productName", this.productName);
      formData.append("price", this.price);
      formData.append("quantity", this.quantity);
      formData.append("category", this.category);
      formData.append("subcategory", this.subcategory);
      formData.append("img", this.img);
      formData.append("color", this.description);
      formData.append("size", this.discount);
      console.log(formData);
      if (
        this.productName !== "" &&
        this.price !== "" &&
        this.quantity !== ""
      ) {
        this.$emit("addProduct", { formData });
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
    click() {
      console.log(this.pictures);
    },
  },
  mounted() {
    this.getCategories();
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
  grid-template-columns: repeat(5, 1fr);
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
</style>

<template>
  <div class="container">
    <div
      v-for="image in images"
      :key="image"
      class="previewBlock"
      @click="chooseFile"
      :style="{ 'background-image': `url(${image})` }"
    ></div>

    <div>
      <input
        class="form-control form-control-lg"
        ref="fileInput"
        multiple
        type="file"
        id="formFileLg"
        @input="selectImgFile"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      filePreview: null,
      arr: [],
      images: [],
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
          this.images.push(a);
        };
        reader.readAsDataURL(this.arr[i]);
        this.$emit("fileInput", this.arr[i]);
      }
    },
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
</style>
