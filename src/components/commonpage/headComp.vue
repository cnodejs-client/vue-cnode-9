<template>

    <div class="top">
      <div class="logo">
        <img src="../../assets/cnodejs_light.svg"/>
      </div>
      <div class="search">
        <form method="get" action="http://www.baidu.com/s">
          <input type="text" class="search-input" name="wd" autocomplete="off"/>
        </form>
      </div>
      <div class="leder">
        <ul>
          <li v-for="nav in navs" class="leder-li">
            <router-link :to="nav.path" class="leder-a" tag="a">{{nav.name}}</router-link>
          </li>
        </ul>
      </div>
    </div>
</template>
<style lang="less">
  .top {//class="top"的样式
    position: fixed;
    top: 0;
    height: 20%;
    width:100%;
    background-color: #444;
    text-align:center;
    .logo {
      img {
          width: 30%;
      }
      padding:10px 0;
    }
    .search{
      .search-input {
        border-radius: 15px;
        width: 60%;
        height: 25px;
        background:url("../../assets/search.png") 5px 3px no-repeat #888;
        padding-left: 22px;
        &:hover{
          background-color: white;
        }
      }
    }
    .leder{
      .leder-li {
        display: inline-block;
        padding: 20px 3% 20px 3%;
        &:hover{
          opacity: 0.3;
        }
        .leder-a {
          color: white;
        }
      }
    }
  }
</style>
<script>
   export default{
    data(){
      return {
        navs: [{
          path: '/',
          name: '首页'
        }, {
          path: '/topics/create',
          name: '发布话题'
        }, {
          path: '/my/message',
          name: '消息通知'
        }]
      }
    },
    created(){

    },
    methods: {
    },
    components: {}
  }
</script>
