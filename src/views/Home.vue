<template>
  <div class="home">
    <banner isHome="true"></banner>
    <div class="site-content animate">
      <!--通知栏-->
      <div class="notify">
        <div class="search-result" v-if="hideSlogan">
          <span v-if="searchWords">搜索结果："{{ searchWords }}" 相关文章</span>
          <span v-else-if="category">分类 "{{ category }}" 相关文章</span>
        </div>
       <quote v-else>{{ notice }}</quote>
      </div>

      <!--焦点图-->
      <div class="top-feature" v-if="!hideSlogan">
        <section-title>
          <div style="display: flex;align-items: flex-end;color: white">聚焦
            <small-ico></small-ico>
          </div>
        </section-title>
        <div class="feature-content">
          <div class="feature-item" v-for="item in features" :key="item.title">
            <Feature :data="item"></Feature>
          </div>
        </div>
      </div>
      <!--文章列表-->
      <main class="site-main" :class="{'search':hideSlogan}">
        <section-title v-if="!hideSlogan" style="color: white">文章推荐</section-title>
        <template v-for="item in postList">
          <post :post="item" :key="item.id"></post>
        </template>
      </main>

      <!--加载更多-->
      <div class="more" v-if="hasNextPage">
        <div class="more-btn" @click="loadMore">More</div>
      </div>
      <div class="more1" v-else>
        <div class="more-btn">-------------------------我是有底线的-------------------------</div>
      </div>
    </div>
  </div>
</template>

<script>
import Banner from '@/components/banner'
import Feature from '@/components/feature'
import sectionTitle from '@/components/section-title'
import Post from '@/components/post'
import SmallIco from '@/components/small-ico'
import Quote from '@/components/quote'
import {fetchFocus, fetchList} from '../api'

export default {
  name: 'Home',
  props: ['cate', 'words'],
  data() {
    return {
      features: [
        {
          id: 1,
          title: 'Akina',
          img: 'https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/gtimg.jpg'
        },
        {
          id: 2,
          title: '使用说明',
          img: 'https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/gtimg.jpg'
        },
        {
          id: 3,
          title: '文章归档',
          img: 'https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/gtimg.jpg'
        },
        {
          id: 4,
          title: '文章归档',
          img: 'https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/gtimg.jpg'
        }
      ],
      postList: [],
      currPage: 1,
      hasNextPage: true,
      page: {
        pageNum: 1,
        pageSize: 4,
        pages: 1
      }
    }
  },
  components: {
    Banner,
    Feature,
    sectionTitle,
    Post,
    SmallIco,
    Quote
  },
  computed: {
    searchWords() {
      return this.$route.params.words
    },
    category() {
      return this.$route.params.cate
    },
    hideSlogan() {
      return this.category || this.searchWords
    },
    notice() {
      return this.$store.getters.notice
    }
  },
  methods: {
    // fetchFocus() {
    //   fetchFocus().then(res => {
    //     this.features = res.data || []
    //   }).catch(err => {
    //     console.log(err)
    //   })
    // },
    fetchList() {
      this.page.pageNum = 1
      this.page.pageSize = 4
      let par = {
        page: this.page.pageNum,
        size: this.page.pageSize,
        queryParam: {
          words: this.$route.params.words,
          category: this.$route.params.cate
        }
      }
      fetchList(par).then(res => {
        if (res) {
          this.postList = res.data.records
          this.hasNextPage = res.data.pages > this.page.pageNum
        }
      }).catch(err => {
        console.log(err)
      })
    },
    loadMore() {
      this.page.pageNum = this.page.pageNum + 1
      let par = {
        page: this.page.pageNum,
        size: this.page.pageSize,
        queryParam: {
          words: this.$route.params.words,
          category: this.$route.params.cate
        }
      }
      fetchList(par).then(res => {
        this.postList = this.postList.concat(res.data.records)
        this.hasNextPage = res.data.pages > this.page.pageNum
      })
    }
  },
  mounted() {
    // this.fetchFocus();
    this.fetchList();
  },
  watch: {
    $route() {
      this.words = this.$route.params.words
      this.cate = this.$route.params.cate
    },
    words() {
      this.fetchList()
    },
    cate() {
      this.fetchList()
    }
  }
}
</script>
<style scoped lang="less">

.site-content {
  min-width: 50%;
  padding: 30px;
  background-color: rgba(200, 200, 200, 0.1);

  .notify {
    margin: 60px 0;
    border-radius: 3px;

    & > div {
      padding: 20px;
    }
  }


  .search-result {
    padding: 15px 20px;
    text-align: center;
    font-size: 20px;
    font-weight: 400;
    border: 1px dashed #ddd;
    color: #e1e1e1;
  }
}

.top-feature {
  width: 100%;
  height: auto;
  margin-top: 30px;

  .feature-content {
    margin-top: 10px;

    display: flex;
    justify-content: space-between;
    position: relative;
    color: white;

    .feature-item {
      width: 25%;
      margin-right: 10px;
    }
  }
}

.site-main {
  padding-top: 80px;

  &.search {
    padding-top: 0;
  }
}

.more {
  margin: 50px 0;

  .more-btn {
    width: 800px;
    height: 40px;
    line-height: 40px;
    text-align: center;
    color: #d7d7d7;
    border: 1px solid #e5e5e5;
    border-radius: 20px;
    margin: 0 auto;
    cursor: pointer;

    &:hover {
      color: #8fd0cc;
      border: 1px dashed #8fd0cc;
    }
  }
}

.more1 {
  margin: 50px 0;

  .more-btn {
    width: 800px;
    height: 40px;
    line-height: 40px;
    text-align: center;
    color: #e8e8e8;
    border-radius: 20px;
    margin: 0 auto;
    cursor: pointer;
  }
}

/******/
@media (max-width: 800px) {
  .top-feature {
    display: none;
  }

  .site-main {
    padding-top: 40px;
  }

  .site-content {
    .notify {
      margin: 30px 0 0 0;
    }

    .search-result {
      margin-bottom: 20px;
      font-size: 16px;
    }
  }
}

/******/
</style>
