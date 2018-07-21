<template>
    <div class="claimWrapper">
        <h1 class="claim">MOVIER</h1>
        <p class="subclaim">Search for your favourite movies</p>
    </div>
</template>

<script>
export default {
  name: 'Claim',
};
</script>

<style lang="scss" scoped>
.claimWrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #fff;
}
.claim {
  font-size: 50px;
  margin: 0;

  @media(min-width: 768px) {
      font-size: 80px;
  }
  @media(min-width: 1024px) {
      font-size: 100px;
  }
}
.subclaim {
  margin: 20px;
  font-weight: 400;
  font-size: 13px;

  @media(min-width: 768px) {
    font-size: 17px;
    line-height: 1;
    letter-spacing: 1px;
  }
}
</style>
