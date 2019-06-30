<template>
  <el-container>
    <el-header>
      <el-row type="flex" class="row-bg" justify="space-between">
        <el-col :span="6">
          <div class="imgs">
            <img src="../assets/logo.png" alt class="img" />
          </div>
        </el-col>
        <el-col :span="6">
          <div class="item">唯品购后台管理</div>
        </el-col>
        <el-col :span="6">
          <div>
            <div class="emit">
              欢迎上海前端31期星曜会员
              <a href="#" class="a">退出</a>
            </div>
          </div>
        </el-col>
      </el-row>
    </el-header>
    <el-container class="el-bg">
      <el-aside width="200px">
        <el-row class="tac">
          <el-col :span="24">
            <el-menu
              :default-active="$route.path"
              class="el-menu-vertical-demo"
              background-color="#545c64"
              text-color="#fff"
              active-text-color="#ffd04b"
              :unique-opened="true"
              :router="true"
            >
              <el-submenu index="1">
                <template slot="title">
                  <i class="el-icon-location"></i>
                  <span>用户管理</span>
                </template>
                <el-menu-item-group>
                  <el-menu-item index="/user">
                    <i class="el-icon-menu"></i>
                    <span slot="title">用户列表</span>
                  </el-menu-item>
                </el-menu-item-group>
              </el-submenu>

              <el-submenu index="3">
                <template slot="title">
                  <i class="el-icon-location"></i>
                  <span>权限管理</span>
                </template>
                <el-menu-item-group>
                  <el-menu-item index="4">
                    <i class="el-icon-menu"></i>
                    <span slot="title">角色列表</span>
                  </el-menu-item>
                  <el-menu-item index="5">
                    <i class="el-icon-menu"></i>
                    <span slot="title">权限列表</span>
                  </el-menu-item>
                </el-menu-item-group>
              </el-submenu>

              <el-submenu index="6">
                <template slot="title">
                  <i class="el-icon-location"></i>
                  <span>商品管理</span>
                </template>
                <el-menu-item-group>
                  <el-menu-item index="7">
                    <i class="el-icon-menu"></i>
                    <span slot="title">商品列表</span>
                  </el-menu-item>
                  <el-menu-item index="8">
                    <i class="el-icon-menu"></i>
                    <span slot="title">分页参数</span>
                  </el-menu-item>
                  <el-menu-item index="9">
                    <i class="el-icon-menu"></i>
                    <span slot="title">商品分类</span>
                  </el-menu-item>
                </el-menu-item-group>
              </el-submenu>

              <el-submenu index="10">
                <template slot="title">
                  <i class="el-icon-location"></i>
                  <span>订单管理</span>
                </template>
                <el-menu-item-group>
                  <el-menu-item index="11">
                    <i class="el-icon-menu"></i>
                    <span slot="title">订单列表</span>
                  </el-menu-item>
                </el-menu-item-group>
              </el-submenu>

              <el-submenu index="12">
                <template slot="title">
                  <i class="el-icon-location"></i>
                  <span>数据管理</span>
                </template>
                <el-menu-item-group>
                  <el-menu-item index="13">
                    <i class="el-icon-menu"></i>
                    <span slot="title">数据列表</span>
                  </el-menu-item>
                </el-menu-item-group>
              </el-submenu>
            </el-menu>
          </el-col>
        </el-row>
      </el-aside>
      <el-container>
        <el-main>
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
  </el-container>
</template>

<script>
export default {};
</script>


<style>
.el-header,
.el-footer {
  background-color: #b3c0d1;
  color: #333;
  text-align: center;
  line-height: 60px;
}
.el-container .el-header {
  padding: 0;
}
.el-header .row-bg {
  background-color: #b3c0d1;
}
.el-main {
  background-color: #e9eef3;
  color: #333;
  text-align: center;
  line-height: 160px;
}

.el-container {
  height: 100%;
}
.el-bg {
  height: 100%;
}
.el-aside {
  height: 100%;
  background-color: rgb(84, 92, 100);
}
.imgs {
  width: 200px;
  height: 60px;
}
.img {
  width: 200px;
}
.item {
  font-size: 28px;
  font-weight: 700;
  color: #fff;
}
.emit {
  width: 300px;
  color: black;
  font-weight: 700;
  text-align: right;
}
.a {
  color: orange;
}
.el-main {
  line-height: 0;
}
</style>