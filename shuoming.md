# 项目流程

#### 1、创建项目

```tex
vue init webpack umall
```

#### 2、清空工作

```tex
删除assets下的所有东西
删除了compoents下的所以东西
重置了router/index.js
重置了App.vue
重置了main.js
```

#### 3、项目搭建

##### 1、目录搭建

```tex
-src
	-assets	静态资源
		-css	存放css
		-js		存放js
		-image	存放img
	-components	存放公共组件
		index.js	整合所有公共组件
	-filters	过滤器
		index.js	整合所有过滤器
	-page	路由组件
	-router	路由
		index.js	配置路由规则
	-utils	工具类
		request.js	数据交互
	App.vue	根组件
	main.js	入口文件
```

##### 2、assets

在main.js中引入reset.css

```js
import "./assets/css/reset.css"
```

##### 3、公共组件components

在components/index.js中

```js
import Vue from "vue"

let obj = {

}

for(let i in obj){
  Vue.component(i,obj[i])
}
```

在main.js中引入

```js
import "./components/index.js"
```

##### 4、过滤器filter

在filters/index.js中

```js
import Vue from "vue"

let obj = {

}

for(let i in obj){
  Vue.filter(i,obj[i])
}
```

在main.js中引入

```js
import "./filters/index.js"
```

##### 5、数据交互

1、安装依赖包

```tex
npm i axios qs --save
```

2、在config/index.js中配置代理

```js
    proxyTable: {
      "/api":{
        target:"http://localhost:3000",
        changeOrigin:true,
        pathRewrite:{
          "^/api":"http://localhost:3000"
        }
      }
    }
```

3、在utils/request.js中

```js
import axios from "axios"
import qs from "qs"

// 开发环境
let baseUrl = "/api"
// 上线环境
// let baseUrl = ""

// 响应拦截器
axios.interceptors.response.use(res=>{
  console.log("=====响应拦截开始=====");
  console.log(res);
  console.log("=====响应拦截结束=====");


  return res
})
```

##### 6、vuex

1、安装

```tex
npm i vuex --save
```

2、配置store/actions.js

```js
const actions = {}
export default actions
```

3、配置store/mutations.js

```js
export const state = {}
export const mutations = {}
export const getters = {}
```

4、配置store/index.js

```js
import Vue from "vue"
import Vuex from "vuex"
Vue.use(Vuex)

import actions from "./actions.js"
import {state,mutations,getters} from "./mutations.js"


export default new Vuex.Store({
  state,
  mutations,
  actions,
  getters,
  modules:{

  }
})
```

5、在 main.js中引入并挂载配置项中

```js
import store from "./store/index.js"

new Vue({
  store
})
```

##### 7、element-ui

1、安装

```tex
npm i element-ui --save
```

2、在main.js中引入

```js
import ElementUI from "element-ui"
import "element-ui/lib/theme-chalk/index.css"
Vue.use(ElementUI)
```

3、在utils/index.js中进行二次封装弹窗

```js
import Vue from "vue"
let vm = new Vue()

export const successAlert = (msg)=>{
  vm.$message({
    message: msg,
    type: 'success'
  });
}

export const warningAlert = (msg)=>{
  vm.$message({
    message: msg,
    type: 'warning'
  });
}
```

#### 4、配置一级路由

1、在page下创建了login和index文件夹，文件夹下分别创建了index.vue和login.vue文件

2、在router/index.js中配置路由规则

```js
export default new Router({
  routes: [{
    path:"/login",
    component: ()=>import("../page/login/login.vue")
  },{
    path:"/",
    component: ()=>import("../page/index/index.vue")
  }]
})
```

3、在App.vue中设置路由出口

```html
<router-view></router-view>
```

#### 5 、login.vue

```html
// html
  <div class="login">
    <div class="box">
      <h3>登录</h3>
      <div class="smbox">
        <el-input v-model="user.name" placeholder="请输入账号"></el-input>
      </div>
      <div class="smbox">
        <el-input v-model="user.pass" placeholder="请输入密码" show-password></el-input>
      </div>
      <div>
         <el-button type="primary">登录</el-button>
      </div>
    </div>
  </div>
```

```css
// css
<style>
.login{
  width: 100vw;
  height: 100vh;
  background: linear-gradient(to right,rgb(62,30,44),rgb(27,41,74));
}
.box{
  width: 410px;
  height: 250px;
  background: white;
  border-radius: 10px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  text-align: center;
}
.smbox{
  width: 80%;
  margin: 10px auto;
}
</style>
```



#### 6、配置二级路由

1、在page文件夹下创建二级路由组件

2、配置二级路由规则

```js
{
    path:"/",
    component: ()=>import("../page/index/index.vue"),
    children:[{
      path:"",
      name:"首页",
      component: ()=>import("../page/index/index.vue")
    },{
      path:"menu",
      name:"菜单管理",
      component: ()=>import("../page/menu/menu.vue")
    },{
      path:"role",
      name:"角色管理",
      component: ()=>import("../page/role/role.vue")
    },{
      path:"manage",
      name:"管理员管理",
      component: ()=>import("../page/manage/manage.vue")
    },{
      path:"cate",
      name:"商品分类",
      component: ()=>import("../page/cate/cate.vue")
    },{
      path:"specs",
      name:"商品规格",
      component: ()=>import("../page/specs/specs.vue")
    },{
      path:"goods",
      name:"商品管理",
      component: ()=>import("../page/goods/goods.vue")
    },{
      path:"member",
      name:"会员管理",
      component: ()=>import("../page/member/member.vue")
    },{
      path:"banner",
      name:"轮播图管理",
      component: ()=>import("../page/banner/banner.vue")
    },{
      path:"seckill",
      name:"秒杀活动",
      component: ()=>import("../page/seckill/seckill.vue")
    }]
  }
```

#### 7、index.vue

##### 1、布局

去element-ui中找到Container 布局容器的导倒数第二个组件

```html
  <div>
    <el-container class="box">
      <el-aside width="200px" class="navLeft">
        <el-row class="tac">
          <el-col :span="24">
            <!-- default-active 当前激活的
                  unique-opened 是否只保持一个子菜单打开
                  background-color  背景颜色
                  text-color  文字颜色
                  active-text-color 激活的那一项文字颜色
                  router  开启路由跳转
             -->
            <el-menu class="el-menu-vertical-demo" default-active="0" unique-opened background-color="rgb(32,34,42)" text-color="#fff" active-text-color="#ffd04b" router>

              <el-menu-item index="/">
                <i class="el-icon-s-home"></i>
                <span>首页</span>
              </el-menu-item>

              <el-submenu index="1">
                <template slot="title">
                  <i class="el-icon-s-tools"></i>
                  <span>系统设置</span>
                </template>
                <el-menu-item index="/menu">菜单管理</el-menu-item>
                <el-menu-item index="/role">角色管理</el-menu-item>
                <el-menu-item index="/manage">管理员管理</el-menu-item>
              </el-submenu>

              <el-submenu index="2">
                <template slot="title">
                  <i class="el-icon-s-goods"></i>
                  <span>商城管理</span>
                </template>
                <el-menu-item index="/cate">商品分类</el-menu-item>
                <el-menu-item index="/specs">商品规格</el-menu-item>
                <el-menu-item index="/goods">商品管理</el-menu-item>
                <el-menu-item index="/member">会员管理</el-menu-item>
                <el-menu-item index="/banner">轮播图管理</el-menu-item>
                <el-menu-item index="/seckill">秒杀活动</el-menu-item>
              </el-submenu>
            </el-menu>
          </el-col>
        </el-row>
      </el-aside>
      <el-container>
        <el-header>头部</el-header>
        <el-main>
          <!-- 路由出口 -->
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
  </div>
```

