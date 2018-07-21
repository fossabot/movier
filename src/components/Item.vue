<template>
    <div :style="style" class="item"></div>
</template>

<script>
export default {
  name: 'Item',
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      poster: this.item.Poster,
      title: this.item.Title,
    };
  },
  computed: {
    style() {
      return `background-image: url("${this.poster}")`;
    },
  },
};
</script>

<style lang="scss" scoped>
.item {
  width: 250px;
  height: 400px;
  margin: 10px;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: 50%;
  transition: box-shadow .3s;
  &:hover{
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  }
}
</style>
