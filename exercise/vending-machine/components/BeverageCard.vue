<template>
  <div
    class="cardContianer"
    @click="$emit('click')"
    :class="available && item.in_stock ? 'available' : 'unavailable'"
    :style="{ 'background-image': 'url(' + item.image + ')' }"
  >
  <div
    class="soldout"
    v-if="!item.in_stock"
  >
    <span>Sold Out</span>
  </div>
    <div class="detail">
      <h5>{{ item.name }}</h5>
      <h4>{{ item.price }} THB</h4>
    </div>
  </div>
</template>

<script>
export default {
  props: ['item', 'available']
}
</script>

<style lang="scss" scoped>

.cardContianer {
  background-size: cover;
  background-repeat: no-repeat;
  position: relative;
  width: 275px;
  height: 120px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  border-radius: 4px;
  cursor: pointer;
  .soldout {
    position: absolute;
    z-index: 100;
    border-radius: 4px;
    background-color: rgba(255, 0, 0, 0.7);
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    > span {
      font-size: 2em;
      font-weight: bold;
      color: #fff;
      opacity: 1;
    }
  }
  > .detail {
    transition: visibility 0.5s ease, opacity 0.5s ease;
    background-color: #b2daff;
    opacity: 0;
    color: #333;
    text-align: center;
    visibility: hidden;
  }
  &:hover {
    > .detail {
      visibility: visible;
      opacity: 0.8;
    }
  }

  &.unavailable {
    transition: box-shadow 0.5s ease;
    box-shadow: inset 0px 0px 20px #f00, 0px 0px 0px #aaa;
    &:hover {
      box-shadow: inset 0px 0px 20px #f00, 5px 5px 20px #aaa;
    }
  }
  &.available {
    transition: box-shadow 0.5s ease;
    box-shadow: inset 0px 0px 20px #0f0, 0px 0px 0px #aaa;
    &:hover {
      box-shadow: inset 0px 0px 20px #0f0, 5px 5px 20px #aaa;
    }
  }
}
</style>