一定不要忘了在el-main部分设置路由出口

##### 2、主体内容

1、设置面包屑，请先完成第2步

```html
        <el-main>
          <!-- 面包屑 -->
          <el-breadcrumb separator-class="el-icon-arrow-right">
            <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>{{$route.name}}</el-breadcrumb-item>
          </el-breadcrumb>
          <!-- 路由出口 -->
          <router-view></router-view>
        </el-main>
```

2、给二级路由设置name

```js
children:[{
      path:"menu",
      name:"菜单管理",
      component: ()=>import("../page/index/components/menu.vue")
    },{
      path:"role",
      name:"角色管理",
      component: ()=>import("../page/index/components/role.vue")
    }
```

#### 8、menu.vue

##### 添加功能

1、拆分页面，拆分成list.vue和add.vue

```html
<template>
  <div>
    <el-button type="primary" @click="isTrue=true">添加</el-button>
    <v-list></v-list>
    <v-add :isTrue="isTrue"></v-add>
  </div>
</template>
```

2、add.vue

先去element-ui找到form

```html
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="菜单名称">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="上级菜单">
          <el-select v-model="form.region" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="菜单类型">
          <el-radio-group v-model="form.resource">
            <el-radio label="线上品牌商赞助"></el-radio>
            <el-radio label="线下场地免费"></el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="菜单图标">
          <el-select v-model="form.region" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="菜单地址">
          <el-select v-model="form.region" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="状态">
          <el-switch v-model="form.delivery"></el-switch>
        </el-form-item>
      </el-form>
```

3、去element-ui中找到对话框，并把form的所以东西都放到对话框里

```html
    <el-dialog title="菜单添加" :visible.sync="isTrue" width="40%">
      <el-form ref="form" :model="form" label-width="80px">
      ...
      ...
      ...
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
```

4、对话需要一个变量isShow来控制显示与隐藏，在menu.vue中定义isShow，但是对话框就需要这个isShow，所以我们在父组件中点击了添加按钮将isTrue变成true，同时通过父传子传递给add组件，add组件接收到了isShow之后可以帮顶给自己对话框的:visible.sync="isShow"，但是子组件点击了关闭后需要把isShow变成false，所以我们不能传递基本数据类型，需要传递一个对象类型，从而实现父变子变，子变父变

menu.vue给子组件传递用来控制显示与隐藏的数据

```js
        obj:{
          isShow:false
        }
        
        <v-add :obj="obj"></v-add>
```

add.vue通过props接收

```js
props: ["obj"]

<el-dialog title="菜单添加" :visible.sync="obj.isShow" width="40%">
```

5、在data中定义数据，使用v-model来渲染页面，下拉地址那里需要特殊处理一下，在下拉地址的时候请先完成第6步

```js
import {secondRouter} from "../../../router/index.js"

		form: {
          pid:"aaa",
          title:"",
          icon:"aaa",
          type:1,
          url:"aaa",
          status:1
        },
        // 用来渲染图标的下拉框
        iconArr:["el-icon-delete-solid","el-icon-s-tools","el-icon-star-on","el-icon-question","el-icon-upload"],
        // 用来渲染菜单地址
        secondRouter:secondRouter,
```

6、在router/index.js中修改二级路由，用一个变量来接收，同时使用...运算符假如到路由规则中

```js
export const secondRouter = [{
  path: "menu",
  name: "菜单管理",
  component: () => import("../page/menu/menu.vue")
}, {
  path: "role",
  name: "角色管理",
  component: () => import("../page/role/role.vue")
}, {
  path: "manage",
  name: "管理员管理",
  component: () => import("../page/manage/manage.vue")
}, {
  path: "cate",
  name: "商品分类",
  component: () => import("../page/cate/cate.vue")
}, {
  path: "specs",
  name: "商品规格",
  component: () => import("../page/specs/specs.vue")
}, {
  path: "goods",
  name: "商品管理",
  component: () => import("../page/goods/goods.vue")
}, {
  path: "member",
  name: "会员管理",
  component: () => import("../page/member/member.vue")
}, {
  path: "banner",
  name: "轮播图管理",
  component: () => import("../page/banner/banner.vue")
}, {
  path: "seckill",
  name: "秒杀活动",
  component: () => import("../page/seckill/seckill.vue")
}]
```

```js
export default new Router({
  routes: [{
    path: "/login",
    component: () => import("../page/login/login.vue")
  }, {
    path: "/",
    component: () => import("../page/index/index.vue"),
    children: [{
      path: "",
      name: "首页",
      component: () => import("../page/home/home.vue")
    },
    ...secondRouter]
  }]
})
```

7、当上级菜单发生变化的时候

因为菜单类型不能让用户自己选择，而是通过上级菜单的变化来通知，当上级菜单是0的时候，应该选择中目录，当上级菜单是非0的时候，应该选中的是菜单。

```js
      // 当上级菜单发生变化的时候
      changePid(){
        if(this.form.pid==0){
          // 说明选择的是顶级菜单，那么对应的就应该让目录选中
          this.form.type = 1
          this.form.url = ""
        }else{
          // 说明选择的不是顶级菜单，那么对应的就应该让菜单选中
          this.form.type = 2
          this.form.icon = ""
        }
      }
```

8、点击取消，第一要关闭add这个弹窗，第二要清空数据，同时还要给对话框添加一个@close事件

```js
      <el-dialog @close="close()">
      <el-button @click="close()">取 消</el-button>

		// 清空数据
      empty(){
        this.form = {
          pid:"aaa",
          title:"",
          icon:"aaa",
          type:1,
          url:"aaa",
          status:1
        }
      },
      // 点击了关闭按钮
      close(){
        this.obj.isShow = false
        this.empty()
      }
```

9、添加交互接口在utils/request.js中

```js
export const menuAdd = (form)=>{
  return axios({
    url:baseUrl + "/api/menuadd",
    method:"post",
    data: qs.stringify(form)
  })
}
```

10、点击了添加按钮

```js
      submit(){
        menuAdd(this.form).then(res=>{
          if(res.data.code == 200){
            // 成功的弹窗
            successAlert(res.data.msg)
            // 关闭add.vue并清空form
            this.close()
          }else{
            // 失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
```

##### 列表功能

1、去element-ui中找到表格（树形结构的那个表格）

