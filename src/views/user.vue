<template>
  <div>
    <el-table :data="userList" stripe style="width: 100%">
      <el-table-column prop="username" label="姓名" width="180"></el-table-column>
      <el-table-column prop="email" label="邮箱" width="180"></el-table-column>
      <el-table-column prop="mobile" label="电话"></el-table-column>

      <el-table-column label="用户状态">
       <template v-slot="{row}">
          <el-switch v-model="row.type" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
       </template>
      </el-table-column>

      <el-table-column label="状态">
        <el-row>
         
          <el-button size="mini" type="primary" plain icon="el-icon-edit"></el-button>
        
          <el-button size="mini" type="danger" plain icon="el-icon-delete"></el-button>

          <el-button size="mini" type="success" plain icon="el-icon-check">角色分配</el-button>
        </el-row>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      userList: []
    };
  },
  created() {
    axios({
      url: "http://localhost:8888/api/private/v1/users",
      params: {
        pagenum: 1,
        pagesize: 5
      },
      headers: {
        Authorization: localStorage.getItem("token")
      }
    }).then(res => {
      // console.log(res);
      if (res.data.meta.status === 200) {
        this.userList = res.data.data.users;
      }
    });
  }
};
</script>
