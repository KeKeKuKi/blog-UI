<template>
  <article class="post post-list">
    <div class="post-entry">
      <div class="feature">
        <router-link :to="`/article/${post.id}`">
          <img :src="post.banner"/>
        </router-link>
      </div>
      <h1 class="entry-title">
        <router-link :to="`/article/${post.id}`"><span v-if="post.weight > 90" style="color:#ff6d6d;font-weight:600">[置顶] </span>{{ post.title }}
        </router-link>
      </h1>
      <div class="p-time">
        <i class="iconfont2 iconmeditor-time"></i> {{ new Date(post.createDate).toLocaleDateString() }}<i
          v-if="post.viewCounts > 10" class="iconfont iconfire" style="margin-left: 5px;color: #ff6d6d;"></i>
      </div>
      <div class="summary">
        <p>{{ post.summary }}</p>
<!--        <router-link :to="`/article/${post.id}`" style="color: #58a6f6;">全文阅读>></router-link>-->
      </div>

      <footer class="entry-footer">
        <div class="info-meta">
          <div class="comnum">
                        <span>
                            <i class="iconfont2 iconcomment"></i>
                            <a href="https://zhebk.cn/Web/Akina.html">{{ post.commentsCount }} 条评论</a>
                        </span>
          </div>
          <div class="views">
            <span><i class="iconfont2 iconeyes"></i>{{ post.viewCounts }} 热度</span>
          </div>
        </div>
      </footer>
    </div>
    <hr style="margin: 10px 0 0 30%;color: #2B2B2B;width: 70%"/>
  </article>
</template>

<script>

export default {
  name: "post",
  props: {
    post: {
      type: Object
    }
  }
}
</script>

<style scoped lang="less">
.post {
  margin: 0 0 4% 0;
  position: relative;
  padding: 10px;
  border-radius: 5px;
  //box-shadow: 0px 0px 5px #ffffff;
}

.post-entry {
  height: 200px;
  width: 100%;
  .feature {
    position: absolute;
    width: 99%;
    height: 99%;
    img {
      height: 100%;
      width:28%;
      object-fit: cover;

      border-radius: 3%;
      //padding: 1px;
      //border: 1px solid #DADADA;
      transition: all 0.2s linear;

      &:hover {
        transform: scale(1.1, 1.1);
        filter: contrast(130%);
      }
    }
  }

  .entry-title {
    font-size: 21px;
    font-weight: 600;
    line-height: 50px;
    margin: 0 0 0 30%;
    position: relative;
    z-index: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    width: 70%;
    color: #ffffff;
  }

  .p-time {
    position: absolute;
    right: 20px;
    top: 16px;
    font-size: 12px;
    color: #ffffff;
    letter-spacing: 1px;
    font-family: din, 'Hiragino Sans GB', 'Microsoft Yahei', Arial, sans-serif;
    display: flex;
    align-items: center;
  }

  footer.entry-footer {
    margin: 0 0 0 14%;
    list-style: none;

    .info-meta {
      margin-top: 10px;
      font-family: din, 'Hiragino Sans GB', 'Microsoft Yahei', Arial, sans-serif;
      position: absolute;
      top: 20px;
      opacity: 0;
      padding-top: 8px;
      border-top: 1px solid #ddd;
      -webkit-transform: translate3d(-150px, 0, 0);
      transform: translate3d(-150px, 0, 0);
      visibility: hidden;
      transition: .7s all ease;
      -webkit-transition: .6s all ease;
      -moz-transition: .6s all linear;
      -o-transition: .6s all ease;
      -ms-transition: .6s all ease;

      span, a {
        color: #ffffff;
        font-size: 12px;
      }

      i {
        margin-top: 3px;
        margin-right: 10px;
      }
    }
  }

}

.summary {
  min-height: 100px;
  margin: 0 0 0 31%;
  font-size: 16px;
  color: #ffffff;
  letter-spacing: 1px;
  line-height: 30px;
  overflow:hidden;
  clear:both;
}

.post-entry:hover footer.entry-footer .info-meta {
  -webkit-transform: translate3d(-300px, 0, 0);
  transform: translate3d(-300px, 0, 0);
  opacity: 1;
  visibility: visible;
}

.post hr {
  width: 33%;
  height: 1px;
  margin: 0 auto;
  border: 0;
  background: #ffffff;
}

/*******/
@media (max-width: 1060px) {
  .info-meta {
    display: none;
  }
}

@media (max-width: 800px) {
  .post-entry {
    .entry-title {
      font-size: 16px;
      line-height: 30px;
    }

    .p-time {
      position: relative;
      margin: -15px 0 0 65px;
    }
  }

  .post-list hr {
    margin-top: 20px;
  }
}

/******/
</style>