```html
    <el-table :data="MenuList" style="width: 100%;margin-bottom: 20px;" row-key="id" border
      :tree-props="{children: 'children', hasChildren: 'hasChildren'}">
      <el-table-column prop="id" label="菜单编号">
      </el-table-column>
      <el-table-column prop="title" label="菜单名称">
      </el-table-column>
      <el-table-column label="菜单图标">
        <template slot-scope="scope">
          <div>
            <i :class="scope.row.icon"></i>
          </div>
        </template>
      </el-table-column>
      <el-table-column prop="url" label="菜单地址">
      </el-table-column>
      <el-table-column prop="status" label="状态">
        <template slot-scope="scope">
          <div>
            <el-button v-if="scope.row.status==1">启 用</el-button>
            <el-button v-else>禁 用</el-button>
          </div>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template>
          <div>
            <el-button type="primary">编 辑</el-button>
            <el-button type="danger">删 除</el-button>
          </div>
        </template>
      </el-table-column>
    </el-table>
```

2、添加交互接口在utils/request.js中

```js
export const menuList = ()=>{
  return axios({
    url:baseUrl + "/api/menulist",
    method:"get",
    params:{
      istree:true
    }
  })
}
```

3、配置状态层，在状态层请求数据

```js
import { menuList } from "../../utils/request"

const state = {
  list:[]
}
const mutations = {
  changeList(state,arr){
    state.list = arr
  }
}
const actions = {
  reqList(context){
    menuList().then(res=>{
      context.commit("changeList",res.data.list)
    })
  }
}
const getters = {
  list(state){
    return state.list
  }
}

export default{
  state,
  mutations,
  actions,
  getters,
  namespaced: true
}
```

4、页面一进来就先请求数据

```js
    mounted() {
      this.reqMenuList()
    }
```

5、针对第一条复制过来的表格，页面需要修改的地方

```html
      <el-table-column label="菜单图标">
        <template slot-scope="scope">
          <div>
            <i :class="scope.row.icon"></i>
          </div>
        </template>
      </el-table-column>
            <el-table-column prop="status" label="状态">
        <template slot-scope="scope">
          <div>
            <el-button v-if="scope.row.status==1">启 用</el-button>
            <el-button v-else>禁 用</el-button>
          </div>
        </template>
      </el-table-column>
            <el-table-column label="操作">
        <template>
          <div>
            <el-button type="primary">编 辑</el-button>
            <el-button type="danger">删 除</el-button>
          </div>
        </template>
      </el-table-column>
```

6、列表功能完事了，回到add.vue，修改上级菜单部分

```js
// 1、取出状态层的列表数据
    computed: {
      ...mapGetters({
        menuList:"menu/list"
      }),
    }
    
// 2、使用menuList渲染页面中的下拉框
        <el-form-item label="上级菜单">
          <el-select v-model="form.pid" placeholder="请选择上级菜单" @change="changePid()">
            <el-option label="请选择" disabled value="aaa"></el-option>
            <el-option label="顶级菜单" :value="0"></el-option>
            <el-option v-for="item in menuList" :key="item.id" :value="item.id" :label="item.title"></el-option>
          </el-select>
        </el-form-item>
```

7、当添加成功之后还要重新请求列表数据

```js
      ...mapActions({
        reqMenuList:"menu/reqList"
      })
      
            submit(){
        menuAdd(this.form).then(res=>{
          if(res.data.code == 200){
            // 成功的弹窗
            successAlert(res.data.msg)
            // 关闭add.vue并清空form
            this.close()
            // 重新请求列表数据
            this.reqMenuList()
          }else{
            // 失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
```

##### 删除功能

1、封装删除的接口

```js
export const menuDel = (form)=>{
  return axios({
    url: baseUrl + "/api/menudelete",
    method:"post",
    data:qs.stringify(form)
  })
}
```

2、点击了删除按钮

```js
<el-button type="danger" @click="del(scope.row.id)">删 除</el-button>

      del(id){
        menuDel({id:id}).then(res=>{
          if(res.data.code == 200){
            // 删除成功的弹窗
            successAlert(res.data.msg)
            //  重新请求列表数据
            this.reqMenuList()
          }else{
            // 删除失败的弹窗
            warningAlert(res.data.msg)
          }
        })
```

##### 获取一条信息

1、先让add.vue显示出来，并且展示的提示字是菜单编辑

```js
// list.vue中
<el-button type="primary" @click="edit()">编 辑</el-button>
edit(id){
	this.$emit("edit")
}

// menu.vue
<v-list @edit="edit"></v-list>

edit(id){
	this.obj.isShow=true
	this.obj.isAdd=false
}

// add.vue
<el-dialog :title="obj.isAdd?'菜单添加':'菜单编辑'" :visible.sync="obj.isShow" width="40%" @close="close()">
    
<el-button type="primary" @click="submit()" v-if="obj.isAdd">添 加</el-button>
<el-button type="primary" @click="edit()" v-else>编 辑</el-button>
```

2、获取一条信息的功能

```js
// list.vue
<el-button type="primary" @click="edit(scope.row.id)">编 辑</el-button>

edit(id){
	this.$emit("edit",id)
}


// menu.vue
<v-list @edit="edit"></v-list>
<v-add :obj="obj" ref="pink"></v-add>
edit(id){
	this.obj.isShow=true
	this.obj.isAdd=false
	this.$refs.pink.getOne(id)
}


// add.vue
getOne(id){
	menuInfo(id).then(res=>{
		this.form = res.data.list
		// 因为后面编辑功能需要用到id，所以我们在获取一条信息的时候就将id添加到form中
		this.form.id = id
	})
}
```

##### 编辑功能

```js
// add.vue
      edit(){
        menuEdit(this.form).then(res=>{
          if(res.data.code == 200){
            // 编辑成功的弹窗
            successAlert(res.data.msg)
            // 关闭页面并清空form
            this.close()
            // 重新请求列表数据
            this.reqMenuList()
          }else{
            // 编辑失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
```

#### 9、role.vue

##### 添加功能

1、拆分页面

和menu一样，list.vue中就放了一个空表格，后期我们写完添加再处理。add中，放了一个form表单，又使用对话框把form套起来，对话框的显示与隐藏需要role.vue传递过来

```js
// role.vue
    data() {
      return {
        obj:{
          isShow:false
        }
      };
    }
    
    <el-button type="primary" @click="add()">添加</el-button>
      add(){
        this.obj.isShow = true
      }
      
// add.vue
props: ["obj"]
<el-dialog title="角色添加" :visible.sync="obj.isShow" width="40%">
```

2、add.vue中需要使用树形控件，并且数据是依赖于menu的列表，所以页面一进来就请求状态层的menu列表数据，然后绑定给我树形控件的data

```html
// html
	<el-form-item label="角色权限">
          <!-- 
            data  要渲染的数据
            show-checkbox   节点是否可以被选择，如果删掉就没有前面的复选框
            default-expanded-keys   默认展开的
            default-checked-keys    默认勾选的
            children  子节点的渲染
            label     展示给用户的

           -->
          <el-tree :data="menuList" show-checkbox node-key="id" :props="{children: 'children',label: 'title'}">
          </el-tree>
        </el-form-item>
```

