<template>
  <main>
    <div class="container-fluid c-section">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-spacing-top-medium"></div>
          <h2>Ajouter une catégorie</h2>
          <form>
            <div class="a-spacing-top-medium">
              <label>Type</label>
              <input
                type="text"
                class="a-input-text"
                style="width: 100%"
                v-model="type"
              />
            </div>
            <hr />
            <div class="a-spacing-top-large">
              <span class="a-button-register">
                <span class="a-button-inner">
                  <span class="a-button-text" @click="onAddcategory"
                    >Ajouter catégorie</span
                  >
                </span>
              </span>
            </div>
          </form>
          <br />
          <ul class="list-group-item">
            <li v-for="categorie in categories" :key="categorie._id">
              {{ categorie.type }}
            </li>
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
      let response = await $axios.$get("http://localhost:3000/api/categories");
      return {
        categories: response.categories
      };
    } catch (error) {
      console.log(error);
    }
  },
  data() {
    return {
      type: ""
    };
  },
  methods: {
    async onAddcategory() {
      try {
        let data = { type: this.type };
        let response = await this.$axios.$post(
          "http://localhost:3000/api/categories",
          data
        );
        this.categories.push(data);
        this.type = "";
      } catch (error) {}
    }
  }
};
</script>
