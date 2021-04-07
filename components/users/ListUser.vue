<template>
  <div>
    <CCard>
      <CCardHeader>
        <h3>List User</h3>
      </CCardHeader>
      <CCardBody>
        <CButton color="primary" class="m-2 btn_add">
          <nuxt-link to="/user/add" class="text-white d-block">
            + Add</nuxt-link
          >
        </CButton>
        <CDataTable
          :items="dataUser"
          :fields="fields"
          hover
          items-per-page-select
          pagination
        >
        <template #role_id="{item}">
            <td>
              {{getRoleName(item.role_id)}}
            </td>
        </template>
         <template #show_details="{ item, index }">
            <td class="py-2">
              <CButton
                color="primary"
                variant="outline"
                square
                size="sm"
                @click="toggleDetails(item, index),getTaskOfUser(item.id)"
              >
                {{ Boolean(item._toggled) ? "Hide" : "Show" }}
              </CButton>
            </td>
          </template>
           <template #details="{ item }">
            <CCollapse
              :show="Boolean(item._toggled)"
              :duration="collapseDuration"
            >
              <CCardBody>
                <h6>List Task</h6>
                <CDataTable 
                :items="listTasks"
                :fields="user_task"
                hover
                pagination
                striped
                >
                </CDataTable>
              </CCardBody>
            </CCollapse>
          </template>
        </CDataTable>
      </CCardBody>
    </CCard>
  </div>
</template>
<script>
const user_task = [
  { key: "id", label: "Id" },
  { key: "subject", label: "Name" },
  { key: "id_project", label: "Project" },
  { key: "content", label: "Content" },
  { key: "member_id", label: "Assignee" },
  { key: "status", label: "Status" },
  { key: "start_date", label: "Start Date" },
  { key: "due_date", label: "Due Date" },
 
];
import axios from "axios";
export default {
  name: "ListUser",
  props: {
    dataUser: {
      type: Array,
      default: () => [],
    },
    fields: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      dataUser: this.dataUser.map((item, id) => {
        return { item, id };
      }),
      dataRole: [],
      listTasks : [],
      details: [],
      collapseDuration: 0,
      user_task : user_task
    };
  },
  methods: {
    /**
     * get role in api
     */
    getRole(){
      axios.get('http://localhost:8000/api/roles').then(res=>{
        this.dataRole = res.data
        // console.log(this.dataRole);
      });
    },
    /**
     * get name of role 
     * @param Interger role_id of data user
     * @return String role name
     */
    getRoleName(id){
     return this.dataRole.filter(role =>{return role.id==id}).map(role => {return role.name}).join(',');
    },
    toggleDetails(item, index) {
      this.$set(this.dataUser[index], "_toggled", !item._toggled);
      this.collapseDuration = 300;
      this.$nextTick(() => {
        this.collapseDuration = 0;
      });
    },
    getTaskOfUser(id){
       axios.get("http://localhost:8000/api/user/get/"+id).then(res => {
        this.listTasks = res.data;
        console.log(res.data);
        // this.page = res.data;
      });
      
    }
  },
  mounted(){
    this.getRole();
    
  }
};
</script>
<style scoped>
</style>

