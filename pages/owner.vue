<template>
  <main>
    <div class="container-fluid c-section">
      <div class="row">
        <div class="col-sm-3"></div>

        <div class="col-sm-6">
          <div class="a-spacing-top-medium"></div>
          <h2 class="text-center">
            Add a new owner
          </h2>

          <form>
            <!-- name -->
            <div class="a-spacing-top-medium">
              <label for="">Name</label>
              <input
                style="width: 100%"
                type="text"
                v-model.trim="name"
                class="a-input-text"
              />
            </div>

            <!-- about -->
            <div class="a-spacing-top-medium">
              <label for="">About</label>
              <input
                style="width: 100%"
                type="text"
                v-model.trim="about"
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
                  <span class="a-button-text" @click="addOwner">
                    Add owner
                  </span>
                </span>
              </span>
            </div>
          </form>
          <br />
          <ul>
            <li class="list-group-item" v-for="(owner, i) in owners" :key="i">
              {{ owner.name }}
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
      const { owners } = await $axios.$get(`http://localhost:3000/api/owners`);
      return {
        owners
      };
    } catch (error) {
      console.log(error.response);
    }
  },
  data() {
    return {
      type: "",
      name: "",
      about: "",
      fileName: "",
      selectedFile: null
    };
  },
  methods: {
    async addOwner() {
      try {
        let data = new FormData();
        data.append("name", this.name);
        data.append("about", this.about);
        data.append("photo", this.selectedFile, this.selectedFile.name);

        let response = await this.$axios.$post(
          `http://localhost:3000/api/owner`,
          data
        );
        this.owners.push({ name: this.name });

        // this.$router.push("/");
      } catch (error) {
        console.log(error.response);
      }
    },
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      console.log(this.selectedFile);
      this.fileName = event.target.files[0].name;
    }
  }
};
</script>

<style lang="scss" scoped></style>
