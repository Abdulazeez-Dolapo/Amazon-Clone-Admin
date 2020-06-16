<template>
  <main>
    <div class="container-fluid c-section">
      <div class="row">
        <div class="col-sm-3"></div>

        <div class="col-sm-6">
          <div class="a-spacing-top-medium"></div>
          <h2 class="text-center">
            Add a new category
          </h2>

          <form>
            <div class="a-spacing-top-medium">
              <label for="">Type</label>
              <input
                style="width: 100%"
                type="text"
                v-model.trim="type"
                class="a-input-text"
              />
            </div>

            <!-- button -->
            <hr />
            <div class="a-spacing-top-large">
              <span class="a-button-register">
                <span class="a-button-inner">
                  <span class="a-button-text" @click="addCategory">
                    Add category
                  </span>
                </span>
              </span>
            </div>
          </form>
          <br />
          <ul>
            <li
              class="list-group-item"
              v-for="(category, i) in categories"
              :key="i"
            >
              {{ category.type }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    try {
      const { categories } = await $axios.$get(
        `http://localhost:3000/api/categories`
      );
      return {
        categories
      };
    } catch (error) {
      console.log(error.response);
    }
  },
  data() {
    return {
      type: ""
    };
  },
  methods: {
    async addCategory() {
      try {
        let data = { type: this.type };
        let response = await this.$axios.$post(
          `http://localhost:3000/api/category`,
          data
        );
        this.categories.push(data);

        // this.$router.push("/");
      } catch (error) {
        console.log(error.response);
      }
    }
  }
};
</script>

<style lang="scss" scoped></style>
