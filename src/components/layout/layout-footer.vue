<template>
  <div id="layout-footer" class="footer">
    <div class="footer-main">
      <div class="footer-item" v-if="socials.length">
        <div v-for="item in socials" :key="item.id"><a target="_blank" class="out-link" :href="item.href"><i
            class="iconfont" :class="item.icon"></i>{{ item.title }}</a></div>
      </div>
      <div class="footer-item">
        <div style="font-size:17px;font-weight: bold;">友情链接</div>
        <div><a target="_blank" class="out-link" href="https://www.baidu.com">百度</a>
        </div>
        <div><a target="_blank" class="out-link" href="https://www.google.com/">Goole</a></div>
      </div>
      <div class="footer-item">
        <div>本站已正常运行 {{ runTimeInterval }}</div>
        <div><a target="_blank" class="out-link" href="http://162.14.77.8:8080/">☞后台管理快捷入口</a></div>
      </div>
    </div>
    <div class="copyright">Copyright © 2020 by <a target="_blank" class="out-link" href="https://www.fengziy.cn">fengziy.cn</a>
      . All rights reserved. | <a target="_blank" class="out-link"
                                  href="http://www.beian.miit.gov.cn">渝ICP备17015355号-1</a></div>
  </div>
</template>

<script>
import sectionTitle from '@/components/section-title'

export default {
  name: "layout-footer",
  data() {
    return {
      socials: []
    }
  },
  components: {
    sectionTitle
  },
  computed: {
    runTimeInterval() {
      return this.$store.state.runTimeInterval;
    }
  },
  methods: {
    getSocial() {
      this.$store.dispatch('getSocials').then(data => {
        this.socials = data
      })
    },
  },
  created() {
    // this.getSocial();
    this.$store.dispatch('initComputeTime');
  }
}
</script>

<style scoped lang="less">
#layout-footer {
  margin-top: 50px;
  background-color: rgba(0,0,0,0.8);
  padding: 2%;
  border-top: 1px solid #F7F7F7;
  font-size: 12px;
  color: #ffffff;

  a.out-link:hover {
    color: #ff6d6d;
  }

  .footer-main {
    max-width: 800px;
    margin: 0 auto 20px auto;
    display: flex;
    justify-content: space-around;
    align-items: flex-start;

    .footer-item {
      flex: 1;

      & > div:not(:last-child) {
        margin-bottom: 10px;
      }

      i {
        margin-right: 10px;
      }
    }
  }

  .copyright {
    text-align: center;
  }
}

/*****/
@media (max-width: 800px) {
  #layout-footer {
    .footer-main .footer-item:nth-child(3) {
      flex: 2;
    }
  }
}

@media (max-width: 600px) {
  #layout-footer {
    .footer-main {
      display: block;

      .footer-item {
        display: flex;
        justify-content: space-around;
        align-items: center;
        flex-wrap: wrap;

        & > div {
          margin-bottom: 10px;
        }
      }
    }
  }
}

/*****/
</style>
