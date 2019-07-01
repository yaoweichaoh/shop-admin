<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>活动管理</el-breadcrumb-item>
      <el-breadcrumb-item>活动列表</el-breadcrumb-item>
    </el-breadcrumb>

    <div>
      <el-input
        placeholder="请输入内容"
        class="input-with-select"
        v-model="keyword"
        @keyup.enter.native="search"
      >
        <el-button slot="append" icon="el-icon-search" @click="search"></el-button>
      </el-input>

      <el-button type="success" plain @click="ondialogFormVisible">添加用户</el-button>
    </div>

    <el-table :data="userList" stripe style="width: 100%">
      <el-table-column prop="username" label="姓名" width="180"></el-table-column>
      <el-table-column prop="email" label="邮箱" width="180"></el-table-column>
      <el-table-column prop="mobile" label="电话"></el-table-column>

      <el-table-column label="用户状态">
        <template v-slot="{ row }">
          <el-switch
            v-model="row.mg_state"
            active-color="#13ce66"
            inactive-color="#ff4949"
            @change="putuser(row)"
          ></el-switch>
        </template>
      </el-table-column>

      <el-table-column label="状态">
        <template v-slot="{ row }">
          <el-button size="mini" type="primary" plain icon="el-icon-edit" @click="putUserFormData(row.id)"></el-button>

          <el-button size="mini" type="danger" plain icon="el-icon-delete" @click="deluser(row.id)"></el-button>

          <el-button size="mini" type="success" plain icon="el-icon-check">角色分配</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页 -->
    <el-pagination
      background
      layout="prev, pager, next"
      :total="total"
      :page-size="pagesize"
      :current-page="currentpage"
      @current-change="onPageChange"
    ></el-pagination>

    <!-- 添加模态框 -->
    <el-dialog
      title="添加用户"
      :visible.sync="isAddUserDialogShow"
      @close="$refs.addUserForm.resetFields()"
    >
      <el-form label-width="100px" :model="addUserFormData" :rules="addrules" ref="addUserForm">
        <el-form-item label="用户名" prop="username">
          <el-input v-model="addUserFormData.username" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="密码" prop="password">
          <el-input v-model="addUserFormData.password" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="邮箱" prop="email">
          <el-input v-model="addUserFormData.email" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="手机" prop="mobile">
          <el-input v-model="addUserFormData.mobile" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="isAddUserDialogShow = false">取 消</el-button>
        <el-button type="primary" @click="diauser">确 定</el-button>
      </div>
    </el-dialog>

     <!-- 修改模态框 -->
    <el-dialog
      title="添加用户"
      :visible.sync="isPutUserDialogShow"
      @close="$refs.editUserForm.resetFields()"
    >
      <el-form label-width="100px" :model="putUserForm" :rules="putrules" ref="editUserForm">
        <el-form-item label="用户名" prop="username">
          <el-input type="info" v-text="putUserForm.username" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="邮箱" prop="email">
          <el-input v-model="putUserForm.email" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="手机" prop="mobile">
          <el-input v-model="putUserForm.mobile" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="isPutUserDialogShow = false">取 消</el-button>
        <el-button type="primary" @click="editUser">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