```js
// 页面一进来请求状态层的menu列表数据
	computed: {
      ...mapGetters({
        menuList: "menu/list"
      }),
    },
    methods: {
      ...mapActions({
        menuReqList: "menu/reqList"
      }),
    },
    mounted() {
      // 一进来先请求menu的列表数据供我的树形控件使用
      this.menuReqList()
    }
```

3、点击了取消

```js
      // 清空
      empty(){
        this.form = {
          rolename:"",
          menus:"",
          status:1
        }
        // 因为树形控件和form没有任何联系，我们需要通过element-ui中提供的设置的方法来清空树形控件
        this.$refs.tree.setCheckedKeys([])
      },
      // 点击了取消
      close(){
        // 让add组件关闭
        this.obj.isShow = false
        // 调用清空
        this.empty()
      }
```

4、点击了添加按钮

```js
      submit(){
        // 因为树形控件和form没有任何联系，我们需要通过官网提供的方法获取属性控件选中的内容，然后添加到我的form中，在添加之前一定要转数据类型，获取到的是数组类型，后端需要的是字符串数组
        this.form.menus = JSON.stringify(this.$refs.tree.getCheckedKeys())
        roleAdd(this.form).then(res=>{
          if(res.data.code == 200){
            // 添加成功的提示窗
            successAlert(res.data.msg)
            // 关闭add并重置form
            this.close()
          }else{
            // 添加失败的提示窗
            warningAlert(res.data.msg)
          }
        })
      }
```

##### 列表功能

1、先去utils/request.js中封装接口

```js
export const roleList = ()=>{
  return axios({
    url:baseUrl + "/api/rolelist",
    method:"get"
  })
}
```

2、去store/modules下面创建定义role的状态层

```js
import {roleList} from "../../utils/request"

const state = {
  list:[]
}

const mutations = {
  changeList(state,arr){
    state.list = arr
  }
}

const actions = {
  reqList(context){
    roleList().then(res=>{
      context.commit("changeList",res.data.list)
    })
  }
}

const getters = {
  list(state){
    return state.list
  }
}

export default {
  state,
  mutations,
  actions,
  getters,
  namespaced: true
}
```

3、页面一进来就请求role的列表数据

```js
    computed: {
      ...mapGetters({
        roleList: "role/list"
      }),
    },
    methods: {
      ...mapActions({
        roleReqList: "role/reqList"
      })
    }
    
    mounted() {
      // 页面一进来先请求列表数据
      this.roleReqList()
    }
```

4、根据请求完的列表数据渲染页面

##### 删除功能

1、先封装删除接口

```js
export const roleDel = (form)=>{
  return axios({
    url:baseUrl + "/api/roledelete",
    method:"post",
    data:qs.stringify(form)
  })
}
```

2、点击了删除按钮去调用删除的接口

```js
      <el-table-column label="操作">
        <template slot-scope="scope">
          <div>
            <el-button type="primary">编 辑</el-button>
            <el-button type="danger" @click="del(scope.row.id)">删 除</el-button>
          </div>
        </template>
      </el-table-column>
      
            // 点击了删除
      del(id){
        roleDel({id:id}).then(res=>{
          if(res.data.code == 200){
            // 删除成功的弹窗
            successAlert(res.data.msg)
            // 重新请求列表数据
            this.roleReqList()
          }else{
            // 删除失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
```

##### 获取一条信息

1、先让add组件展示出来，并且让提示字变成角色编辑

```js
// list.vue
<el-button type="primary" @click="edit()">编 辑</el-button>
edit(){
	this.$emit("edit")
}

// role.vue
<v-list @edit="edit"></v-list>
add(){
	this.obj.isShow = true
	this.obj.isAdd = true
}
edit(id){
	this.obj.isShow = true
	this.obj.isAdd = false
}

// add.vue
props: ["obj"]
<el-dialog :title="obj.isAdd?'角色添加':'角色编辑'" :visible.sync="obj.isShow">
<el-button type="primary" @click="submit()" v-if="obj.isAdd">添 加</el-button>
<el-button type="primary" @click="edit()" v-else>编 辑</el-button>
```

2、获取一条信息的功能

```js
// list.vue传递id
<el-button type="primary" @click="edit(scope.row.id)">编 辑</el-button>
edit(id){
	this.$emit("edit",id)
}

// role.vue
<v-add :obj="obj" ref="pink"></v-add>
edit(id){
	this.obj.isShow = true
	this.obj.isAdd = false
	this.$refs.pink.getOne(id)
}

// add.vue
      getOne(id){
        roleInfo(id).then(res=>{
          this.form = res.data.list
          // 因为树形控件和form没有任何关系，所以树形控件不会渲染数据，我们需要使用element提供的方法给树形控件设置数据
          this.form.menus = JSON.parse(res.data.list.menus)
          this.$refs.tree.setCheckedKeys(this.form.menus)
          // 因为一会点击编辑的时候需要用到id，所以我们把id加入到form中
          this.form.id = id
        })
      }
```

##### 编辑功能

```js
      edit(){
        // 和添加的逻辑一模一样，先获取到树形控件选中的东西，然后转换成字符串数组加入到form中
        this.form.menus = JSON.stringify(this.$refs.tree.getCheckedKeys())
        roleEdit(this.form).then(res=>{
          if(res.data.code == 200){
            // 修改成功的提示窗
            successAlert(res.data.msg)
            // 关闭弹窗并清空form
            this.close()
            // 重新请求列表数据
            this.roleReqList()
          }else{
            // 修改失败的提示窗
            warningAlert(res.data.msg)
          }
        })
      }
```

#### 10、manage.vue

1、拆分页面，直接复制的menu

2、统一书写manage的接口

3、add.vue

```js
// 因为添加的时候，所属角色用到了role的列表数据，所以我们在这个页面需要调用一下role的列表数据
      ...mapGetters({
        // 角色列表数据
        roleList: "role/list"
      })
      ...mapActions({
        reqRoleList : "role/reqList"
      })
    mounted() {
      // 页面一进来请求role的列表数据供所属角色下拉框使用
      this.reqRoleList()
    }
    
// 请求完了数据，roleList就有数据了，就能渲染页面了
<el-form-item label="所属角色">
	<el-select v-model="form.roleid" placeholder="请选择所属角色">
	<el-option label="请选择" disabled value="aaa"></el-option>
	<el-option v-for="item in roleList" :key="item.id" :value="item.id" :label="item.rolename"></el-option>
	</el-select>
</el-form-item>
```

3、点击了添加按钮

```js
      submit() {
        manageAdd(this.form).then(res=>{
          if(res.data.code == 200){
            // 添加成功的提示窗
            successAlert(res.data.msg)
            // 关闭添加页面并重置form
            this.close()
            // 重新请求列表数据
            this.reqManageList()
          }else{
            // 添加失败的提示窗
            warningAlert(res.data.msg)
          }
        })
      }
```

4、书写manage的状态层

```js
import { manageList } from "../../utils/request"

const state = {
  list:[]
}
const mutations = {
  changeList(state,arr){
    state.list = arr
  }
}
const actions = {
  reqList(context){
    manageList({size:2,page:1}).then(res=>{
      context.commit("changeList",res.data.list)
    })
  }
}
const getters = {
  list(state){
    return state.list
  }
}

export default{
  state,
  mutations,
  actions,
  getters,
  namespaced: true
}
```

