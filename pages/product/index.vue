 <template lang="">
  <div>
    <search  @sendKeyword="getKeyword"/>
    <ListProduct :dataProducts="dataProducts"  @getListProducts="getData"/>
  </div>
</template>
<script>
import { URL_API } from "@/constant/constant";
import axios from "axios";
import Search from "@/components/common/Search";
import ListProduct from "~/components/product/ListProduct";
export default {
  components: {
    ListProduct,
    Search,
  },
  data() {
    return {
      dataProducts: [],
      search: "",
    };
  },
  methods: {
    /**
     * get list product
     */
    getData() {
      axios.get(URL_API + "product").then((res) => {
        this.dataProducts = res.data.data;
        // this.page = res.data
        console.log(this.dataProducts);
      });
    },

    /**
     * get key search
     */
    getKeyword(value) {
      this.search = value;
      axios.get(URL_API + "product?name=" + this.search).then((res) => {
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
