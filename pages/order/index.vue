 <template lang="">
  <div>
    <search  @sendKeyword="getKeyword"/>
    <ListOrder :dataOrders="dataOrders"  @getListOrders="getData"/>
  </div>
</template>
<script>
import { URL_API } from "@/constant/constant";
import axios from "axios";
import Search from "@/components/common/Search";
import ListOrder from "~/components/order/ListOrder";
export default {
  components: {
    ListOrder,
    Search,
  },
  data() {
    return {
      dataOrders: [],
      search: "",
    };
  },
  methods: {
    /**
     * get list order
     */
    getData() {
      axios.get(URL_API + "order").then((res) => {
        this.dataOrders = res.data.data;
        // this.page = res.data
        console.log(this.dataOrders);
      });
    },

    /**
     * get key search
     */
    getKeyword(value) {
      this.search = value;
      axios.get(URL_API + "order?name=" + this.search).then((res) => {
        this.dataProducts = res.data;
        console.log(res.data);
        // this.page = res.data;
      });
    },
  },
  mounted() {
    this.getData();
  },
};
</script>
<style lang="">
</style>
