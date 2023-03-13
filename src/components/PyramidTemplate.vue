<template>
          <div
        class="container_piramid"
        v-for="pyramid in parsedResponse"
        :key="pyramid.name"
      >
        <img
          alt="Egypt logo"
          class="picture"
          :class="{ blur: pyramid.available == false }"
          :src="pyramid.imageUrl"
        />
        <div :class="{ blur: pyramid.available == false }">
          <b class="b_class">{{ pyramid.name }}</b>
          <div class="info_container">
            <div class="height_container">
              <p>HEIGHT</p>
              <b>{{ pyramid.dimensions }}</b>
            </div>
            <div class="price_container">
              <p>TOUR PRICE</p>
              <b v-if="pyramid.available == true">${{ pyramid.price/100 }}</b>
              <b v-else-if="pyramid.available == false">Not open for tours</b>
            </div>
          </div>
        </div>
      </div>
</template>

<script>
export default {
    name: 'PyramidTemplate',
    props: {
        parsedResponse: {
            type: Array
        }
    },
}
</script>

<style lang="scss">

.container_grid {
  display: grid;
  place-items: center;
  grid-template-columns: repeat(4, 1fr);
  column-gap: 30px;
  row-gap: 30px;
  align-items: start;
}

@media (max-width: 1000px) {
  .container_grid {
    display: grid;
    place-items: center;
    grid-template-columns: repeat(3, 1fr);
    column-gap: 15px;
    row-gap: 15px;
    align-items: start;
  }
  .price_container {

  margin-left: 0 !important;
}

.info_container {
  display: flex;
  margin: 10px 0 25px 20px;
  flex-direction: column;
}
}

@media (max-width: 800px) {
  .container_grid {
    display: grid;
    place-items: center;
    grid-template-columns: repeat(2, 1fr);
    column-gap: 15px;
    row-gap: 15px;
    align-items: start;
  }
}

.info_container {
  display: flex;
  margin: 10px 0 25px 20px;
}
</style>