<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="dark">
      <b-navbar-brand href="#">Recommender</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item
            v-for="(item, i) in navList"
            :key="i"
            :href="item.href"
            :class="{ active: currentPath == item.href }"
          >
            {{ item.name }}
          </b-nav-item>
        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">

          <b-nav-item-dropdown right>
            <!-- Using 'button-content' slot -->
            <template v-slot:button-content>
              <em>User</em>
            </template>
            <b-dropdown-item href="#">Profile</b-dropdown-item>
            <b-dropdown-item href="#">Sign Out</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script charset="utf-8" src=""></script>
<script>
export default {
  data () {
    return {
      navList: [
        { href: '/index', name: '首页' },
        // { href: '/law', name: '法规' },
        // // { href: '/medical', name: '医疗' },
        // { href: '/movie', name: '电影' },
        // { href: '/event', name: '活动' },
        { href: '/poi', name: '兴趣点' },
        { href: '/graphic', name: '结果图表' }
      ]
    }
  },
  computed: {
    currentPath () {
      var x = this.$route.path.indexOf('/', 1)
      if (x !== -1) {
        return this.$route.path.substring(0, x)
      } else {
        return this.$route.path
      }
    }
  }
}
</script>

<style scoped>
/* Change background color to navbar in bootstrap-vuejs
  https://stackoverflow.com/questions/56827797/change-background-color-to-navbar-in-bootstrap-vuejs */
.navbar.navbar-dark.bg-dark {
  background-color: #563d7c !important;
}
</style>
