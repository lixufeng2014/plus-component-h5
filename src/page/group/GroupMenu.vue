<template>
      <div class="m-pos-f m-box p-group-menu m-justify-end" :style="menuStyle"  @click.self="showMenu">
      <div class="m-box-model p-group-menu-wrap">
        
      </div>
    </div>
</template>
<script>
export default {
  name: "group-menu",
  props: {
    value: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      open: false
    };
  },
  computed: {
    // 侧边栏相关
    menuStyle() {
      return {
        transform: `translateX(${this.open ? 0 : (400 / 750) * 100 + "%"})`,
        transition: `all .5s ease`,
        zIndex: this.open ? 99 : -99
      };
    }
  },
  methods: {
    showMenu() {
      this.$emit("input", !this.value);
    }
  }
};
</script>