5、list.vue

```js
    computed: {
      ...mapGetters({
        manageList:"manage/list"
      }),
    },
    methods: {
      ...mapActions({
        reqManageList:"manage/reqList"
      })
     }
    mounted() {
      this.reqManageList()
    }
```

此刻已经有数据了，接下来就是渲染页面

6、删除功能。一定要注意需要的是uid

```js
<el-button type="danger" @click="del(scope.row.uid)">删 除</el-button>

      del(uid){
        manageDel({uid:uid}).then(res=>{
          if(res.data.code == 200){
            // 删除成功的弹窗
            successAlert(res.data.msg)
            //  重新请求列表数据
            this.reqManageList()
          }else{
            // 删除失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
```

7、获取一条信息。一定要注意需要的是uid

```js
// list.vue
<el-button type="primary" @click="edit(scope.row.uid)">编 辑</el-button>
      edit(uid){
        this.$emit("edit",uid)
      }
      
// manage.vue
<v-list @edit="edit"></v-list>
<v-add :obj="obj" ref="pink"></v-add>
edit(uid){
	this.obj.isShow=true
	this.obj.isAdd=false
	this.$refs.pink.getOne(uid)
}

// add.vue
getOne(uid) {
	manageInfo(uid).then(res => {
		this.form = res.data.list
		this.form.password = ""
	})
}
```

8、确定编辑功能

```js
      edit() {
        manageEdit(this.form).then(res => {
          if (res.data.code == 200) {
            // 编辑成功的弹窗
            successAlert(res.data.msg)
            // 关闭页面并清空form
            this.close()
            // 重新请求列表数据
            this.reqManageList()
          } else {
            // 编辑失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
```

#### 11、分页

1、去element-ui中复制分页

```html
    <!-- 
      background    背景颜色
      layout        布局
      total         总条数
      page-size     每页的条数
     -->
    <el-pagination background layout="prev, pager, next" :total="17" :page-size="2">
    </el-pagination>
```

2、后端提供了一个接口，这个接口是用来请求总条数的，我们在状态层去请求总条数，并把总条数导出给组件使用

```js
// 状态层
const state = {
  // 总条数
  total:0
}
const mutations = {
  changeTotal(state,num){
    state.total = num
  }
}
const actions = {
  // 请求总条数
  reqChangeTotal(context){
    manageCount().then(res=>{
      context.commit("changeTotal",res.data.list[0].total)
    })
  }
}
const getters = {
  total(state){
    return state.total
  }
}
// list.vue
...mapGetters({
	// 总条数
	total: "manage/total"
})
...mapActions({
	// 请求总条数
	reqChangeTotal: "manage/reqChangeTotal"
})
mounted() {
	// 请求列表数据
	this.reqManageList()
	// 请求总条数
	this.reqChangeTotal()
}

// list.vue中的分页组件
<el-pagination background layout="prev, pager, next" :total="total" :page-size="2">
</el-pagination>
```

3、组件需要使用size，状态层请求数据的时候也需要size，所以我们在状态层定义了一个size

```js
// 状态层
const state = {
  // 每页展示的数量
  size:2
}
const actions = {
  // 请求列表数据
  reqList(context){
    manageList({size:context.state.size,page:1}).then(res=>{
      context.commit("changeList",res.data.list)
    })
  }
}
const getters = {
  size(state){
    return state.size
  }
}

// list.vue
...mapGetters({
	// 每页展示的条数
	size: "manage/size"
})
<el-pagination background layout="prev, pager, next" :total="total" :page-size="size">
</el-pagination>
```

4、添加成功和删除成功之后都需要重新请求列表总条数

5、状态层需要知道你点击了哪一页，从而请求对应的数据

```js
// 状态层
const state = {
  // 当前页码数
  page:1
}
const mutations = {
  changePage(state,num){
    state.page = num
  }
}
const actions = {
  // 修改当前页码数
  reqChangePage(context,num){
    // 修改当前页码数
    context.commit("changePage",num)
  }
}
```

```js
// list.vue中
...mapActions({
	// 修改当前页码数
	reqChangePage: "manage/reqChangePage"
})
// 通过官网中提供的@current-change时间可以获取到你当前页码
<el-pagination background layout="prev, pager, next" :total="total" :page-size="size" @current-change="changePage">
changePage(e){
	this.reqChangePage(e)
}
```

6、bug

 假设当前在第3页，我点击了两次删除之后，会报错，同时数据也是不正确的

 先解决报错，把lsit提出来，如果没有数据的话后端返回的是null，null没有length这个属性，所有才会报错

```js
  reqList(context){
    manageList({size:context.state.size,page:context.state.page}).then(res=>{
      // 首先解决报错问题，因为没有数据后端返回的是null，我的想法是如果没有数据就让res.data.list=[]
      // list有两种情况，一种是有数据，另外一种是null，但是我们不想让他为null，如果为null就让他等于空数组
      const list = res.data.list?res.data.list:[]
      context.commit("changeList",list)
    })
  }
```

第二个问题页码变过来了，但是数据没有变过来

```js
  reqList(context){
    manageList({size:context.state.size,page:context.state.page}).then(res=>{
      console.log(context.state.page);
      // 首先解决报错问题，因为没有数据后端返回的是null，我的想法是如果没有数据就让res.data.list=[]
      // list有两种情况，一种是有数据，另外一种是null，但是我们不想让他为null，如果为null就让他等于空数组
      const list = res.data.list?res.data.list:[]
      // 判断如果list.length==0并且当前页码大于1那么就让page-1
      if(list.length==0 && context.state.page > 1){
        // 如果没有数据就让当前页码-1
        context.commit("changePage",context.state.page-1)
        // 重新请求列表数据
        context.dispatch("reqList")
        return
      }
      context.commit("changeList",list)
    })
  }
```

#### 12、cate.vue

1、拆分页面，复制的menu的所有东西

2、统一封装所有关于cate的接口

3、add.vue铺页面

3.1、需要注意的是文件上传

```html
// html
        <!-- 1、原生js -->
        <el-form-item label="图片">
          <div class="fileBox">
            <h3>+</h3>
            <input type="file" @change="changeInp" v-if="obj.isShow">
            <img :src="imgUrl" v-if="imgUrl">
          </div>
        </el-form-item>
        <!-- 2、element-ui -->
        <el-form-item label="图片">
          <div class="fileBox1">
            <el-upload class="avatar-uploader" action="#" :on-change="changeInp1">
              <img v-if="imgUrl" :src="imgUrl" class="avatar">
              <i v-else class="el-icon-plus avatar-uploader-icon"></i>
            </el-upload>
          </div>
        </el-form-item>
```

```css
// css
  .fileBox {
    width: 150px;
    height: 150px;
    border: 1px dashed #ccc;
    position: relative;
  }

  .fileBox h3 {
    font-size: 50px;
    line-height: 150px;
    text-align: center;
    margin: 0;
  }

  .fileBox input {
    width: 150px;
    height: 150px;
    position: absolute;
    left: 0;
    top: 0;
    opacity: 0;
    z-index: 2;
  }

  .fileBox img {
    width: 150px;
    height: 150px;
    position: absolute;
    left: 0;
    top: 0;
    z-index: 1;
  }
```

