<template>
    <div class="about">
        <div class="site-content">
            <div class="content-warp">
                <div class="about-site about-info">
                    <section-title><span>❤</span>关于ZZZBlog</section-title>
                    <div class="info-card">
                      <p><span style="color: #0aaeb3" >ZZZ Blog</span> 基于开源博客UI <a href="https://gitee.com/fengziy/Gblog" style="color: #f35a5a">G-blog</a> 开发而成</p>
                        <br>
                        <p>现已将项目开源到我的个人 Github <a target="_blank" href="https://github.com/KeKeKuKi" style="color: #ff6d6d;">github.com/KeKeKuKi</a> </p>
                    </div>
                </div>
                <div class="about-me about-info">
                    <section-title id="Guestbook"><span>❤</span>给我留言</section-title>
                    <div class="info-card">
                        <div class="contactForm">
                            <div class="form-item">
                                <label for="mail">您的邮箱</label>
                                <input class="v" type="email" name="mail" id="mail">
                            </div>
                            <div class="form-item">
                                <label for="content">批评和建议</label>
                                <textarea class="v" id="content" name="content"></textarea>
                            </div>
                            <div class="form-item">
                                <label></label>
                                <button>提交</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    import sectionTitle from '@/components/section-title'
    import {getTime,getTimeInterval} from '@/utils'
    // import Quote from "@/components/quote";
    // import {fetchFriend} from '../api'
    export default {
        name: "About",
        data() {
            return {
                list: []
            }
        },
        components: {
            // Quote,
            sectionTitle
        },
        methods: {},
        mounted() {
        }
    }
</script>
<style scoped lang="less">
    .about {
        padding-top: 40px;
    }

    .content-warp {
        margin-top: 80px;

        .about-info {
            margin: 30px 0;

            span {
                color: red;
                margin-right: 10px;
            }

            .info-card {
                min-height: 100px;
                padding: 20px;
                border-radius: 3px;
                margin: 30px 0 50px 0;
                box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
                p{
                    line-height: 1.7rem;
                }
            }
        }
        .contactForm{
            width: 100%;
            padding: 20px;
            .form-item{
                align-items: center;
                display: flex;
                &:not(:last-child){
                    margin-bottom: 20px;
                }
                label{
                  width: 110px;
                }
                .v{
                    min-height: 40px;
                    line-height: 20px;
                    border-radius: 3px;
                    padding: 2px 10px;
                    outline:none;
                    border: 1px solid #8fd0cc;
                    width: 100%;
                    resize: vertical;
                }
                button{
                    width: 100px;
                    height: 40px;
                    border-radius: 3px;
                    outline:0;
                    border-style: none;
                    cursor: pointer;
                    background-color: #409eff;
                    color: white;
                    &:hover{
                        opacity: 0.8;
                    }
                }
            }
        }
    }

    /*******/
    @media (max-width: 800px) {
        .content-warp {
            margin-top: 0;
        }
    }
</style>
