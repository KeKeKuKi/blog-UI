<template>
  <div class="articles">
    <banner></banner>
    <div class="site-content animate">
      <main class="site-main">
        <article class="hentry">
          <!-- 文章头部 -->
          <header class="entry-header">
            <!-- 标题输出 -->
            <h1 class="entry-title">{{ this.article.title }}</h1>
            <hr>
            <div class="breadcrumbs">
              <div id="crumbs">最后更新时间：{{ new Date(this.article.createDate).toLocaleDateString() }}</div>
            </div>
          </header>
          <!-- 正文输出 -->
          <mavon-editor v-model="article.articleBodyDTO.markdown" :toolbars="toolbars" @keydown="" :editable="false" :externalLink="false"
                        :subfield="false" defaultOpen="preview" codeStyle="agate" :transition="true" :external-link="externalLink" style="margin: 10px 0 20px 0"/>
          <!-- 文章底部 -->
          <section-title>
            <footer class="post-footer">
              <!-- 阅读次数 -->
              <div class="post-like">
                <i class="iconfont2 iconeyes"></i>
                <span class="count" style="color: white">{{ article.viewCounts }}</span>
              </div>
              <div class="post-share">
                <ul class="sharehidden">
                  <li>
                    <a href="http://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?url=http://localhost:8888/article/2/&amp;title=title"
                       onclick="window.open(this.href, 'weibo-share', 'width=800,height=600');return false;"
                       class="s-qq" target="_blank" rel="nofollow noopener noreferrer">
                      <img src="https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/qqz.jpg"
                           style="width: 30px;height: 30px;border-radius: 50%;margin-right: 10px">
                    </a>
                    <a href="http://service.weibo.com/share/share.php?url=http://localhost:8888/article/2/&amp;title=title"
                           onclick="window.open(this.href, 'weibo-share', 'width=800,height=600');return false;"
                           class="s-sina" target="_blank" rel="nofollow noopener noreferrer">
                      <img src="https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/weibo1.jpg"
                         style="width: 30px;height: 30px;border-radius: 50%;margin-right: 10px">
                    </a>
                    <a href="http://shuo.douban.com/!service/share?http://localhost:8888/article/2/&amp;title=title"
                           onclick="window.open(this.href, 'renren-share', 'width=800,height=600');return false;"
                           class="s-douban" target="_blank" rel="nofollow noopener noreferrer">
                    <img src="https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/douban.png"
                         style="width: 30px;height: 30px;border-radius: 50%;margin-right: 10px">
                    </a>
                  </li>
                </ul>
                <i class="iconfont show-share"></i>
              </div>
              <div class="donate" @click="showDonate=!showDonate">
                <span>赏</span>
                <ul class="donate_inner" :class="{'show':!showDonate}">
                  <li class="wedonate"><img
                      src="https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/qq_pic_merged_1638789899182.jpg">
                    <p>微信</p></li>
                  <li class="alidonate"><img
                      src="https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/qq_pic_merged_1638789925115.jpg">
                    <p>支付宝</p></li>
                </ul>
              </div>
              <!-- 文章标签 -->
              <div class="post-tags">
                <i class="iconfont iconcategory"></i>
                <router-link :to="'/category/'+article.categoryId">{{article.category}}</router-link>
              </div>
            </footer>
          </section-title>

          <!--声明-->
          <div class="open-message">
            <p>声明：阿忠博客|版权所有，违者必究|如未注明，均为原创|本网站采用<a href="https://creativecommons.org/licenses/by-nc-sa/3.0/" target="_blank">BY-NC-SA</a>协议进行授权</p>
          </div>
          <!--评论-->
          <div class="comments">
            <comment v-for="item in comments" :key="item.comment.id" :comment="item.comment">
              <template v-if="item.reply.length">
                <comment v-for="reply in item.reply" :key="reply.id" :comment="reply"></comment>
              </template>
            </comment>
          </div>
        </article>
      </main>
    </div>
  </div>
</template>

<script>
import Banner from '@/components/banner'
import sectionTitle from '@/components/section-title'
import comment from '@/components/comment'
import menuTree from '@/components/menu-tree'
import {fetchComment, getArticleBody, updateArticle} from '../api'
import {marked} from 'marked'

