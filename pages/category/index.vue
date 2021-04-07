 <template lang="">
  <div>
    <search  @sendKeyword="getKeyword"/>
    <ListCategory :dataCategory="dataCategory"  @getListCategory="getData"/>
  </div>
</template>
<script>
import axios from "axios";
import Search from "@/components/common/Search"
import ListCategory from "~/components/category/ListCategory";
export default {
  components: {
    ListCategory,
    Search
  },
  data() {
    return {
      dataCategory: [],
      search:"",
    };
  },
  methods: {
    /**
     * get list blog
     */
    getData() {
      axios.get("http://127.0.0.1:8000/api/category").then((res) => {
        console.log(res.data)
        this.dataCategory = res.data;
        // this.page = res.data

      });
    },
      getKeyword(value){
      this.search = value;
      axios.get("http://localhost:8000/api/category?name="+this.search).then(res => {
        this.dataCategory = res.data;
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
