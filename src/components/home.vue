<template xmlns="" xmlns="">
      <el-container>
<!--        头部-->
        <el-header>
<!--          logo区域-->
          <div>
            <img alt="" src="">
          </div>
<!--          退出按钮-->
          <el-button @click="logout" class="logout" type="info">退出</el-button>
        </el-header>
<!--        页面主体区域-->
        <el-container>
<!--          侧边栏-->
          <el-aside :width="isCollapse ? '64px' : '200px'">
            <div @click="toggleCollapse" class="toggle-button">|||</div>
<!--            侧边栏菜单区域-->
            <el-menu
              :collapse="isCollapse"
              :collapse-transition="false"
              :default-active="activePath"
              active-text-color="#409Eff"
              background-color="#333744 "
              class="el-menu-vertical-demo"
              default-active="2"
              router
              text-color="#fff"
              unique-opened>
<!--              一级菜单-->
              <el-submenu :index="item.id + ''" :key="item.id" v-for="item in munelist">
<!--                一级菜单的模板区域-->
                <template slot="title">
<!--                  图标-->
                  <i class="el-icon-s-custom"></i>
<!--                  文本-->
                  <span>{{item.authName}}</span>
                </template>
<!--                二级菜单-->
                  <el-menu-item :key="subItem.id" @click="saveNavState('/'+subItem.path)" index="'/' + subItem.path" v-for="subItem in item.children">
<!--                   图标-->
                    <i class="el-icon-menu"></i>
                    <!--                  文本-->
                    <span>{{subItem.authName}}</span>
                  </el-menu-item>
                </el-submenu>
            </el-menu>
          </el-aside>
<!--          右侧主体内容区域-->
          <el-main>
            <router-view/>
          </el-main>
        </el-container>
      </el-container>
</template>

<script>
    export default {
        name: "home",
        data (){
            return {
              munelist:[
                  {
                      "id": 101,
                      "authName": "商品管理",
                      "path": null,
                      "children": [
                          {
                              "id": 104,
                              "authName": "商品列表",
                              "path": null,
                              "children": []
                          }
                      ]
                  }
              ],
                // 是否折叠
                isCollapse:false,
                // 被激活的链接地址
                activePath:''
            }
        },
        created(){
            this.getMenuList()
            this.activePath = window.sessionStorage.getItem('activePath')
        },
        methods:{
            // 退出按钮的点击事件
            logout(){
                window.sessionStorage.clear();
                this.$router.push('/login')
            },
            // 获取所有菜单
            async getMenuList(){
                 const {data:res} = await this.$http.get('menus')
                // console.log(res)
                if(res.mate.status !==200)return this.$message.error('这是错误的信息');
                this.munelist = res.data
            },
            // 点击按钮,切换菜单的折叠与展开
            toggleCollapse(){
                this.isCollapse = !this.isCollapse
            },
            // 保存链接的激活状态
            saveNavState(activePath){
            window.sessionStorage.getItem('activePath',activePath)
            this.activePath = activePath
            }
        }
    }
</script>

<style scoped>
  .el-container{
    height: 100%;
  }
  .el-header{
    background-color: #2b4b6b;
    display: flex;
    justify-content: space-between;
    padding-left: 0;
    align-items: center;
  }
  .el-aside{
    background-color:#333744;
  }
  .el-main{
    background-color:aqua;
  }
  .el-menu{
    border-right: none;
  }
 .toggle-button{
   text-align: center;
   background-color: #dddddd;
   letter-spacing: 0.2em;
   cursor: pointer;
 }
</style>