export default {
  data() {
    return {
      userList: [],
      currentpage: 1,
      pagesize: 3,
      total: 0,
      keyword: "",
      // 控制模态框隐藏
      isAddUserDialogShow: false,
      addUserFormData: {
        username: "",
        password: ""
      },
      addrules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          {
            min: 5,
            max: 12,
            message: "长度在 5 到 12 个字符",
            trigger: "change"
          }
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          {
            min: 5,
            max: 14,
            message: "长度在 5 到 14 个字符",
            trigger: "change"
          }
        ],
        email: [
          { required: true, message: "请输入邮箱地址", trigger: "blur" },
          {
            type: "email",
            message: "请输入正确的邮箱地址",
            trigger: ["blur", "change"]
          }
        ],
        mobile: [
          {
            pattern: /^(0|86|17951)?(13[0-9]|15[012356789]|166|17[3678]|18[0-9]|14[57])[0-9]{8}$/,
            message: "请输入正确的手机号",
            trigger: "change"
          }
        ]
      },
      isPutUserDialogShow: false,
      putUserForm: {
        id: 0,
        username: "",
        email: "",
        mobile: ""
      },
      putrules: {
         email: [
          { required: true, message: "请输入邮箱地址", trigger: "blur" },
          {
            type: "email",
            message: "请输入正确的邮箱地址",
            trigger: ["blur", "change"]
          }
        ],
        mobile: [
          {
            pattern: /^(0|86|17951)?(13[0-9]|15[012356789]|166|17[3678]|18[0-9]|14[57])[0-9]{8}$/,
            message: "请输入正确的手机号",
            trigger: "change"
          }
        ]
      }
    };
  },
  created() {
    this.getUserList();
  },
  methods: {
    getUserList() {
      this.$http({
        url: "users",
        params: {
          query: this.keyword,
          pagenum: this.currentpage,
          pagesize: this.pagesize
        }
      }).then(res => {
        let {
          data: { data }
        } = res;
        this.userList = data.users;
        this.total = data.total;
      });
    },
    onPageChange(page) {
      this.currentpage = page;

      this.getUserList();
    },
    async deluser(id) {
      try {
        await this.$confirm("此操作将永久删除该文件, 是否继续?", "提示", {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning",
          center: true
        });
        let res = await this.$http({
          url: `users/${id}`,
          method: "delete"
        });
        if (res.data.meta.status == 200) {
          this.$message({
            type: "success",
            message: res.data.meta.msg,
            duration: 1000
          });
          this.currentpage = 1;
          this.getUserList();
        }
      } catch {
        this.$message({
          type: "info",
          message: "已取消删除"
        });
      }
    },
    ondialogFormVisible() {
      this.isAddUserDialogShow = true;
    },
    async diauser() {
      try {
        await this.$refs.addUserForm.validate();
        let res = await this.$http({
          url: "users",
          method: "post",
          data: this.addUserFormData
        });
        console.log(res);
        if (res.data.meta.status === 201) {
          this.$message({
            message: res.data.meta.msg,
            type: "success",
            duration: 1000
          });
          this.getUserList();
          this.isAddUserDialogShow = false;
        } else {
          this.$message({
            message: res.data.meta.msg,
            type: "error",
            duration: 1000
          });
        }
      } catch {}
    },
    async putuser(user) {
      console.log(user);
      try {
        let res = await this.$http({
          url: `users/${user.id}/state/${user.mg_state}`,
          method: "put"
        });
        //  console.log(res)
        if (res.data.meta.status === 200) {
          this.$message({
            message: res.data.meta.msg,
            type: "success",
            duration: 1000
          });
          // this.getUserList()
        } else {
          this.$message({
            message: res.data.meta.msg,
            type: "error",
            duration: 1000
          });
          user.mg_state = !user.mg_state;
        }
      } catch {}
    },
    search() {
      this.getUserList();
    },
   async putUserFormData(id) {
     this.isPutUserDialogShow = true

     let res = await this.$http({
       url: `users/${id}`
     })
    //  console.log(res)
    this.putUserForm = res.data.data
    },
   async editUser(){
      try {
        await this.$refs.editUserForm.validate();

        let res = await this.$http({
          url: `users/${this.putUserForm.id}`,
          method:"put",
          data: this.putUserForm
        })
        // console.log(res)
        if(res.data.meta.status === 200) {
          this.$message({
             message: res.data.meta.msg,
            type: "success",
            duration: 1000
          })
          this.isPutUserDialogShow = false
          this.getUserList()
        }else {
          this.$message({
            message: res.data.meta.msg,
            type: "error",
            duration: 1000
          });
        }
      } catch {}
    }
  }
};
</script>
<style>
.el-main .el-breadcrumb {
  height: 50px;
  background-color: #d4dae0;
  font-size: 16px;
  line-height: 50px;
  padding-left: 15px;
}
.el-main .el-input {
  width: 350px;
}
.el-main {
  text-align: left;
}
.el-main .el-button {
  margin-left: 20px;
}
</style>
