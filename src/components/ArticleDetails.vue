<template>
  <div class="main">
    <div class = "title">
      <h2>{{ article.title }}</h2>
    </div>
    <div class = "extra">
      <div class="date">{{article.date}}</div>
      <div class="author">{{article.username}}</div>
    </div>
    <div class = "content" v-html="article.content"></div>
    <div class = "comments">
      <div class="addComment">
        <el-input v-model="commentadd" class='inputClass'></el-input>
        <el-button type="primary" size="mini" @click="DoComment()">评论</el-button>
      </div>
      <ul>
        <li v-for="item in article.comments" :key="item">
          <div class="header">
            <div class="author">{{item.user}}</div>
          </div>
          <div class = "comment">{{item.content}}</div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      article: {},
      comments: [],
      commentadd: ''
    }
  },
  mounted: function () {
    this.init()
  },
  methods: {
    init: function () {
      var that = this
      var nid = this.$route.params.id// 获取id
      this.$axios.request({
        url: 'http://localhost:8081/article/' + nid,
        method: 'GET',
        responseType: 'json'
      }).then(function (response) {
        console.log(response.data)
        that.article = response.data.ok
      })
    },
    DoComment: function () {
      var nid = this.$route.params.id
      if (this.$store.state.username) {
        var that = this
        if (!this.commentadd) {

        } else {
          this.$axios.request({
            url: 'http://localhost:8081/article/' + nid + '/comment',
            method: 'Post',
            data: JSON.stringify({
              User: that.$store.state.username,
              Article_id: parseInt(nid),
              Date: '2020-12-12',
              Content: this.commentadd
            }),
            headers: {
              'Authorization': that.$store.state.token
            },
            responseType: 'json'
          }).then(function (response) {
            console.log(response.data)
            if (!response.data.error) {
              that.article.comments = []
              that.article.comments.push({
                user: response.data.ok.user,
                article_id: response.data.ok.article_id,
                date: response.data.ok.date,
                content: response.data.ok.content
              })
              that.commentadd = ''
            } else {
              alert(response.data.error)
            }
          })
        }
      } else {
        this.$router.push('/login')
      }
    }
  }
}
</script>

<style scoped>
.main {
  width: 1000px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  margin: auto;
  margin-bottom: 100px;
}
.extra {
  bottom: 10px;
  height: 20px;
  font-size: 12px;
}
.author {
  height: 20px;
  float: left;
  margin-left: 20px;
  line-height: 20px;
  color:#909399;
}
.date {
  height: 20px;
  float: left;
  margin-left: 25px;
  line-height: 20px;
  color:#C0C4CC;
}
.content {
  margin: auto;
  margin-top: 10px;
  margin-bottom: 10px;
  border-top: 1px solid #DCDFE6;
  border-bottom: 1px solid #DCDFE6;
  width: 960px;
  text-align: left;
  color: #606266
}
.comments {
  width: 940px;
  margin: auto;
}
.header {
  height: 20px;
}
.header .author {
  color: black;
  font-weight: bold;
  font-size: 14px;
  margin-left: 0px;
}
.header .date {
  font-size: 12px;
}
.comment {
  margin-top: 20px;
  margin-left: 20px;
  text-align: left;
  color: #606266
}
.comments li {
  list-style-type :none;
  padding: 8px;
  border-top: 1px dashed #DCDFE6;
}
.comments li:first-child {
  border-top: 1px #DCDFE6;
}
.addComment {
  height: 90px;
}
.el-button {
  float: right;
  margin-top: 10px;
}
</style>
