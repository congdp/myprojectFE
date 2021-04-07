<template>
  <CDropdown
    in-nav
    class="c-header-nav-items"
    placement="bottom-end"
    add-menu-classes="pt-0"
  >
    <template #toggler>
      <CHeaderNavLink>
        <div class="c-avatar">
          <img
            src=""
            class="c-avatar-img "
          >
        </div>
          <a href="">{{$auth.user.name}}</a>
      </CHeaderNavLink>
    </template>
    <CDropdownHeader tag="div" class="text-center" color="light">
      <strong>Account</strong>
    </CDropdownHeader>
    <CDropdownDivider />
    <CDropdownItem>
      <CIcon :content="$options.freeSet.cilShieldAlt" />  <nuxt-link :to="`/user/${$auth.user.id}`">info</nuxt-link>
    </CDropdownItem>
    <CDropdownItem>
      <CIcon :content="$options.freeSet.cilLockLocked" /> <CButton @click="logout">Logout</CButton>
    </CDropdownItem>
  </CDropdown>
</template>

<script>
import { freeSet } from "@coreui/icons";
export default {
  name: 'TheHeaderDropdownAccnt',
  freeSet,
  methods: {
    logout(){
      this.$auth.logout();
      this.$router.push('/login');
    }
  }
}
</script>

<style scoped>
  .c-icon {
    margin-right: 0.3rem;
  }
</style>
