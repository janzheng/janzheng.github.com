<template>
  <!-- <div id="top" class="ContentFrame Layout" :class="navOpen ? '--open' : ''" > -->
  <div id="top" class="ContentFrame Layout" :class="isLoaded ? '--loaded' : ''" >
    <div class="color-palette color-top" id="top">
      <div id="c1"></div><div id="c2"></div><div id="c3"></div><div id="c4"></div><div id="c5"></div>
      <div id="a1"></div><div id="a2"></div><div id="a3"></div><div id="a4"></div><div id="a5"></div>
    </div>

    <nav>
      <div class="_nav-content">
        <!-- <span class="_nav-links">
          <a href="mailto:hello@janzheng.com">hello@janzheng.com</a> <span>/</span>
          <a href="./files/resume.pdf">résumé</a> <span>/</span>
          <a class="_nav-more " href="./about.html"> about</a>
        </span> -->
        <a class="_nav-logo" href="/"><svg width="66" height="25" viewBox="34 26 66 25" xmlns="http://www.w3.org/2000/svg"><path d="M83.35 50.33h-5.4V39.87c0-1.25-.32-2.2-.97-2.9-.66-.67-1.56-1-2.72-1-1.15 0-2.05.33-2.7 1-.66.7-1 1.65-1 2.9v10.46h-6.55v-2.8c-1.43 2.03-3.34 3.04-5.74 3.04-2.4 0-4.44-.92-6.14-2.76-1.03-1.1-1.75-2.4-2.16-3.88-.22 1.88-.95 3.4-2.18 4.55-1.5 1.4-3.44 2.1-5.8 2.1-2.34 0-4.26-.7-5.75-2.1C34.75 47.1 34 45.14 34 42.6h5.87c.02.92.23 1.6.62 2.03.38.44.9.66 1.5.66.63 0 1.15-.23 1.55-.67.4-.43.6-1.1.6-2.03V26.1h5.9v11.18c.43-1.38 1.12-2.6 2.1-3.65 1.7-1.85 3.77-2.78 6.22-2.78 2.45 0 4.34.9 5.65 2.73v-2.52h6.54v2.83c1.22-2.03 3.17-3.05 5.84-3.05 2.22 0 4 .74 5.38 2.2 1.37 1.5 2.05 3.5 2.05 6.02v5.35l9.37-13.84H82.87v-4.5H100v4.5L89.67 45.84H100v4.5H83.35zM64.02 40.7c0-1.52-.43-2.7-1.28-3.5-.85-.83-1.83-1.24-2.94-1.24-1.1 0-2.08.42-2.93 1.25-.85.84-1.28 2-1.28 3.53 0 1.52.42 2.7 1.27 3.5.85.82 1.83 1.23 2.93 1.23s2.1-.4 2.94-1.24c.85-.83 1.28-2 1.28-3.52z" fill="#971F1F" fill-rule="evenodd"></path></svg></a>
      </div>
      <div class="_nav-bottom">
        <div><a href="https://www.linkedin.com/in/janzh"><span class="_font-phage icon-linkedin"/></a></div>
        <div><a href="https://github.com/janzheng"><span class="_font-phage icon-github-circled"/></a></div>
        <div><a href="https://twitter.com/@yawnxyz"><span class="_font-phage icon-twitter"/></a></div>
        <div><a href="mailto:hello@janzheng.com"><span class="_font-phage icon-mail"/></a></div>
      </div>
    </nav>
    <!-- <Header :scroll-y="scrollY" /> -->
    

    <div class="ContentFrame-body _main" >
      <nuxt @click="onClick($event)" />
      <Footer/>
    </div>


  </div>
</template>


<script>

import _ from '~/other/lodash.custom.min.js'

import { mapState } from 'vuex'
import VueScrollTo from 'vue-scrollto'

// import Header from '~/components/layout/Header.vue'
import Footer from '~/components/layout/Footer.vue'
// import PolicyBanner from '~/components/org/PolicyBanner.vue'

// import Directory from '~/components/Directory.vue'
// import SignupAlert from '~/components/SignupAlert.vue'
// import SignupRequest from '~/components/SignupRequest.vue'
// import SignupCapsid from '~/components/SignupCapsid.vue'

export default {

  components: {
    // Header,
    Footer,
    // PolicyBanner,
    // SignupAlert,
    // SignupRequest,
    // SignupCapsid,
    // Directory,
  },

  // head () {
  //   return {
  //     title: this.$store.state.pageName || '',
  //   }
  // },

  async asyncdata () {
  },

  data () {
    return {
      scrollY: 0,
      route: '',
      width: 0,
      isLoaded: false,
      // searchString: 'testStr'
      // children: route ? route.children : undefined,
    }
  },


  computed: {
    ...mapState([
      'searchString',
      'navOpen',
      ]),
  },

  watch: {
  },

  beforeUpdate () {
    const route = this.$router.options.routes.find((route) => {
      return route.path === this.$route.path
    })

    // console.log('new route', route)
    this.route = route
  },

  mounted () {
    // handle anchor links on page load
    const _this = this
    let scrolled = false

    setTimeout(() => {
      _this.isLoaded = true
    }, 300)

    this.$nextTick(function () { // this waits to all the children are mounted, too

      if(_this.$route.hash && !scrolled) {
        // console.log('-- hash scroll')
      } 
      if(_this.$route.hash) {
        const scroll = _.throttle(function () {
          VueScrollTo.scrollTo(_this.$route.hash, 900, {
           offset: -50
         })
        }, 600)

        // scrolls too fast / before stuff's loaded, add a small buffer
        setTimeout(scroll, 300)
        // scroll()
      }
      scrolled = true
    })
  
  },

  // link intercept idea from: https://github.com/nuxt/nuxtjs.org/blob/master/components/HtmlParser.vue
  created () {
    // this.width = window.outerWidth

    if(process.browser)
      window.addEventListener('scroll', this.handleScroll)
  },

  destroyed () {
    if(process.browser)
      window.removeEventListener('scroll', this.handleScroll)
  },

  methods: {
    onClick(evt) {
      evt.preventDefault()
      console.log('on click evt', evt)
    },
    handleScroll() {
      if(process.browser) {
        const _this = this
        _.throttle(function () {
          _this.scrollY = window.scrollY
        }, 200)()
      }
    }
  },
}

</script>


<style lang="scss" scoped>
</style>