export default {
  name: 'articles',
  data() {
    return {
      externalLink: {
        markdown_css: false,
        hljs_js: () => '/md/highlightjs/highlight.min.js',
        hljs_css: (css) => '/md/highlightjs/styles/' + css + '.min.css',
        hljs_lang: (lang) => '/md/highlightjs/languages/' + lang + '.min.js',
        katex_css: () => '/md/katex/katex.min.css',
        katex_js: () => '/md/katex/katex.min.js',
      },
      compiledMarkdown: '',
      toolbars: {
        // bold: true, // 粗体
        // italic: true, // 斜体
        // header: true, // 标题
        // underline: true, // 下划线
        // mark: true, // 标记
        // superscript: true, // 上角标
        // quote: true, // 引用
        // ol: true, // 有序列表
        // link: true, // 链接
        // imagelink: true, // 图片链接
        help: true, // 帮助
        // code: true, // code
        // subfield: true, // 是否需要分栏
        // fullscreen: true, // 全屏编辑
        readmodel: true, // 沉浸式阅读
        // /* 1.3.5 */
        // undo: true, // 上一步
        // trash: true, // 清空
        // save: true, // 保存（触发events中的save事件）
        // /* 1.4.2 */
        navigation: true, // 导航目录
      },
      article: {
        articleBodyDTO: {
          markdown: ''
        }
      },
      showDonate: false,
      comments: [],
      menus: []
    }
  },
  components: {
    Banner,
    sectionTitle,
    comment,
    menuTree
  },
  methods: {
    update() {
      updateArticle(this.article).then(res => {
        if (res.data) {
          this.getArticleBody()
        }
      }).catch(err => {
        console.log(err)
      })
    },
    getComment() {
      fetchComment().then(res => {
        this.comments = res.data || []
      }).catch(err => {
        console.log(err)
      })
    },
    fetchH(arr, left, right) {
      if (right) {
        return arr.filter(item => item.offsetTop > left && item.offsetTop < right)
      } else {
        return arr.filter(item => item.offsetTop > left)
      }
    },
    // 生成目录
    createMenus() {
      let arr = []
      for (let i = 6; i > 0; i--) {
        let temp = []
        let e = document.querySelector(".entry-content").querySelectorAll(`h${i}`)
        for (let j = 0; j < e.length; j++) {
          let child = this.fetchH(arr, e[j].offsetTop, (j + 1 === e.length) ? undefined : e[j + 1].offsetTop)
          temp.push({
            h: i,
            title: e[j].innerText,
            id: e[j].id,
            offsetTop: e[j].offsetTop,
            child
          })
        }
        if (temp.length) {
          arr = temp
        }
      }
      this.menus = arr
    },
    getArticleBody() {
      let id = this.$route.params.id
      getArticleBody(id).then(res => {
        if (res) {
          this.article = res.data
          this.compiledMarkdown = marked(res.data.articleBodyDTO.markdown, {sanitize: true})
        }
      }).catch(err => {
        console.log(err)
      })

    }
  },
  mounted() {
    this.getArticleBody()
  },
  created() {
    // 获取评论
    // this.getComment()
  }
}
</script>

<style scoped lang="less">
@import '/md/markdown/github-markdown.min.css';

.site-content {
  position: relative;

  .site-main {
    padding: 10px 0 0 0;
  }
}

#article-menus {
  position: sticky;
  top: 0;
  box-shadow: 0 2px 6px rgba(0, 0, 0, .1);
  border-radius: 3px;
  padding: 15px;
  width: 300px;
  transform: translateX(-120%) translateY(150px);
  font-size: 14px;
}

article.hentry {
  .entry-header {
    .entry-title {
      font-size: 23px;
      font-weight: 600;
      color: #ffffff;
      margin: 0.67em 0;

      &:before {
        content: "#";
        margin-right: 6px;
        color: #d82e16;
        font-size: 20px;
        font-weight: 600;
      }
    }

    hr {
      height: 1px;
      border: 0;
      background: #EFEFEF;
      margin: 15px 0;
    }

    .breadcrumbs {
      font-size: 14px;
      color: #D2D2D2;
      text-decoration: none;
      margin-bottom: 30px;
    }
  }

  .entry-content {
  }

  footer.post-footer {
    width: 100%;
    padding: 20px 10px;
    margin-top: 30px;
    height: 65px;
    position: relative;

    i {
      font-size: 18px;
      margin-right: 5px;
    }

    .post-like {
      float: right;
      margin: 7px 0 0 20px;
    }

    .post-share {
      float: right;
      list-style: none;
      margin-right: 20px;
    }

    .donate {
      float: left;
      line-height: 36px;
      border-radius: 100%;
      -webkit-border-radius: 100%;
      -moz-border-radius: 100%;
      border: 1px solid #fd9621;
      &:hover {
        border: 1px solid #ffcc5a;

        span {
          color: #ffcd53;
        }
      }

      span {
        color: #fd9621;
        padding: 10px;
        position: relative;
        cursor: pointer;
      }

      .donate_inner {
        display: none;
        margin: 0;
        list-style: none;
        position: absolute;
        left: 130px;
        top: -38px;
        //background: #FFF;
        padding: 10px;
        //border: 1px solid #ddd;
        box-shadow: 0 2px 6px rgba(0, 0, 0, .08);
        border-radius: 3px;

        &.show {
          display: block;
        }

        li {
          float: left;
          margin-right: 20px;
        }

        img {

          width: 150px;
        }

        p {
          text-align: center;
          font-size: 15px;
          color: #D2D2D2;
          line-height: 1rem;
        }
      }

      //.donate_inner:after, .donate_inner:before {
      //  content: "";
      //  position: absolute;
      //  left: 0;
      //  bottom: 55%;
      //  border-top: 8px solid transparent;
      //  border-bottom: 8px solid transparent;
      //  border-right: 8px solid #fff;
      //}

      .donate_inner:before {
        left: -1px;
        //border-right: 8px solid #ddd;
      }

    }

    .post-tags {
      margin: 7px 0 0 20px;
      float: left;
      text-transform: uppercase;
      color: white;
      a:hover {
        color: #cecece;
      }
    }
  }

  .open-message {
    margin: 50px 0;
    position: relative;
    background: #2B2B2B;
    padding: 10px 30px;
    border-radius: 3px;
    font-size: 14px;
    color: #fff;

    &:after {
      content: "";
      border-left: 10px solid transparent;
      border-right: 10px solid transparent;
      border-bottom: 10px solid #2B2B2B;
      position: absolute;
      top: -8px;
      left: 48%;
    }

    p {
      margin: 10px 0;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }

    a {
      color: #A0DAD0;
      padding: 0 5px;
    }
  }
}
</style>
