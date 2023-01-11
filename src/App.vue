<template>
  <div class="container">
    <UserDataTable :users="users" :totalPage="totalPage"></UserDataTable>

    <div class="mt-3 border rounded">
      <UserFormData @update:user="users = $event"></UserFormData>
    </div>
  </div>
</template>

<script>
import UserDataTable from "./components/UserDataTable.vue";
import UserFormData from "./components/UserFormData.vue";

export default {
  components: { UserDataTable, UserFormData },
  data: () => ({
    users: [],
  }),
  watch: {
    // users(newValue) {
    //   localStorage.setItem("users", JSON.stringify(newValue));
    // },
    users: {
      handler(newValue) {
        localStorage.setItem("users", JSON.stringify(newValue));
      },
      deep: true,
    },
  },
  mounted() {
    this.users = JSON.parse(localStorage.getItem("users") || "[]");
  },
};
</script>

<style lang="scss" scoped></style>
