<template>
  <div class="grid-content bg-purple-dark" style="height: 100%;background-color: #324057;overflow-y: auto;">
    <el-menu :default-active="active" :router=true :unique-opened=true theme="dark" style="height: 100%;">
      <template v-for="item in this.config">
        <template v-if="item.children">
          <el-submenu :index="item.name">
            <template slot="title">{{item.name}}</template>
            <template v-for="children in item.children">
              <el-menu-item :index="children.path">{{children.name}}</el-menu-item>
            </template>
          </el-submenu>
        </template>
      </template>
    </el-menu>
  </div>
</template>

<script>


  export default {
    data() {
      return {
        active: this.$route.path
      }
    },
    props: {
      config: {
        type: Array,
        default() {
          return []
        }
      }
    },
    watch: {
      $route() {
        this.active = this.$route.path
      }
    }
  }

</script>