```js
// js
      changeInp(e) {
        // 获取到那个文件信息
        let files = e.target.files[0]
        console.log(files)
        // 限制图片大小，图片最大为2M。 1024B=1KB   1024KB=1MB
        if (files.size > 2 * 1024 * 1024) {
          warningAlert("图片大小超出限制")
          return
        }
        // 通过URL.createObjectURL可以将文件信息转换成一张具体的图片
        this.imgUrl = URL.createObjectURL(files)
        // 将文件信息放到form中传给后端
        this.form.img = files
      },
      //  当图片的上传框发生变化的时候
      changeInp1(e) {
        console.log(e)
        // 获取到那个文件信息
        let files = e.raw
        // 限制图片大小，图片最大为2M。 1024B=1KB   1024KB=1MB
        if (files.size > 2 * 1024 * 1024) {
          warningAlert("图片大小超出限制")
          return
        }
        // 转换成一张具体的图片
        this.imgUrl = URL.createObjectURL(files)
        // 和form里面的img是没有联系的，所以还要加入到form中
        this.form.img = files
      }
```

```js
// 去修改添加的接口
export const cateAdd = (form)=>{
  // 此刻form里面包含了文件，我们就需要模拟form表单提交
  let obj = new FormData()
  for(let i in form){
    obj.append(i,form[i])
  }

  return axios({
    url:baseUrl + "/api/cateadd",
    method:"post",
    data: obj
  })
}
```

4、list.vue

图片那个地方需要处理一下，因为后端返回的数据是服务器环境地址，我们需要在前面拼上http://localhost:3000

```js
// 在utils/request.js
import Vue from "vue"

// 开发环境
let baseUrl = "/api"
Vue.prototype.$imgUrl = "http://localhost:3000"
// 上线环境
// let baseUrl = ""
// Vue.prototype.$imgUrl= ""
```

在使用的时候

```html
      <el-table-column label="图片">
        <template slot-scope="scope">
          <div>
            <img :src="$imgUrl+scope.row.img" alt="">
          </div>
        </template>
      </el-table-column>
```

5、删除功能

```js
<el-button type="danger" @click="del(scope.row.id)">删 除</el-button>

      del(id) {
        cateDel({id: id}).then(res => {
          if (res.data.code == 200) {
            // 删除成功的弹窗
            successAlert(res.data.msg)
            //  重新请求列表数据
            this.reqCateList()
          } else {
            // 删除失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
```

6、获取一条信息

```js
      getOne(id) {
        cateInfo(id).then(res => {
          this.form = res.data.list
          // 因为图片路径和form没有关系，所以图片不能显示出来，我们需要给imgUrl设置路径
          this.imgUrl = this.$imgUrl + this.form.img
          // 因为后面编辑功能需要用到id，所以我们在获取一条信息的时候就将id添加到form中
          this.form.id = id
        })
      }
```

7、编辑功能

```js
// list.vue
	edit() {
        cateEdit(this.form).then(res => {
          if (res.data.code == 200) {
            // 编辑成功的弹窗
            successAlert(res.data.msg)
            // 关闭页面并清空form
            this.close()
            // 重新请求列表数据
            this.reqCateList()
          } else {
            // 编辑失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
      
      
// utils/request.js
export const cateEdit = (form)=>{
  // 因为编辑的数据里也涉及到了文件，所以也要模拟form提交
  let obj = new FormData()
  for(let i in form){
    obj.append(i,form[i])
  }

  return axios({
    url:baseUrl + "/api/cateedit",
    method:"post",
    data:obj
  })
}
```

#### 13、specs.vue

1、拆分页面，复制的manage的页面

2、统一书写接口

3、修改add.vue的页面

```js
// 规格属性部分
        <el-form-item label="规格属性" v-for="(item,index) in attrArr" :key="index">
          <div class="attrBox">
            <el-input v-model="item.value"></el-input>
            <el-button type="primary" v-if="index==0" @click="addAttr()">新增规格属性</el-button>
            <el-button type="danger" v-else @click="delAttr(index)">删除</el-button>
          </div>
        </el-form-item>
        
// js
      // 点击了新增规格属性
      addAttr(){
        this.attrArr.push({value:""})
      },
      // 点击了删除规格属性
      delAttr(index){
        this.attrArr.splice(index,1)
      }
     
// css
.attrBox{
  width: 100%;
  display: flex;
}
```

4、添加功能

```js
      submit() {
        // 现在:attrArr = [{value:"红"},{value:"蓝"},{value:"绿"}]
        // 处理后:attrArr = ["红","蓝","绿"]
        this.form.attrs = JSON.stringify(this.attrArr.map(item=>item.value))
        specsAdd(this.form).then(res => {
          if (res.data.code == 200) {
            // 添加成功的提示窗
            successAlert(res.data.msg)
            // 关闭添加页面并重置form
            this.close()
            // 重新请求列表数据
            // this.reqManageList()
            // 重新请求总条数
            // this.reqChangeTotal()
          } else {
            // 添加失败的提示窗
            warningAlert(res.data.msg)
          }
        })
      }
```

5、处理状态层

```js
  reqList(context){
    specsList({size:context.state.size,page:context.state.page}).then(res=>{
      console.log(context.state.page);
      // 首先解决报错问题，因为没有数据后端返回的是null，我的想法是如果没有数据就让res.data.list=[]
      // list有两种情况，一种是有数据，另外一种是null，但是我们不想让他为null，如果为null就让他等于空数组
      const list = res.data.list?res.data.list:[]
      // 判断如果list.length==0并且当前页码大于1那么就让page-1
      if(list.length==0 && context.state.page > 1){
        // 如果没有数据就让当前页码-1
        context.commit("changePage",context.state.page-1)
        // 重新请求列表数据
        context.dispatch("reqList")
        return
      }
      /* 
        list:[{id:1,attrs:"[a,b,c]",specsname:"名称"},{id:2,attrs:"[a,s,d]",specsname:"颜色"}]
      现在：list.attrs = "[a,b,c]"
      想要：list.attrs = [a,b,c]
      */
     list.forEach(item=>{
      item.attrs = JSON.parse(item.attrs)
     })
      context.commit("changeList",list)
    })
  }
```

6、list.vue中

```html
      <el-table-column label="规格属性">
        <template slot-scope="scope">
          <div>
            <el-tag v-for="item in scope.row.attrs" :key="item">{{item}}</el-tag>
          </div>
        </template>
      </el-table-column>
```

7、删除

```js
      del(id) {
        specsDel({id: id}).then(res => {
          if (res.data.code == 200) {
            // 删除成功的弹窗
            successAlert(res.data.msg)
            //  重新请求列表数据
            this.reqSpecsList()
            // 重新请求列表总条数
            this.reqChangeTotal()
          } else {
            // 删除失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
```

8、获取一条信息

