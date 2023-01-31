<template>
  <div class="bg-gray-200 py-10">
    <form id="login" v-on:submit.prevent>
      <div class="bg-white dark:bg-gray-800">
        <div
          class="container mx-auto bg-white dark:bg-gray-800 mt-10 rounded px-4"
        >
          <div class="mx-auto pt-4">
            <div class="container mx-auto">
              <form class="my-6 w-11/12 mx-auto xl:w-full xl:mx-0">
                <div class="xl:w-1/4 lg:w-1/2 md:w-1/2 flex flex-col mb-6">
                  <label
                    for="message"
                    class="font-bold block mb-2 text-sm font-medium text-gray-900"
                  >
                    the article
                  </label>
                  <textarea
                    v-model="article"
                    id="message"
                    rows="4"
                    class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:border-gray-600 dark:placeholder-gray-400 dark:focus:ring-blue-500 dark:focus:border-blue-500"
                    placeholder="Leave a comment..."
                  ></textarea>
                </div>
              </form>
            </div>
          </div>
        </div>
        <div class="container mx-auto w-11/12 xl:w-full">
          <div
            class="w-full py-4 sm:px-0 bg-white dark:bg-gray-800 flex justify-end"
          >
            <button
              v-if="asor === 1"
              @click="adminProducts"
              class="bg-indigo-700 focus:outline-none transition duration-150 ease-in-out hover:bg-indigo-600 rounded text-white px-8 py-2 text-sm"
              type="submit"
            >
              Save
            </button>
            <button
              v-if="asor === 0"
              class="bg-indigo-700 focus:outline-none transition duration-150 ease-in-out hover:bg-indigo-600 rounded text-white px-8 py-2 text-sm"
              type="submit"
            >
              Save
            </button>
          </div>
        </div>
        <div
          class="container mx-auto mt-10 rounded bg-gray-100 dark:bg-gray-700 w-11/12 xl:w-full"
        >
          <div class="xl:w-full py-5 px-8">
            <div class="flex items-center mx-auto">
              <div class="container mx-auto">
                <div
                  v-for="product in products"
                  :key="product._id"
                  class="mx-auto xl:w-full"
                >
                  <p class="text-sm text-gray-500 dark:text-gray-400 pt-1">
                    {{ product.article }}
                  </p>
                  <button @click="deleteProduct(product._id)" class="remove">
                    remove
                  </button>
                  <!-- <button class="update">update</button> -->
                </div>
              </div>
            </div>
          </div>
          <div class="container mx-auto pb-6"></div>
        </div>
      </div>
    </form>
  </div>
</template>
<style>
.remove {
  background-color: #a11;
  border-radius: 5px;
  color: aliceblue;
  width: 70px;
  margin: 5px;
}
.update {
  background-color: #519312;
  border-radius: 5px;
  color: aliceblue;
  width: 70px;
  margin: 5px;
}
#xclose {
  position: relative;
  right: 35px;
  top: -86px;
}
.immudel {
  width: 80px;
}
</style>
<script>
import AuthenticationServices from "@/services/AuthenticationServices";
export default {
  name: "aboutView",
  data() {
    return {
      products: [],
      // product: { price: "", title: "", image: "" },
      activeItem: null,
      article: null,
      page: 1,
      perPage: 10,
      pages: [],
      title: null,
      alert: "",
      alerte: "",
      asor: 1,
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
    async adminProducts() {
      this.alerte = "";
      this.asor = 0;
      try {
        await AuthenticationServices.adminProduct({
          article: this.article,
        });
        window.location.reload();
      } catch (error) {
        this.error = error.response.data.error;
      }
    },
    async deleteProduct(_id) {
      await AuthenticationServices.dleteadminProduct(_id);
      window.location.reload();
    },
    paginate(products) {
      let page = this.page;
      let perPage = this.perPage;
      let from = page * perPage - perPage;
      let to = page * perPage;
      return products.slice(from, to);
    },
    setusers() {
      let numberOfPages = Math.ceil(this.products.length / this.perPage);
      for (let i = 1; i <= numberOfPages; i++) {
        this.pages.push(i);
      }
    },
  },
  computed: {
    displayproducts() {
      return this.paginate(this.products);
    },
  },
  watch: {
    products() {
      this.setusers();
    },
  },
};
</script>
