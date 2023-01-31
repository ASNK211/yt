<template style="background-color: #02376b">
  <div style="background-color: #02376b">
    <strong>last news</strong>
    <div
      style="background-color: #02376b; color: aliceblue"
      v-for="product in products"
      :key="product._id"
      class="w-full flex flex-col justify-between rounded-lg py-5 px-4"
    >
      <ul>
        <li style="color: aliceblue">
          <span> {{ product.article }}</span>
        </li>
      </ul>
      <button @click="returns" style="color: darkorange; margin: 20px">
        Done
      </button>
    </div>
  </div>
</template>

<script>
import AuthenticationServices from "@/services/AuthenticationServices";
export default {
  data() {
    return {
      products: [],
      id: "",
      isLoading: true,
      urlimage: "",
    };
  },
  async created() {
    this.id = this.$route.params.id;
    const respons = await AuthenticationServices.getalladminProduct();
    this.products = respons.data.products;
    const response = await AuthenticationServices.updatedadminProduct();
    this.product = response.data;
  },
  methods: {
    returns() {
      this.$router.go(-1);
    },
  },
};
</script>

<style scoped>
body {
  font-family: "Lato", sans-serif;
}
span {
  width: 150px;
  word-wrap: break-word;
}
</style>
