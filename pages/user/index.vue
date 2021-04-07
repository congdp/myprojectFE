<template lang="">
  <div>
    <search @sendKeyword="getKeyword" />
    <list-user :dataUser="dataUser" :fields="fields" />
    <!-- <b-pagination
      v-show="page.last_page > 1"
      v-model="currentPage"
      :total-rows="page.total"
      :per-page="page.per_page"
      @page-click="getUser"
      prev-text="Prev"
      next-text="Next"
    ></b-pagination> -->
  </div>
</template>
<script>
import ListUser from "@/components/users/ListUser";
import Search from "../../components/common/Search";
import axios from "axios";
import { FIELDS } from "../../constant/constant";
export default {
  components: {
    ListUser,
    Search,
  },
  data() {
    return {
      dataUser: [],
      fields: FIELDS,
      page: {},
      currentPage: 1,
      search: "",
    };
  },
  methods: {
    /**
     * get all user in api
     */
    getUser() {
      axios.get("http://localhost:8000/api/all-user").then((res) => {
        this.dataUser = res.data;
        // this.page = res.data;
      });
    },

    /**
     * get key word search
     * @param String key word search
     */
    getKeyword(value) {
      this.search = value;
      axios
        .get("http://localhost:8000/api/search?username=" + this.search)
        .then((res) => {
          this.dataUser = res.data;
          // console.log(res.data);
          // this.page = res.data;
        });
    },
  },
  mounted() {
    this.getUser();
  },
};
</script>
<style lang=""></style>
