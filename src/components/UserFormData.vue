<template>
  <form class="mt-3 border rounded p-3" @submit.prevent="addUser">
    <div class="mb-3">
      <label class="form-label">รหัสพนักงาน</label>
      <input :value="input.id" class="form-control" type="text" disabled />
    </div>
    <div class="mb-3">
      <label class="form-label">ชื่อจริง</label>
      <input
        v-model="input.firstName"
        class="form-control"
        type="text"
        required
      />
    </div>
    <div class="mb-3">
      <label class="form-label">นามสกุล</label>
      <input
        v-model="input.lastName"
        class="form-control"
        type="text"
        required
      />
    </div>
    <div class="mb-3">
      <label class="form-label">สาขา</label>
      <select v-model="input.branch" class="form-select">
        <option v-for="branch of branches" :value="branch">
          {{ branch }}
        </option>
      </select>
    </div>
    <div class="mb-3">
      <label class="form-label">สีประจำพนักงาน</label>
      <input v-model="input.color" class="form-control" type="color" />
    </div>
    <div class="mb-3">
      <label class="form-label">วันที่เริ่มงาน</label>
      <input v-model="input.date" class="form-control" type="date" />
    </div>
    <div class="d-flex gap-1">
      <button class="btn btn-primary">เพิ่มข้อมูล</button>
      <button type="button" class="btn btn-success" @click="randomData">
        สุ่มข้อมูล
      </button>
      <button class="btn btn-danger" type="button" @click="clearData">
        ล้างข้อมูล
      </button>
    </div>
  </form>
</template>

<script>
import { faker } from "@faker-js/faker";
import { sample, clone } from "lodash";
export default {
  data: () => ({
    users: [],
    branches: [
      "Back-end Developer",
      "Front-end Developer",
      "Fullstack Developer",
    ],
    input: {
      id: 1,
      firstName: "",
      lastName: "",
      branch: "",
      color: "",
      startDate: "",
    },
  }),
  methods: {
    addUser() {
      this.users.push(clone(this.input));
      this.$emit("update:user", this.users);
      this.clearData();
      this.input.id++;
    },
    clearData() {
      this.input.firstName = "";
      this.input.lastName = "";
      this.input.branch = "";
      this.input.color = "";
      this.input.startDate = "";
    },
    randomData() {
      this.input.firstName = faker.name.firstName();
      this.input.lastName = faker.name.lastName();
      this.input.branch = sample(this.branches);
      this.input.color = faker.color.rgb({ format: "hex", casing: "lower" });
      this.input.startDate = faker.date.past();
      this.addUser();
    },
    // formatDate(date) {
    //   return dayjs(date).format("DD/MM/YYYY");
    // },
  },
};
</script>
