<template>
  <table class="table">
    <thead>
      <tr>
        <th>รหัสพนักงาน</th>
        <th>ชื่อจริง - นามสกุล</th>
        <th>สาขา</th>
        <th>สีประจำพนักงาน</th>
        <th>วันที่เริ่มงาน</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="user of usersMapped">
        <td>{{ user.id }}</td>
        <td>{{ user.firstName }} {{ user.lastName }}</td>
        <td>{{ user.branch }}</td>
        <td class="text-center">
          <div
            class="user-color"
            :style="{ backgroundColor: user.color }"
          ></div>
        </td>
        <td>{{ user.startDate }}</td>
      </tr>
    </tbody>
  </table>

  <div class="d-flex align-items-start justify-content-between flex-wrap">
    <BootstrapPagination v-model="page" :page="totalPage"></BootstrapPagination>
    <select v-model="perPage" class="form-select" style="width: 150px">
      <option v-for="num of perPages" :value="num">{{ num }} ต่อหน้า</option>
    </select>
  </div>
</template>

<script>
import dayjs from "dayjs";
import BootstrapPagination from "./BootstrapPagination.vue";

export default {
  components: { BootstrapPagination },
  props: {
    users: {
      type: Array,
      required: true,
    },
    totalPage: {
      type: Function,
      required: true,
    },
  },
  data: () => ({
    page: 1,
    perPage: 5,
    perPages: [5, 10, 15, 25],
  }),
  computed: {
    //cache
    usersMapped() {
      const start = this.perPage * (this.page - 1); //this.page จะมีจำนวนเป้น 1 อยู่ ถ้า this.perPage เป้น 5 นั่นหมายความว่าเราจะเริ่มตำแหน่่งที่ 5 เป็นต้นไป ดั้งนั้น this.page - 1 เพื่อให้หน้าแรกเริ่มต้นที่ 0
      return this.users
        .map((user) => {
          user.startDate = dayjs(user.startDate).format("DD/MM/YYYY");
          return user;
        })
        .slice(start, start + this.perPage);
    },
    totalPage() {
      return Math.ceil(this.users.length / this.perPage);
    },
  },
  watch: {
    //ดูการเปลี่ยนแปลงของข้อมูล
    perPage() {
      this.page = 1;
    },
  },
};
</script>

<style scoped>
.user-color {
  display: inline-block;
  width: 1rem;
  height: 1rem;
  border-radius: 50%;
}
</style>
