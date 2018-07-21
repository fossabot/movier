<template>
    <div class="searchWrapper">
      <input
        id="search"
        name="search"
        :class="{ dark }"
        :value="value"
        @keyup.enter="handleChange"
      />
    </div>
</template>

<script>
export default {
  name: 'SearchInput',
  props: {
    value: {
      type: String,
      required: true,
    },
    dark: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    handleChange(e) {
      this.$emit('input', e.target.value);
    },
  },
};
</script>

<style lang="scss" scoped>
.searchWrapper {
  display: flex;
  flex-direction: column;
  width: 300px;
  margin-top: 70px;

  input {
    color: white;
    text-align: center;
    font-size: 18px;
    font-weight: 300;
    height: 30px;
    border: 0;
    border-bottom: 1px solid white;
    background: none;
    transition: box-shadow 0.3s ease-out;
    @media (min-width: 1024px) {
      font-size: 20px;
      font-weight: 400;
    }
    &:focus{
      outline: none;
      box-shadow: 0 10px 20px -8px rgba(255, 255, 255, .5);
    }
  }
  .dark{
    color: #000;
    border-bottom-color: #000;
    &:focus{
      box-shadow: 0 10px 20px -8px rgba(#000,.2);
    }
  }
}


</style>
