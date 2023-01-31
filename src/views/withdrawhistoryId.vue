<template>
  <body class="bod" style="background-color: #cccccc">
    <div>
      <div class="over">
        <button
          @click="copyTestingCode"
          type="button"
          style="position: relative; left: 10%"
          class="py-2.5 px-2 mr-2 text-sm font-medium text-gray-900 bg-white rounded-lg border border-gray-200 hover:text-blue-700 focus:z-10 focus:ring-4 focus:outline-none focus:ring-blue-700 focus:text-blue-700 dark:bg-gray-800 dark:text-gray-400 dark:border-gray-600 dark:hover:text-white dark:hover:bg-gray-700 inline-flex items-center"
        >
          <div
            v-if="shows === 1"
            style="color: black; font-weight: 800; font-size: large"
          >
            <h1 class="text-center font-bold leading-10 text-gray-800">
              {{ senderusdtId }}
            </h1>
          </div>
        </button>
      </div>
      <input type="hidden" id="testing-code" :value="senderusdtId" />
      <div style="display: inline-block">withdraw history</div>
      <h1 class="text-center font-bold leading-10 text-gray-800">
        {{ users }}
      </h1>
      <div class="userId" v-for="order in orders" :key="order._id">
        {{ order.userId }}
      </div>
      <div class="inor">
        <div class="balance">
          <div style="font-weight: 800">amount</div>
          <h1
            v-for="user in orders"
            :key="user._id"
            class="text-center font-bold leading-10 text-gray-800"
          >
            {{ user.amount }}
          </h1>
        </div>
        <div class="profit">
          <div style="font-weight: 800">with tax</div>
          <h1
            v-for="user in orders"
            :key="user._id"
            class="text-center font-bold leading-10 text-gray-800"
          >
            {{ user.prs }}
          </h1>
        </div>
      </div>
    </div>
  </body>
</template>

<script>
import AuthenticationServices from "@/services/AuthenticationServices";

export default {
  name: "MyComponent",
  data() {
    return {
      orders: [],
      users: "",
      id: "",
      senderusdtId: "",
      shows: 1,
    };
  },
  async created() {
    this.id = this.$route.params.id;
    const orderId = this.id;
    const response = await AuthenticationServices.getonewithdrawhistory(
      orderId
    );
    this.orders = response.data;
    this.senderusdtId = response.data.orders.usdtId;
    this.users = response.data.orders.createdAt;
  },
  methods: {
    copyTestingCode() {
      try {
        let testingCodeToCopy = document.querySelector("#testing-code");
        testingCodeToCopy.setAttribute("type", "text"); // 不是 hidden 才能複製
        testingCodeToCopy.select();
        document.execCommand("copy");
      } catch (err) {
        alert("Oops, unable to copy");
      }
      //   this.$router.push(`/order`);
    },
    async deletedeposit() {
      const orderId = this.id;
      const response = await AuthenticationServices.deletedeposit(orderId);
      console.log(response);
    },
  },
};
</script>

<style scoped>
.over {
  border: 1px solid rgb(199, 196, 196);
  overflow: scroll;
  width: 206px;
  border-radius: 7px;
  position: relative;
  left: 30%;
  height: 60px;
}
#testing-code {
  position: absolute;
  left: -9999px;
}
.bod {
  height: 550px;
}
.userId {
  position: relative !important;
  top: 8px !important;
  font-size: large;
  font-weight: bolder;
}
.inor {
  position: relative !important;
  top: 35px !important;
}
.balance {
  display: inline-block;
  position: relative;
  right: 20%;
}
.profit {
  display: inline-block;
  position: relative;
  left: 20%;
}
</style>
