<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>

        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 class="text-center">
              Add a new product
            </h2>

            <form action="">
              <!-- category dropdown -->
              <div class="a-spacing-top-medium">
                <label for="">Category</label>
                <select
                  name=""
                  id=""
                  v-model="categoryID"
                  class="a-select-option"
                >
                  <option
                    v-for="category in categories"
                    :value="category._id"
                    :key="category._id"
                    >{{ category.type }}</option
                  >
                </select>
              </div>

              <!-- owner dropdown -->
              <div class="a-spacing-top-medium">
                <label for="">Owner</label>
                <select name="" id="" v-model="ownerID" class="a-select-option">
                  <option
                    v-for="owner in owners"
                    :value="owner._id"
                    :key="owner._id"
                    >{{ owner.name }}</option
                  >
                </select>
              </div>

              <!-- title -->
              <div class="a-spacing-top-medium">
                <label for="">Title</label>
                <input
                  style="width: 100%"
                  type="text"
                  v-model.trim="title"
                  class="a-input-text"
                />
              </div>

              <!-- price -->
              <div class="a-spacing-top-medium">
                <label for="">Price</label>
                <input
                  style="width: 100%"
                  type="number"
                  v-model.number="price"
                  class="a-input-text"
                />
              </div>

              <!-- stock quantity -->
              <div class="a-spacing-top-medium">
                <label for="">Stock Quantity</label>
                <input
                  style="width: 100%"
                  type="number"
                  v-model.number="stockQuantity"
                  class="a-input-text"
                />
              </div>

              <!-- description -->
              <div class="a-spacing-top-medium">
                <label for="">Description</label>
                <textarea
                  placeholder="Provide details of the product"
                  style="width: 100%"
                  type="text"
                  v-model.trim="description"
                  class="a-input-text"
                />
              </div>

              <!-- photo -->
              <div class="a-spacing-top-medium">
                <label for="" style="margin-buttom: 0px;">Add photo</label>
                <div class="a-row a-spacing-top-medium">
                  <label class="choosefile-button" for="">
                    <i class="fal fa-plus"></i>
                    <p style="margin-top: -70px">
                      {{ fileName }}
                    </p>
                  </label>
                  <input type="file" @change="onFileSelected" name="" id="" />
                </div>
              </div>

              <!-- button -->
              <hr />
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="addProduct">
                      Add product
                    </span>
                  </span>
                </span>
              </div>
            </form>
          </div>
        </div>

        <div class="col-sm-3"></div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    try {
      const categories = $axios.$get(`http://localhost:3000/api/categories`);
      const owners = $axios.$get(`http://localhost:3000/api/owners`);

      const [categoryResponse, ownerResponse] = await Promise.all([
        categories,
        owners
      ]);

      console.log(categoryResponse, ownerResponse);

      return {
        categories: categoryResponse.categories,
        owners: ownerResponse.owners
      };
    } catch (error) {
      console.log(error.response);
    }
  },
  data() {
    return {
      ownerID: null,
      categoryID: null,
      title: "",
      price: 0,
      description: "",
      fileName: "",
      selectedFile: null,
      stockQuantity: 1
    };
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      console.log(this.selectedFile);
      this.fileName = event.target.files[0].name;
    },
    async addProduct() {
      let data = new FormData();
      data.append("title", this.title);
      data.append("price", this.price);
      data.append("description", this.description);
      data.append("categoryID", this.categoryID);
      data.append("ownerID", this.ownerID);
      data.append("stockQuantity", this.stockQuantity);
      data.append("photo", this.selectedFile, this.selectedFile.name);

      let result = await this.$axios.$post(
        `http://localhost:3000/api/product`,
        data
      );

      this.$router.push("/");
    }
  }
};
</script>

<style></style>
