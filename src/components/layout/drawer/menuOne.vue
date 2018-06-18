<template>
  <div class="list no-border platform-delimiter light-paragraph">
    <template v-for="(parent, index) in links">
      <div class="list-label cursor-pointer" @click="parent.show = !parent.show">{{replaceUnderlineToSpace(index)}}</div>
      <template v-for="child in parent.routes">
        <transition name="menu">
          <div v-show="parent.show">
            <router-link :to="child.route" exact tag="div" class="item item-link drawer-closer cursor-pointer" >
              <i :class="child.faIcon" class="item-primary"></i>
              <div class="item-content">{{child.name}}</div>
            </router-link>
          </div>
        </transition>
      </template>
      <hr>
    </template>
    <div class="list-label cursor-pointer">Quasar Ready UI</div>
    <a href="http://quasar-framework.org/quasar-play/android/index.html#/showcase" target="_blank"
       class="item item-link drawer-closer cursor-pointer text-black" >
      <i class="fa fa-puzzle-piece item-primary" ></i>
      <div class="item-content">Components (55+)</div>
    </a>
  </div>
</template>

<script>
  export default {
    props: ['links'],
    watch: {
      '$route.path' () {
        Object.keys(this.links).forEach(parentName => { this.setParentVisibilityBasedOnRoute(this.links[parentName]) })
      }
    },
    computed: {
      currentRoutePath () {
        return this.$route.path
      }
    },
    methods: {
      setParentVisibilityBasedOnRoute (parent) {
        parent.routes.forEach(item => {
          if (this.$route.path === item.route) {
            parent.show = true
            return
          }
        })
      },
      replaceUnderlineToSpace (text) {
        while (text.indexOf('_') !== -1) {
          text = text.replace('_', ' ')
        }
        return text
      }
    }
  }
</script>
<style lang="stylus" scoped>
  @import '../../../themes/app.variables.styl'

  .list-label:first-child{
    line-height: 50px;
  }
  .router-link-active {
    color: darken($primary,80%);
    background-color: lighten($primary,80%) !important;
    border-right: 2px solid darken($primary,10%);
  }
  .router-link-active .item-primary{
    color: $primary;
  }
</style>
