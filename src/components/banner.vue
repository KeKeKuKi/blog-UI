<template>
  <div id="banner" :class="{'home-banner':isHome}">
    <video class="bgvid" id="bgvid" muted loop>
      <source src="https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/7.mp4" type="video/mp4">
    </video>
    <div class="banner-img">

      <template v-if="isHome">
        <!--博主信息-->
        <div class="focusinfo">
<!--          &lt;!&ndash; 头像 &ndash;&gt;-->
<!--          <div class="header-tou">-->
<!--            <router-link to="/"><img-->
<!--                :src="websiteInfo.avatar || 'https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/1636017556749a514c312bf2e0803.jpg' ">-->
<!--            </router-link>-->
<!--          </div>-->
          <!-- 简介 -->
          <div class="header-info">
            <p>
              {{ websiteInfo.slogan || ' <--  不 断 学 习 , 不 断 进 步 的 Java 程 序 猿 . 阿忠 --> ' }}</p>
          </div>
          <!-- 社交信息 -->
          <div class="top-social">
            <div v-for="item in socials" :key="item.id" :title="item.title">
              <a :href="item.href" target="_blank" :style="{'color':item.color}">
                <i class="iconfont" :class="item.icon"></i>
              </a>
            </div>
          </div>
        </div>
        <!--左右倾斜-->
<!--        <div class="slant-left"></div>-->
<!--        <div class="slant-right"></div>-->
      </template>
    </div>
  </div>
</template>

<script>
export default {
  name: "banner",
  data() {
    return {
      websiteInfo: {},
      socials: [{
        id: 1,
        title: 'QQ',
        icon: 'iconqq',
        color: '#1AB6FF ',
        href: 'http://wpa.qq.com/msgrd?v=3&uin=2669918628&site=qq&menu=yes'
      },
        {
          id: 3,
          title: 'GitHub',
          icon: 'icongithub',
          color: '',
          href: 'https://github.com/KeKeKuKi'
        }]
    }
  },
  props: {
    src: {
      type: String,
      default: 'https://file-bucket-01-1307916663.cos.ap-chengdu.myqcloud.com/%E4%B8%AD%E5%9B%BD%E9%A3%8E%E5%BE%AA%E7%8E%AF%E8%83%8C%E6%99%AF.mp4'
    },
    isHome: {
      type: [Boolean, String],
      default: false
    }
  },
  created() {
    // this.getWebSiteInfo()
    // this.getSocial()
  },
  methods: {
    getSocial() {
      this.$store.dispatch('getSocials').then(data => {
        this.socials = data
      })
    },
    getWebSiteInfo() {
      this.$store.dispatch('getSiteInfo').then(data => {
        this.websiteInfo = data
      })
    }
  },mounted() {
    var myVideo = document.getElementById("bgvid");
    myVideo.play()
  }
}
</script>

<style scoped lang="less">
.bgvid {
  position: fixed;
  right:0;
  bottom:0;
  //min-width:100%;
  //min-height:100%;
  margin-top: 1000px;
  width: 100%;
  height: auto;
  z-index:-100;
  background-size: cover;
}

#banner {
  position: relative;
  //margin-top: 70px;
  width: 100%;
  height: 100px;

  .banner-img {
    background-color: rgba(0,0,0,0.5);
    width: inherit;
    height: inherit;
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;
    transition: all 0.2s linear;
    overflow: hidden;

    &:hover {
      transform: scale(1.1, 1.1);
      filter: contrast(130%);
    }
  }

  &.home-banner {
    height: 180px;

    .banner-img {
      background-position: center center;
      background-repeat: no-repeat;
      background-attachment: fixed;
      background-size: cover;
      z-index: -1;
      transition: unset;

      &:hover {
        transform: unset;
        filter: unset;
      }
    }

    .slant-left {
      content: '';
      position: absolute;
      width: 0;
      height: 0;
      //border-bottom: 100px solid #FFF;
      border-right: 1250px solid transparent;
      left: 0;
      bottom: 0;
    }

    .slant-right {
      content: '';
      position: absolute;
      width: 0;
      height: 0;
      //border-bottom: 100px solid #FFF;
      border-left: 1250px solid transparent;
      right: 0;
      bottom: 0;
    }
  }
}

.focusinfo {
  position: relative;
  padding: 0 10px;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);
  -webkit-transform: translate(-50%, -50%);
  text-align: center;

  img {
    width: 80px;
    height: auto;
    border-radius: 50%;
    border: 3px solid rgba(255, 255, 255, 0.3);
  }

  .header-info {
    width: 50%;
    font-size: 16px;
    color: #1bff5e;
    background: rgba(255, 255, 255, 0.2);
    padding: 20px 30px;
    margin: 30px auto 0 auto;
    font-family: miranafont, "Hiragino Sans GB", STXihei, "Microsoft YaHei", SimSun, sans-serif;
    letter-spacing: 1px;
    line-height: 10px;
  }

  .top-social {
    height: 32px;
    margin-top: 30px;
    margin-left: 46%;
    list-style: none;
    display: inline-block;
    font-family: miranafont, "Hiragino Sans GB", STXihei, "Microsoft YaHei", SimSun, sans-serif;
    div {
      float: left;
      margin-right: 10px;
      height: 32px;
      line-height: 32px;
      text-align: center;
      width: 32px;
      background: white;
      border-radius: 100%;
    }
  }
}

@media (max-width: 960px) {
  #banner {
    height: 400px;
  }
}

@media (max-width: 800px) {
  #banner {
    display: none;
  }
}
</style>