```js
      getOne(id) {
        specsInfo(id).then(res => {
          this.form = res.data.list[0]
          /* 
            现在："['a','b','c','d']"
            想要：[{value:a},{value:b},{value:c},{value:d}]
            第一步先把字符串数组转换成数组之后再处理  ['a','b','c','d']
          */
          //  第一种（正常）
          // this.attrArr = JSON.parse(this.form.attrs).map(item => {
          //   return {
          //     value: item
          //   }
          // })
          //  第二种（简化）
          this.attrArr = JSON.parse(this.form.attrs).map(item=>({value:item}))
        })
      }
```

9、编辑功能

```js
      edit() {
        this.form.attrs = JSON.stringify(this.attrArr.map(item=>item.value))
        specsEdit(this.form).then(res => {
          if (res.data.code == 200) {
            // 编辑成功的弹窗
            successAlert(res.data.msg)
            // 关闭页面并清空form
            this.close()
            // 重新请求列表数据
            this.reqSpecsList()
          } else {
            // 编辑失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
```

#### 14、goods.vue

1、拆分页面（复制的manage）

2、统一封装接口

3、add.vue

3.1、页面一进来请求分类列表（一级分类）

```js
	// 页面
	...mapGetters({
        // 商品分类列表
        cateList: "cate/list"
      })
       ...mapActions({
        // 商品分类列表请求
        reqCateList: "cate/reqList"
      })
      mounted() {
      // 页面一进来就请求商品分类列表数据
      this.reqCateList()
    }
```

```js
// utils/request.js中改了一下接口的传参
export const cateList = (form)=>{
  return axios({
    url:baseUrl + "/api/catelist",
    method:"get",
    params:form
  })
}
```

```js
// cate.js的状态层
const actions = {
  reqList(context){
    cateList({istree:true}).then(res=>{
      context.commit("changeList",res.data.list)
    })
  }
}
```

```html
// add.vue页面中
        <el-form-item label="一级分类">
          <el-select v-model="form.first_cateid" placeholder="请选择一级分类" @change="changeFirst()">
            <el-option v-for="item in cateList" :key="item.id" :label="item.catename" :value="item.id"></el-option>
          </el-select>
        </el-form-item>
```

3.2、当一级分类发生变化的时候请求二级分类，并根据请求的结果渲染二级分类的下拉栏

```js
//	不要忘了在data中定义一个变量，用来渲染二级分类下拉列表的secondArr:[]
	changeFirst() {
        cateList({
          pid: this.form.first_cateid
        }).then(res => {
          if (res.data.code == 200) {
            // 渲染二级分类列表
            this.secondArr = res.data.list
            // 清空二级分类选中的那一项
            this.form.second_cateid = ""
          }
        })
      }
```

```html
// 此刻二级分类的数据有值了，我们就能渲染页面了
        <el-form-item label="二级分类">
          <el-select v-model="form.second_cateid" placeholder="请选择二级分类">
            <el-option v-for="item in secondArr" :key="item.id" :label="item.catename" :value="item.id"></el-option>
          </el-select>
        </el-form-item>
```

3.3、处理图片

```js
        <el-form-item label="图片">
          <div class="fileBox">
            <h3>+</h3>
            <input type="file" @change="changeInp" v-if="obj.isShow">
            <img :src="imgUrl" v-if="imgUrl">
          </div>
        </el-form-item>
        
        changeInp(e) {
        // 获取到那个文件信息
        let files = e.target.files[0]
        console.log(files)
        // 限制图片大小，图片最大为2M。 1024B=1KB   1024KB=1MB
        if (files.size > 2 * 1024 * 1024) {
          warningAlert("图片大小超出限制")
          return
        }
        // 通过URL.createObjectURL可以将文件信息转换成一张具体的图片
        this.imgUrl = URL.createObjectURL(files)
        // 将文件信息放到form中传给后端
        this.form.img = files
      }
```

3.4、页面一进来请求商品规格

```js
        ...mapGetters({
          // 商品规格
          specsList: "specs/list",
        })
        ...mapActions({
        // 规格属性列表请求
        reqSpecsList: "specs/reqList",
      })
     mounted() {
      // 页面一进来就请求规格属性列表数据
      this.reqSpecsList(true)
    }
```

```js
// 在sotre/modules/specs.js中
  reqList(context,bool){
    // 如果bool为undefined的话那么就表示要获取到具体哪一页的数据，否则就表示要获取所有的数据
    // 此时此刻所有调用reqList的地方，都没有传递bool，那么bool就等于undefined
    // 此时此刻bool的值true
    bool = bool?{}:{size:context.state.size,page:context.state.page}
      
    specsList(bool).then(res=>{
      const list = res.data.list?res.data.list:[]
      // 判断如果list.length==0并且当前页码大于1那么就让page-1
      if(list.length==0 && context.state.page > 1){
        // 如果没有数据就让当前页码-1
        context.commit("changePage",context.state.page-1)
        // 重新请求列表数据
        context.dispatch("reqList")
        return
      }
     list.forEach(item=>{
      item.attrs = JSON.parse(item.attrs)
     })
      context.commit("changeList",list)
    })
  }
```

```html
// add.vue中html部分
        <el-form-item label="商品规格">
          <el-select v-model="form.specsid" placeholder="请选择商品规格" @change="changeSpecs()">
            <el-option v-for="item in specsList" :key="item.id" :label="item.specsname" :value="item.id"></el-option>
          </el-select>
        </el-form-item>
```

```js
// 当商品规格发生变化的时候我们需要修改规格属性的下拉栏，不要忘了在data中定义个以数据用来渲染规格属性列表
        <el-form-item label="规格属性">
          <el-select v-model="form.specsattr" multiple placeholder="请选择规格属性">
            <el-option v-for="item in specsArr" :key="item" :label="item" :value="item">
            </el-option>
          </el-select>
        </el-form-item>
        
              changeSpecs() {
        // 要做的事是：给specsArr赋值，值是对应的商品规格下面的规格属性
        let obj = this.specsList.find(item => item.id == this.form.specsid)
        this.specsArr = obj.attrs
        // 清空之前选中的规格属性
        this.form.specsattr = []
      }
```

3.5、点击添加功能

一定不要忘了修改封装的接口，因为涉及到了文件上传

```js
      submit() {
        // 规格属性一定要转换成字符串数组之后再添加
        this.form.specsattr = JSON.stringify(this.form.specsattr)
        goodsAdd(this.form).then(res => {
          if (res.data.code == 200) {
            // 添加成功的提示窗
            successAlert(res.data.msg)
            // 关闭添加页面并重置form
            this.close()
          } else {
            // 添加失败的提示窗
            warningAlert(res.data.msg)
          }
        })
      }
```

3.6、列表

赋值manage的状态层，然后将状态层里面的接口改成关于goods的接口，最后渲染页面

写完列表之后不要忘了回到add.vue中在添加成功的时候还要重新请求列表数据和商品总数

3.7、删除功能

```js
      del(id) {
        goodsDel({
          id: id
        }).then(res => {
          if (res.data.code == 200) {
            // 删除成功的弹窗
            successAlert(res.data.msg)
            //  重新请求列表数据
            this.reqgoodsList()
            // 重新请求列表总条数
            this.reqChangeTotal()
          } else {
            // 删除失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
```

