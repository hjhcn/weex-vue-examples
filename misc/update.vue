<template>
  <scroller class="wrapper">
    <div class="content">
      <text @click="storeDoodle">Store Doodle</text>
    </div>
    <div class="content">
      <text @click="storeExamples">Store Examples</text>
    </div>
    <div class="content">
      <text @click="storeGuide">Store Guide</text>
    </div>
    <div class="content">
      <text @click="storeAbout">Store About</text>
    </div>
    <div class="content">
      <text @click="storeNews">Store News</text>
    </div>
    <div class="content">
      <input type="text" v-model="message" class="message">
      <text @click="sendMessage">Send</text>
    </div>
    <div class="content">
      <text>{{output}}</text>
    </div>
  </scroller>
</template>

<style scoped>
  .content {
    margin: 20px;
  }
</style>


<script>
  import xhr from 'xhr'
  import getExamples from '../examples'
  import guide from './sliders'
  const examples = getExamples({ scope: 'mobile', filterTODO: true })

  const about = [
    {
      title: { en: 'Weex Official Website', zh: 'Weex 官方网站' },
      link: {
        en: 'http://weex-project.io/',
        zh: 'http://weex-project.io/cn/'
      }
    }, {
      title: { en: 'Apache Software Foundation', zh: 'Apache 软件基金会' },
      link: 'http://www.apache.org/'
    }, {
      title: { en: 'Who is using Weex', zh: '谁在使用 Weex' },
      link: {
        en: 'http://weex-project.io/who-is-using-weex.html',
        zh: 'http://weex-project.io/cn/who-is-using-weex.html'
      }
    }, {
      title: { en: 'Contribution', zh: '参与贡献' },
      link: {
        en: 'http://weex-project.io/guide/contributing.html',
        zh: 'http://weex-project.io/cn/guide/contributing.html'
      }
    }, {
      title: { en: 'Release Note', zh: '版本变更' },
      link: {
        en: 'http://weex-project.io/releasenote.html',
        zh: 'http://weex-project.io/cn/releasenote.html'
      },
    }, {
      title: { en: 'FAQ', zh: '常见问题' },
      link: {
        en: 'http://weex-project.io/faq.html',
        zh: 'http://weex-project.io/cn/faq.html'
      }
    }
  ]

  const news = [
    {
      type: 'article',
      poster: 'https://mmbiz.qpic.cn/mmbiz_jpg/33P2FdAnju8OqtiaqCvRCIyh9NRiam3jrRYFYG6icFPibgJjBFBvibu0eoibSGGkKjjsZsTcl70vUqmibOn2XtAruWTicg/640',
      title: '2018 Weex Meetup 上海站 等你来聚',
      link: 'https://mp.weixin.qq.com/s/aGsGvOSavNb-MkmA-VpV5Q',
      time: { zh: '2018年9月4日 21:23', en: '2018/09/04 21:23' },
      related: [{
        poster: 'http://pcdn.mikecrm.com/ugc_3_c/pub/35/354762527021b054af71e22ab9deb0be/form/image/pLOlnrmHIJr5UQQWhf75AaTtC3iGcOKz.png',
        title: '报名链接',
        link: 'http://cn.mikecrm.com/QPSHnpm'
      }]
    }, {
      type: 'article',
      poster: 'https://gw.alicdn.com/tfs/TB1ReL9kVGWBuNjy0FbXXb4sXXa-1080-679.jpg',
      title: '一次美丽的“约会”',
      summary: 'Weex Meetup 深圳站的一点感想',
      link: 'https://mp.weixin.qq.com/s/yI30oAMHXCqICEo2Mc85jQ',
      time: { zh: '2018年4月2日 19:09', en: '2018/04/02 19:09' },
    }, {
      type: 'article',
      poster: 'https://gw.alicdn.com/tfs/TB1S9m9brGYBuNjy0FoXXciBFXa-1595-992.png',
      title: '来，认识一下 Weex 的 JS Framework',
      link: 'https://mp.weixin.qq.com/s/K6wXSGPywc7Ltm0T3lz-Sg',
      time: { zh: '2018年2月27日 21:45', en: '2018/02/27 21:45' },
    }, {
      type: 'article',
      poster: 'https://gw.alicdn.com/tfs/TB1Q9VBkRfH8KJjy1XbXXbLdXXa-3799-1615.png',
      title: 'Weex SDK v0.18.0 is released!',
      link: 'http://weex-project.io/releasenote.html#v0-18-0',
      time: { zh: '2018年2月9日 19:53', en: '2018/02/09 19:53' },
    }, {
      type: 'article',
      poster: 'https://mmbiz.qpic.cn/mmbiz_png/33P2FdAnjuibsOIzHLgiaHW0ib4j1BVmtZeAB3NPctFqGLz0icnnrzgEcJ5OG03aIINCh3KLxSUWuyW2KPTuRHTmiag/640',
      title: 'Weex 实战分享 | 企鹅电竞Weex实践和性能优化',
      link: 'https://mp.weixin.qq.com/s/-HJ42j4AZDZ0ZSo4zHuBAA',
      time: { zh: '2018年1月31日 16:30', en: '2018/01/31 16:30' },
    }, {
      type: 'article',
      poster: 'https://mmbiz.qpic.cn/mmbiz_jpg/33P2FdAnjuibsOIzHLgiaHW0ib4j1BVmtZeSJT9FzChUmw9R6SkHKRE2HAvYqV1FAAxCkFQHJF274IFkvtfDzxoUw/640',
      title: 'Weex 实战分享 | Weex在极客时间APP中的实践',
      link: 'https://mp.weixin.qq.com/s/atEbHaiu60MjDirAOAiNqQ',
      time: { zh: '2018年1月30日 12:00', en: '2018/01/30 12:00' },
    }, {
      type: 'article',
      poster: 'https://mmbiz.qpic.cn/mmbiz_jpg/33P2FdAnju9hLib2a6qwrwibHlLumz2IvCVgQADOLPZuWfbWYEI9T9Gic6GxrOcumdgNFG0TehEETonHymSbWHJww/640',
      title: 'Weex 实战分享 | Weex在盛大游戏中的应用实践',
      link: 'https://mp.weixin.qq.com/s/XSCaxW_Dg67pa4vI1LZBFg',
      time: { zh: '2018年1月29日 10:30', en: '2018/01/29 10:30' },
    }, {
      type: 'article',
      poster: 'https://mmbiz.qpic.cn/mmbiz_jpg/33P2FdAnjuibdbD0UvQhic1XXu4YibPcqyniaZcYaWp3sSoJhlqFbfEHXnItQIibVmGj5l3vYftOic1nV8lNxibeCfUmw/640',
      title: 'WeexConf 2018干货系列 | Weex技术演进(下)',
      link: 'https://mp.weixin.qq.com/s/SoctOi9XG39QQTmfPARWgA',
      time: { zh: '2018年1月26日 12:30', en: '2018/01/26 12:30' },
    }, {
      type: 'article',
      poster: 'https://mmbiz.qpic.cn/mmbiz_jpg/33P2FdAnjuibdbD0UvQhic1XXu4YibPcqynFpG1V1x4O8mzn5pXxTg2QzVjZyWMnIyusTKwvoBvhWtkGgAjdThDUg/640',
      title: 'WeexConf 2018干货系列 | Weex技术演进(上)',
      link: 'https://mp.weixin.qq.com/s/shC1Ydakvl-n2KoEWbMsTw',
      time: { zh: '2018年1月25日 9:50', en: '2018/01/25 9:50' },
    }, {
      type: 'article',
      poster: 'https://mmbiz.qpic.cn/mmbiz_jpg/33P2FdAnju9YFyWBwuoqan1SNEPFTsjHrsSNMRI9lMnl82SqGDohAn6vK2uQ363I3rB1yicAZYNXsFzfTKQM0fw/640',
      title: 'WeexConf 2018干货系列 | Weex + UI',
      link: 'https://mp.weixin.qq.com/s/Sa3FcIKvWSDq_zOlQZ9d6Q',
      time: { zh: '2018年1月24日 10:20', en: '2018/01/24 10:20' },
    }, {
      type: 'article',
      poster: 'https://gw.alicdn.com/tfs/TB1u0M5odfJ8KJjy0FeXXXKEXXa-1208-805.jpg',
      title: '参加 WeexConf 2018 是一种什么体验?',
      link: 'https://mp.weixin.qq.com/s/utkNWHs4opy7P7wzjSbJ7ws',
      time: { zh: '2018年1月22日 9:30', en: '2018/01/22 9:30' },
    }, {
      type: 'article',
      poster: 'https://gw.alicdn.com/tfs/TB1iMVtlZLJ8KJjy0FnXXcFDpXa-1293-496.png',
      title: 'Weex Conf 2018 等你来',
      summary: '2018年1月19日，分享 Weex 的蜕变之旅',
      link: 'http://weex-project.io/weexconf2018/',
      time: { zh: '2018年1月5日 17:30', en: '2018/01/05 17:30' },
    }, {
      type: 'article',
      poster: 'https://gw.alicdn.com/tfs/TB1Q9VBkRfH8KJjy1XbXXbLdXXa-3799-1615.png',
      title: 'Getting Started With Weex',
      link: 'http://weex-project.io/guide/',
      time: { zh: '2018年1月5日 17:30', en: '2018/01/01 0:0' },
      related: [{
        poster: 'https://gw.alicdn.com/tfs/TB1qOlHdgMPMeJjy1XbXXcwxVXa-328-328.png',
        title: '快速上手 Weex',
        link: 'http://weex-project.io/cn/guide/index.html'
      }]
    }, {
      type: 'article',
      poster: 'https://img.alicdn.com/tps/TB1xS6JOVXXXXcnXXXXXXXXXXXX-2000-500.jpg',
      title: 'Weex Conf 2017',
      link: 'https://yq.aliyun.com/activity/145',
      time: { zh: '2017年1月12日 9:30', en: '2017/01/12 9:30' },
      related: [{
        title: '【资料合集】Weex Conf 回顾集锦',
        link: 'https://yq.aliyun.com/articles/68818'
      }]
    }, {
      type: 'article',
      poster: 'https://yqfile.alicdn.com/eed85e0086e30d1b49d006ef8f104f609dc73f1b.png',
      title: 'Apache再次接受阿里开源产品捐赠：移动开发框架Weex进入孵化',
      link: 'https://yq.aliyun.com/articles/66576',
      time: { zh: '2016年12月15日 12:01', en: '2016/12/15 12:01' }
    }, {
      type: 'article',
      poster: 'https://gw.alicdn.com/tfs/TB1xA4Tl8fH8KJjy1XbXXbLdXXa-1080-726.jpg',
      title: 'Weex meets Vue',
      summary: '记一次 Weex 前端团队的讨论',
      link: 'https://github.com/weexteam/article/issues/77',
      time: { zh: '2016年9月9日 17:25', en: '2016/09/09 17:25' }
    }, {
      type: 'article',
      poster: 'https://gw.alicdn.com/tfs/TB1AqLil2DH8KJjy1XcXXcpdXXa-1440-809.png',
      title: 'Weex 在 GitHub 中开源！',
      link: 'https://weibo.com/5897827106/DCFtruTbc',
      time: { zh: '2016年6月30日 20:20', en: '2016/06/30 20:20' }
    }
  ]

  function encodeJSON (json) {
    const arr = []
    json.token = json.token || '4fe14953b4af234a0876c5c1247f6250'
    Object.keys(json).forEach((key) => {
      if (typeof json[key] === 'object') {
        arr.push(encodeURIComponent(key) + '=' + encodeURIComponent(JSON.stringify(json[key])));
      } else {
        arr.push(encodeURIComponent(key) + '=' + encodeURIComponent(json[key]));
      }
    })
    return arr.join('&')
  }

  function store (params, done) {
    xhr({
      uri: `http://weex-playground-beta.ca4d7f2a827354d85a3cc06b171274fa2.cn-qingdao.alicontainer.com/store/weex-playground-app`,
      method: 'post',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
      },
      body: encodeJSON(params)
    }, (error, response, body) => {
      if (body) {
        done(body)
      }
    })
  }

  export default {
    el: '#root',
    data () {
      return {
        output: '',
        message: ''
      }
    },
    methods: {
      storeDoodle () {
        const doodle = {
          from: (new Date()).getTime(),
          to: (new Date(2018, 0, 10, 3, 24, 0)).getTime(),
          // src: '47944db1be068da5714986b282ab2305',
          // next: '1d4b9b89b3e2eacd9b61e9cffb59efed',
          // duration: 10000
        }
        store({ name: 'doodle', doodle }, res => {
          console.log(res)
        })
      },
      storeExamples () {
        console.log(examples)
        store({ name: 'examples', examples }, res => {
          console.log(res)
        })
      },
      storeGuide () {
        console.log(guide)
        store({ name: 'guide', guide }, res => {
          console.log(res)
        })
      },
      storeAbout () {
        console.log(about)
        store({ name: 'about', about }, res => {
          console.log(res)
        })
      },
      storeNews () {
        store({ name: 'news', news }, res => {
          console.log(res)
        })
      },
      sendMessage () {
        const params = {
          token: 'whatever',
          message: this.message || '环境变量'
        }
        console.log('will send message', params)
        xhr({
          uri: `http://weex-playground-beta.ca4d7f2a827354d85a3cc06b171274fa2.cn-qingdao.alicontainer.com/chat`,
          method: 'post',
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded',
            // 'Access-Control-Allow-Origin': '*'
          },
          body: encodeJSON(params)
        }, (error, response, body) => {
          if (body) {
            console.log('result', body)
          }
        })
      }
    }
  }
</script>
