<template>
  <h2>人事管理系统</h2>
  <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect"
    background-color="#545c64" text-color="#fff" active-text-color="#ffd04b">
    <el-submenu index="1">
      <template #title>部门管理</template>
      <el-menu-item index="1-1"><router-link to="/department" class="a">管理部门</router-link></el-menu-item>
      <el-menu-item index="1-2"><router-link to="/adddepartment" class="a">新增部门</router-link></el-menu-item>
    </el-submenu>
    <el-submenu index="2">
      <template #title>岗位管理</template>
      <el-menu-item index="2-1"><router-link to="/post" class="a">管理岗位</router-link></el-menu-item>
      <el-menu-item index="2-2"><router-link to="/addpost" class="a">新增岗位</router-link></el-menu-item>
    </el-submenu>
    <el-submenu index="3">
      <template #title>员工管理</template>
      <el-menu-item index="3-1"><router-link to="/staff" class="a">管理员工</router-link></el-menu-item>
      <el-menu-item index="3-2"><router-link to="/addStaff" class="a">新增员工</router-link></el-menu-item>
    </el-submenu>
    <el-menu-item index="4"><router-link to="/peroidOp" class="a">试用期管理</router-link></el-menu-item>
    <el-submenu index="5">
      <template #title>岗位调动管理</template>
      <el-menu-item index="5-1"><router-link to="/addTransferStaff" class="a">录入岗位调动</router-link></el-menu-item>
      <el-menu-item index="5-2"><router-link to="/transferStaff" class="a">查询调动员工</router-link></el-menu-item>
    </el-submenu>
    <el-submenu index="6">
      <template #title>员工离职管理</template>
      <el-menu-item index="6-1"><router-link to="/addQuit" class="a">录入离职员工</router-link></el-menu-item>
      <el-menu-item index="6-2"><router-link to="/quit" class="a">查询已离职员工</router-link></el-menu-item>
    </el-submenu>
    <el-submenu index="7">
      <template #title>报表管理</template>
      <el-menu-item index="7-1"><router-link to="/newStaffReport" class="a">新聘员工报表</router-link></el-menu-item>
      <el-menu-item index="7-2"><router-link to="/quitStaffReport" class="a">离职员工报表</router-link></el-menu-item>
      <el-menu-item index="7-3"><router-link to="/transferStaffReport" class="a">岗位调动报表</router-link></el-menu-item>
    </el-submenu>
    <el-submenu index="8">
      <template #title>管理员信息</template>
      <el-menu-item @click="dialogVisible = true" index="8-1">修改密码</el-menu-item>
      <el-menu-item @click="toLogout" index="8-2">退出登录</el-menu-item>
    </el-submenu>
    <el-dialog center title="修改密码" v-model="dialogVisible" width="30%">
      <el-form ref="form" :model="form" :rules="rules" style="width:50%;" label-width="20%">
        <el-form-item prop="apwd" placeholder="请输入密码" label="密码">
          <el-input v-model="form.apwd" />
        </el-form-item>
      </el-form>
      <template #footer class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="editPwd">确 定</el-button>
      </template>
    </el-dialog>
  </el-menu>
</template>
<script>
export default {
  name: "NavMain",
  data() {
    return {
      //员工信息中心点亮
      activeIndex: "9",
      dialogVisible: false,
      form: {
        apwd: "",
      },
      rules: {
        apwd: [{ required: true, message: "请输入密码", trigger: "blur" }],
      },
    };
  },
  methods: {
    handleSelect(key, keyPath) {
      console.log(key, keyPath);
    },
    toLogout() {
      this.$store.commit("changeLogin", "0");
      this.$router.replace("/login");
    },
    editPwd() {
      this.dialogVisible = false;
      this.$axios.get(`/editPwd?apwd=${this.form.apwd}`).then((resp) => {
        if (resp.data === "ok") {
          this.$message({
            message: "修改成功，请重新登录",
            type: "success",
          });
          this.$router.replace("/login");
        } else {
          this.$message.error("修改失败");
        }
      });
    },
  },
};
</script>
<style>
.a {
  text-decoration: none;
  color: aliceblue;
}
</style>
