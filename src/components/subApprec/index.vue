<template lang="html">
  <div class="df-subApprec">
    <header class="header">
      <router-link class="title" :to="apprec.apprecUrl">{{ apprec.title }}</router-link>
      <div class="refer">
        <span>源自诗文:</span>
        <router-link class="refer-poem" :to="apprec.refer.poemUrl">{{ apprec.refer.title }}</router-link>
      </div>
    </header>
    <div class="main-wrapper">
      <div class="main">
        <div class="info">
          <span>
            <i class="fa fa-user"></i><router-link to="apprec.profileUrl">{{ apprec.authorName }}</router-link>
          </span>
          <span>
            <i class="fa fa-clock-o"></i>{{ apprec.publish_time }}
          </span>
          <span v-if="apprec.pageviews_count>0">
            <i class="fa fa-eye"></i>{{ apprec.pageviews_count }}
          </span>
        </div>
        <div class="body">
          <div class="content" ref="body" v-html="apprec.body" @click="toggleExpand" v-if="isExpand"></div>
          <div class="content" ref="body" v-html="summary" @click="toggleExpand" v-else></div>
        </div>
      </div>
      <div class="side">
        <el-button class="btn-default">
          <i class="fa fa-thumbs-o-up"></i>
        </el-button>
        <el-button class="btn-default">
          <i class="fa fa-heart-o"></i>
        </el-button>
      </div>
    </div>
    <div class="misc">
      <div class="c-tags" v-if="apprec.tags.length > 0">
        <i class="fa fa-tags"></i>
        <el-tag v-for="(tag,index) in apprec.tags" :key="index" class="tag">{{ tag.name }}</el-tag>
      </div>
      <div class="feedback">
        <a v-if="apprec.comments_count>0" @click="toggleCommentDialog">
          <i class="fa fa-comment-o"></i>{{ apprec.comments_count }}&nbsp;条评论
        </a>
        <a>
          <i class="fa fa-share"></i>分享
        </a>
      </div>
    </div>
    <el-dialog class="comment-dialog" v-model="commentDialogVisible">
      <comment
        :comments="apprec.comments"
        :modelId="apprec.id"
        :isIndex="true"
      ></comment>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'subApprec',
  data() {
    return {
      commentDialogVisible: false,
      isExpand: false,
      apprec: {
        id: 1,
        title: '雪阳',
        apprecUrl: '/appreciation',
        refer: {
          title: '嘻哈之王',
          poemUrl: '/appreciation'
        },
        authorName: 'dragonfly',
        profileUrl: '/appreciation',
        publish_time: '6分钟前',
        pageviews_count: 12,
        summary: 'sdasadsadasdasdasdasdsadasdadasdsa',
        body: 'sdasadsadasdasdasdasdsadasdadasdsasdasadsadasdasdasdasdsadasdadasdsasdasadsadasdasdasdasdsadasdadasdsa',
        comments: [],
        comments_count: 0,
        tags: [
          {
            name: 'adas'
          }
        ]
      }
    }
  },
  computed: {
    summary() {
      const body = this.apprec.body
      let summary = this.apprec.summary
      if (body.length > summary.length) {
        summary += '<span class="ellipsis">...</span>'
      }
      return summary
    }
  },
  methods: {
    toggleExpand() {
      this.isExpand = !this.isExpand
      if (this.isExpand) {
        this.$refs.body.className += ' is-collapse'
      } else {
        this.$refs.body.className = this.$refs.body.className.replace(/(\s|^)+is-collapse{1}(\s|$)+/, '')
      }
    },
    toggleCommentDialog() {
      this.commentDialogVisible = !this.commentDialogVisible
    }
  }
}
</script>

<style lang="stylus" scoped>
@import '../../common/stylus/common'

.df-subApprec
  margin 20px 0
  border-bottom 1px solid Extra-Light-Grey
  .header
    fj(space-between)
    a
      text-decoration none
    .title
      font(2em,mgreen(.8))
      transition all .3s
      &:hover
        color Green
        transform translateY(-3px)
    .refer
      margin-left 5px
      padding-top 22px
      font-size .8em
      color Light-Silver
      .refer-poem
        color Light-Silver
        &:hover
          color Red
  .main-wrapper
    fj(space-between)
    .main
      .info
        margin 5px 0
        &>span:first-child
          &>a
            text-decoration none
            color Green
        span
         margin-right 5px
         font(.8em,Silver)
         &>i
           margin-right 5px
     .body
       margin 10px 0 20px 0
       font(.8em,Silver)
       .content
         overflow hidden
         max-height 120px
         padding-bottom 20px
         line-height 2em
         cursor pointer
         .ellipsis
           color Green
         *
           display inline-block
           margin 0
       .is-collapse
         max-height none
         word-break break-all
         *
           display block
    .side
      fj(center,center)
      padding-left 20px
      button
        wh(45px,45px)
  .misc
    fj(space-between)
    margin-bottom 10px
    .feedback
      &>a
        margin-left 10px
        font(.7em,Silver)
        text-decoration none
        transition all .2s
        &:hover,&:focus
          color Green
          cursor pointer
        &>i
          margin-right 5px
</style>
