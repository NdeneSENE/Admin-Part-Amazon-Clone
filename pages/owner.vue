<template>
  <main>
    <div class="container-fluid c-section">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-spacing-top-medium"></div>
          <h2>Ajouter un proprietaire</h2>
          <form>
            <div class="a-spacing-top-medium">
              <label>Name</label>
              <input type="text" class="a-input-text" style="width: 100%" v-model="name" />
            </div>
            <div class="a-spacing-top-medium">
              <label>A propos</label>
              <input type="text" class="a-input-text" style="width: 100%" v-model="about" />
            </div>
            <div class="a-spacing-top-medium">
              <label>Photo</label>
              <div class="a-row a-spacing-top-medium">
                <label class="choosefile-button">
                  <i class="fal fa-plus"></i>
                  <input type="file" @change="onFileSelected" />
                  <p style="margin-top: -70px">{{ fileName }}</p>
                </label>
              </div>
            </div>
            <hr />
            <div class="a-spacing-top-large">
              <span class="a-button-register">
                <span class="a-button-inner">
                  <span class="a-button-text" @click="onAddOwner">Ajouter proprietaire</span>
                </span>
              </span>
            </div>
          </form>
          <br />
          <ul class="list-group-item">
            <li v-for="owner in owners" :key="owner._id">{{ owner.name }}</li>
          </ul>
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
      let response = await $axios.$get("http://localhost:3000/api/owners");
      return {
        owners: response.owners
      };
    } catch (error) {
      console.log(error);
    }
  },
  data() {
    return {
      name: "",
      about: "",
      selectedFile: null,
      fileName: ""
    };
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      console.log(this.selectedFile);

      this.fileName = event.target.files[0].name;
    },
    async onAddOwner() {
      let data = new FormData();
      data.append("name", this.name),
        data.append("about", this.about),
        data.append("photo", this.selectedFile, this.selectedFile.name);

      let result = await this.$axios.$post(
        "http://localhost:3000/api/owners",
        data
      );
      console.log(data);
      this.owners.push(this.name);
    }
  }
};
</script>
