<template>
  <div class="article-list" @scroll="getArticleDown($event)" ref="articleRef">
    <ul>
     <li v-for="article in articleList">
        <div class="user-avatar">
            <img :src="article.author.avatar_url" alt=""/>
        </div>
        <div class="article-tab" >
            <span :class="{active: article.good || article.top}" >{{article.tab | getTabName(article.top, article.good) }}</span>
        </div>
       <router-link :to="{name: 'topic', params:{id:article.id}}">
         <span class="article-title">{{article.title}}</span>
       </router-link>
       <div class="article-visit">
         <span><img src="../../assets/visit.svg" alt="">{{article.visit_count}}</span>
       </div>
       <div class="article-reply">
         <span><img src="../../assets/reply.svg" alt="">{{article.reply_count}}</span>
       </div>
       <div class="article-last-time">{{article.last_reply_at | getTime }}</div>
      </li>
    </ul>
    <back-top v-bind:limit="limit,articleList"></back-top>
  </div>
</template>

<script>
  import backTop from "../commonpage/backTopComp"

  export default{
    data(){
      return {
        limit: 20,
      	articleList:[],
        timer: null
      }
    },
    created(){
        this.getArticleList();
    },
    methods: {
        getArticleList(){
            let url = !this.$route.query.tab?'/topics?page=1&limit='+ this.limit :'/topics?page=1&limit='+ this.limit +'&tab='+ this.$route.query.tab
            this.$http.get(url)
            .then((res)=>{
                this.$set(this.$data, 'articleList', res.data.data);
            })       
            .catch((error)=>{
                console.log(error);
            });
        },
        getArticleDown(event){
            let scrollTop = event.target.scrollTop;
            let scrollHeight = event.target.scrollHeight;
            let clientHeight = event.target.clientHeight;
            if( scrollHeight - clientHeight == scrollTop ) {
                this.limit += 40;
                this.getArticleList();
            };
        }
    },
    watch: {
        '$route' (to, from) {
             // 对路由变化作出响应...
             this.getArticleList()
         }
    },
    components: {
        backTop
    }
  }

</script>

<style lang="less" scoped>
  .article-list {
    width: 100%;
    height: 100%;
    padding-bottom: 42px;
    box-sizing: border-box;
    overflow-y: scroll;
    li {
      width: 100%;
      padding: 0 15px;
      background: #FFF;
      overflow: hidden;
      align-items: center;
      border-bottom: 1px solid #CCC; 
      .user-avatar {
        float: right;
        width: 50%;
        height: 36px;
        display: flex;
        justify-content: flex-end;
        align-items: flex-end;
        img {
          width: 30px;
          height: 30px;
          border-radius: 6px;
        }
      }
      .article-tab {
        float: left;
        width: 50%;
        height: 36px;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        span {
            padding: 2px;
            border-radius: 2px;
            color: #FFF;
            background: #ccc;
            &.active{
            background-color: #80bd01; 
          }
        }
      }
      a {
          float: left;
          display: block;
          padding: 10px 0;
          width: 100%;
          border-bottom: 1px dashed #DDD;
      }
      .article-visit, .article-reply ,.article-last-time  {
          float: left;
          margin: 5px 0;
          width: 33%;
          height: 20px;
          line-height: 20px;
          text-align: center;
          img {
              width: 16px;
              height: 16px;
              margin-right: 4px; 
              vertical-align: middle;
          }
      }
      .article-reply {
          border-left: 1px solid #DDD;
          border-right: 1px solid #DDD;
      }
    }  
  }
</style>