3.8、获取一条信息

```js
      getOne(id) {
        goodsInfo(id).then(res => {
          // 1、将请求来的数据赋值给form
          this.form = res.data.list
          // 2、因为在后面点击编辑的时候需要用到了id
          this.form.id = id
          // 3、根据获取到的一级分类请求二级分类的数据，渲染给二级分类列表
          cateList({pid: this.form.first_cateid}).then(res=>{
            this.secondArr = res.data.list
          })
          // 4、处理图片的展示
          this.imgUrl = this.$imgUrl + this.form.img
          // 5、根据获取到的商品规格来查找对应的规格属性渲染到页面中
          // 这两行是用来渲染下拉列表的
          let obj = this.specsList.find(item=>item.id==this.form.specsid)
          this.specsArr = obj.attrs
          // 这一行是用来选中哪一个的
          this.form.specsattr = JSON.parse(this.form.specsattr)
        })
      }
```

3.9、确定编辑功能

一定不要忘了修改封装的接口，因为涉及到了文件上传

```js
      edit() {
        // 规格属性一定要转换成字符串数组之后再添加
        this.form.specsattr = JSON.stringify(this.form.specsattr)
        goodsEdit(this.form).then(res => {
          if (res.data.code == 200) {
            // 编辑成功的弹窗
            successAlert(res.data.msg)
            // 关闭页面并清空form
            this.close()
            // 重新请求列表数据
            this.reqGoodsList()
          } else {
            // 编辑失败的弹窗
            warningAlert(res.data.msg)
          }
        })
      }
```

#### 15、富文本编辑器

1、官网：https://www.wangeditor.com/

2、安装方式：

```
NPM方式：npm i wangeditor --save
CND方式：<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/wangeditor@latest/dist/wangEditor.min.js"></script>
```

3、使用

```js
// 引入
import E from "wangeditor"

// 对话框打开动画结束时创建富文本编辑器
opened() {
	// 创建富文本编辑器
	this.editor = new E('#box')
	this.editor.create()

	// 设置富文本编辑器的内容
	this.editor.txt.html(this.form.description)
}

// 在添加的时候，要先把编辑器的内容加入到form中，再调用接口
this.form.description = this.editor.txt.html()
```

#### 16、权限

1、登录交互

```js
      submit(){
        login(this.user).then(res=>{
          if(res.data.code==200){
            // 1、登录成功的提示窗
            successAlert(res.data.msg)
            // 2、将用户信息存储到状态层
            this.reqUserInfo(res.data.list)
            // 3、进入页面
            this.$router.push("/")
          }else{
            // 登录失败的提示窗
            warningAlert(res.data.msg)
          }
        })
```

2、设置状态层

```js
// actions.js
const actions = {
  reqUserInfo(context,obj){
    context.commit("changeUserInfo",obj)
  }
}
export default actions

// mutations
export const state = {
    userInfo:sessionStorage.getItem("userInfo")?JSON.parse(sessionStorage.getItem("userInfo")):{}
}
export const mutations = {
  changeUserInfo(state,obj){
    state.userInfo = obj
  }
}
export const getters = {
  userInfo(state){
    return state.userInfo
  }
}
```

3、登录拦截

```js
router.beforeEach((to,form,next)=>{
  // 如果到达的路径是/login那么就不用拦他，直接放行
  if(to.path=="/login"){
    next()
    return
  }
  // store里面的userInfo中是否包含token，如果包含说明登陆过了就放行，否则说明没登录，没登录就要拦截

  if(store.state.userInfo.token){
    next()
    return
  }

  next("/login")
})
```

4、路由独享守卫

```js
// 封装拦截
function changeEnter(path,next){
  // 取出你能去的路径
  let routerUrl = store.state.userInfo.menus_url
  // 判断能去的路径中是否包含你想去的路径
  if(routerUrl.includes(path)){
    next()
  }else{
    next("/")
  }
}
// 调用的时候示例
{
  path: "menu",
  name: "菜单管理",
  beforeEnter:(to,from,next)=>{
    changeEnter("/menu",next)
  },
  component: () => import("../page/menu/menu.vue")
}, {
  path: "role",
  name: "角色管理",
  beforeEnter:(to,from,next)=>{
    changeEnter("/role",next)
  },
  component: () => import("../page/role/role.vue")
}
```

#### 17、动态侧边栏

index.vue侧边栏部分

```html
            <el-menu class="el-menu-vertical-demo" default-active="0" unique-opened background-color="rgb(32,34,42)"
              text-color="#fff" active-text-color="#ffd04b" router>

              <el-menu-item index="/">
                <i class="el-icon-s-home"></i>
                <span>首页</span>
              </el-menu-item>
              <!-- 两种情况，一种是目录，一种是菜单 -->
              <div v-for="item in userInfo.menus" :key="item.id">
                <!-- 目录 -->
                <el-submenu :index="item.id+''" v-if="item.children">
                  <template slot="title">
                    <i :class="item.icon"></i>
                    <span>{{item.title}}</span>
                  </template>
                  <el-menu-item v-for="i in item.children" :key="i.id" :index="i.url">{{i.title}}</el-menu-item>
                </el-submenu>
                <!-- 菜单 -->
                <el-menu-item :index="item.url" v-else>{{item.title}}</el-menu-item>
              </div>
            </el-menu>
```

头部退出

```js
        <el-header>
          <p>账号：{{userInfo.username}}</p>
          <el-button @click="goOut()">退 出</el-button>
        </el-header>
        
              goOut(){
        // 清空状态层的数据
        this.reqUserInfo({})
        // 跳转到登录页
        this.$router.push("/login")
      }
```

修改状态层

```js
// mutations.js
  changeUserInfo(state, obj) {
    //1、修改state中的userInfo
    state.userInfo = obj

    if (obj.token) {
      //2、将用户信息存储到本地存储，因为本地存储会将数据转换成字符串类型，所以我们自己先转一下
      // 如果token存在说明是登录，登录就要存本地
      sessionStorage.setItem("userInfo", JSON.stringify(obj))
    } else {
      // 如果token不存在说明是退出，退出就要清空本地
      sessionStorage.removeItem("userInfo")
    }
  }
```

#### 18、请求拦截

1、在后台App.js中打开注释的那一段，每次请求的时候都需要在请求头中携带token才能请求到数据

2、utils/request.js

```js
import store from "../store/index.js"

axios.interceptors.request.use(config=>{

  if(config.url != baseUrl+"/api/login"){
    config.headers.authorization = store.state.userInfo.token
  }

  return config
})
```

#### 19、过滤器

1、在filters文件夹下创建filterPrice.js文件

```js
export function filterPrice(price){
  return price.toFixed(2)
}
```

2、在filters/index.js中

```js
import Vue from "vue"

import {filterPrice} from "./filterPrice"

let obj = {
  filterPrice
}

for(let i in obj){
  Vue.filter(i,obj[i])
}
```

3、使用

```html
// 以商品管理页为例
      <el-table-column label="商品价格">
        <template slot-scope="scope">
          <div>
            <span>{{scope.row.price | filterPrice}}</span>
          </div>
        </template>
      </el-table-column>
```

