 <template lang="">
  <div>
    <search  @sendKeyword="getKeyword"/>
    <ListCategory :dataCategory="dataCategory"  @getListCategory="getData"/>
  </div>
</template>
<script>
import { URL_API } from "~/constant/constant";
import axios from "axios";
import Search from "@/components/common/Search";
import ListCategory from "~/components/category/ListCategory";
export default {
  components: {
    ListCategory,
    Search,
  },
  data() {
    return {
      dataCategory: [],
      search: "",
    };
  },
  methods: {
    /**
     * get list category
     */
    getData() {
      axios.get(URL_API + "category").then((res) => {
        console.log(res.data);
        this.dataCategory = res.data.data;
      });
    },
    
    /**
     * get key search
     */
    getKeyword(value) {
      this.search = value;
      axios.get(URL_API + "category?name=" + this.search).then((res) => {
        this.dataCategory = res.data.data;
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
