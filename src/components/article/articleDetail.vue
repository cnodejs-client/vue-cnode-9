<template>
  <div class="article-detail" ref="articleDetail">
    <div class="article-title">
      <div class="title">
        <span v-show="topics.top || topics.good">{{topics.tab | getTabName(topics.top, topics.good)}}</span>
        <span class="title">{{topics.title}}</span>
      </div>
      <div class="author-info">
        <span class="author">作者：{{author.loginname}}</span>
        <span class="origin">来自：{{topics.tab | getTabName}}</span>
      </div>
      <div class="article-info">
        <span class="line">发布于 {{topics.create_at | getTime}}</span>
        <span class="line">最后编辑 {{topics.last_reply_at | getTime}}</span>
        <span class="line">{{topics.visit_count}} 浏览</span>
        <span @click="collectTopic">{{isCollectTopic ? '已收藏' : '收藏'}}</span>
      </div>
    </div>
    <div class="article-content">
      <div v-html="topics.content"></div>
    </div>
    <div class="article-reply">
      <span class="reply-count">共 {{replies.length}} 条回复</span>
      <div class="reply-list" v-for="(reply, index) in replies">
        <div class="reply-title">
          <div class="avatar">
            <img :src="reply.author.avatar_url"  alt="">
          </div>
          <div class="name line">
            {{reply.author.loginname}}
          </div>
          <div class="floor line">
            {{index + 1}}层
          </div>
          <div class="time line">
           {{reply.create_at | getTime}}
          </div>
        </div>
        <div class="reply-content" v-html="reply.content"></div>
      </div>
    </div>
    <form class="article-comment" @submit.prevent="createReplies">
      <textarea v-model = "createReplyContent" class="comment-content" placeholder="吐个槽吧~"></textarea>
      <input type="submit" :disabled="!createReplyContent.length" :class="{active:createReplyContent.length}" class="btn" value="Biu~">
    </form>
  </div>
</template>

<script>
  import { ACCESS_TOKEN } from '../../config'

  export default {
    data() {
      return {
        author: {},
        topics: {},
        replies: [],
        createReplyContent: '',
        isCollectTopic:false
      }
    },
    created(){
      this.getArticleDetail();
    },
    methods:{
      getArticleDetail(){
        this.$http.get(`/topic/${this.$route.params.id}`,{
          params: {
            accesstoken: ACCESS_TOKEN
          }
        })
        .then((data) => {
          this.author = data.data.data.author;
          this.topics = data.data.data;
          this.replies = data.data.data.replies;
          this.isCollectTopic = data.data.data.is_collect;
        })
        .catch((error) => {
          console.log(error);
        })
      },
      collectTopic() {
        if(this.isCollectTopic) {
          this.deCollected();
        }else {
          this.collected();
        };
      },
      collected() {
        this.$http.post('/topic_collect/collect', {
          accesstoken: ACCESS_TOKEN,
          topic_id: this.$route.params.id
        })
        .then((res) => {
          this.isCollectTopic = true;
          console.group("收藏成功");
          console.log(res.data);
          console.groupEnd();
        })
        .catch((error) => {
          console.group("失败数据");
          console.log(error);
          console.groupEnd();
        })
      },
      deCollected(){
        this.$http.post('/topic_collect/de_collect',{
          accesstoken: ACCESS_TOKEN,
          topic_id: this.$route.params.id
        })
        .then((res)=>{
          this.isCollectTopic = false;
          console.group("取消收藏成功");
          console.log(res.data)
          console.groupEnd()
        })
        .catch((error)=>{
          console.group("失败");
          console.log(error);
          console.groupEnd();
        })
       },
      createReplies(){
        this.$http.post(`/topic/${this.$route.params.id}/replies`, {
          accesstoken: ACCESS_TOKEN,
          content: this.createReplyContent
        })
        .then((res) => {
          this.createReplyContent = "";
          this.getArticleDetail();
          this.$refs.articleDetail.scrollTop = this.$refs.articleDetail.scrollHeight;
        })
        .catch((error) => {
          console.log(error);
        })
      }
    },
    components: {

    }
  }
</script>

<style lang="less" scoped>
  .article-detail {
    position: absolute;
    top: 20%;
    padding: 0 10px;
    padding-bottom: 31px;
    width: 100%;
    height: 80%;
    background: #FFF;
    overflow-y: scroll; 
    .article-title {
      margin-bottom: 10px;
      padding: 5px 0;
      border-bottom: 1px solid #ccc;
      .title {
        span:first-child {
          padding: 2px;
          color: #FFF;
          border-radius: 2px;
          background: #80bd01;
        }
        span:last-child {
          color: #333;
          font-weight: bold;
        }
      }
      .author-info {
        display: flex;
        flex-flow: row nowrap;
        align-items: center;
        justify-content: space-between;
        align-content: center;
        padding: 5px 0; 
      }
      .article-info {
        .line {
          padding-right: 6px;
          border-right: 1px solid #ccc;
        }
      }
    }
    .article-content {
      border-bottom: 1px solid #ccc;
    }
    .article-reply {
      .reply-count {
        color: #F00;
      }
      .reply-list {
        border-bottom: 1px dashed #ccc;
        .reply-title {
          padding: 5px 0;
          display: flex;
          flex-flow: row nowrap;
          align-items: center;
          .avatar {
            width: 30px;
            height: 30px;
            border-radius: 6px;
            overflow: hidden;
          }
        }
        .reply-content {
          padding: 5px 0;
        }
      }
      .line {
        margin-left: 6px;
        padding-right: 6px;
        border-right: 1px solid #ccc;
        &:last-child {
          border: 0;
        }
      }
    }
    .article-comment{
      position: fixed;
      left: 0;
      bottom: 0;
      width: 100%;
      height: 32px;
      display: flex;
      flex-flow: row nowrap;
      align-items: center;
      justify-content: space-between;
      textarea.comment-content{
        flex-basis: 75%;
        height: 32px;
        padding: 5px;
        box-sizing: border-box;
        border: 1px solid gainsboro;
        border-radius: 0;
        -webkit-tap-highlight-color: transparent;
        box-shadow: none;
        outline: none;
      }
      input.btn{
        height: 32px;
        flex-basis: 25%;
        border: none;
        background: rgba(128,189,1,.5);
        color: #fff;
        border-radius: 0;
        -webkit-tap-highlight-color: transparent;
        &.active{
          background: #80BD01;
          color: #fff;
        }
      }
    }
  }
</style>