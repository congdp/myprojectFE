 <template lang="">
  <div>
    <search  @sendKeyword="getKeyword"/>
    <ListProduct :dataProducts="dataProducts"  @getListProducts="getData"/>
  </div>
</template>
<script>
import axios from "axios";
import Search from "@/components/common/Search"
import ListProduct from "~/components/product/ListProduct";
export default {
  components: {
    ListProduct,
    Search
  },
  data() {
    return {
      dataProducts: [],
      search:"",
    };
  },
  methods: {
    
    /**
     * get list product
     */
    getData() {
      axios.get("http://127.0.0.1:8000/api/product").then((res) => {
        this.dataProducts = res.data;
        // this.page = res.data

      });
    },
      getKeyword(value){
      this.search = value;
      axios.get("http://localhost:8000/api/product?name="+this.search).then(res => {
        this.dataProducts = res.data;
        console.log(res.data);
        // this.page = res.data;
      });
    }
  },
  mounted() {
    this.getData();
  },
};
</script>
<style lang="">
</style>
