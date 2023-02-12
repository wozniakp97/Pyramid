<template>
  <div class="container_list">
    <div class="container_select">
      <select
        class="select_input"
        name="selected"
        id="select"
        v-model="selected"
        @change="sort(value)"
      >
        <option value="price_min">Price descending</option>
        <option value="price_max">Price ascending</option>
        <option value="volume_min">Volume descending</option>
        <option value="volume_max">Volume ascending</option>
        <option value="height_min">Height descending</option>
        <option value="height_max">Height ascending</option>
      </select>
    </div>
    <div class="container_grid">
        <div
        class="container_piramid"
        v-for="pyramid in store.parsedResponse"
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
              <b v-if="pyramid.available == true">${{ pyramid.price }}</b>
              <b v-else-if="pyramid.available == false">Not open for tours</b>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
    name: "AllPyramids",
    props: {
        parsedResponse: {
            type: Array,
        },
    },
    setup(props) {
        const selected = ref("price_min");
        const store = props;
        function sort() {
            if (selected.value == "price_min") {
                 store.parsedResponse.sort((a, b) => b.price - a.price);
            }
            if (selected.value == "price_max") {
                store.parsedResponse.sort((a, b) => a.price - b.price);
            }
            if (selected.value == "volume_min") {
                 store.parsedResponse.sort((a, b) => Number(b.dimensions.split("x")[0]) *
                    Number(b.dimensions.split("x")[1]) *
                    Number(b.dimensions.split("x")[2]) *
                    0.3 -
                    Number(a.dimensions.split("x")[0]) *
                        Number(a.dimensions.split("x")[1]) *
                        Number(a.dimensions.split("x")[2]) *
                        0.3);
            }
            if (selected.value == "volume_max") {
                store.parsedResponse.sort((a, b) => Number(a.dimensions.split("x")[0]) *
                    Number(a.dimensions.split("x")[1]) *
                    Number(a.dimensions.split("x")[2]) *
                    0.3 -
                    Number(b.dimensions.split("x")[0]) *
                        Number(b.dimensions.split("x")[1]) *
                        Number(b.dimensions.split("x")[2]) *
                        0.3);
            }
            if (selected.value == "height_min") {
                store.parsedResponse.sort((a, b) => Number(b.dimensions.split("x")[2]) -
                    Number(a.dimensions.split("x")[2]));
            }
            if (selected.value == "height_max") {
                store.parsedResponse.sort((a, b) => Number(a.dimensions.split("x")[2]) -
                    Number(b.dimensions.split("x")[2]));
            }
        }
        return { store, sort, selected };
    },
    
};
</script>

<style lang="scss">
.picture {
  width: 19vw;
  height: 20vh;
  object-fit: cover;
  border-radius: 10px 10px 0 0;

  &:disabled {
    filter: opacity(0.5) !important;
  }
}

.container_select {
  display: flex;
  justify-content: flex-end;
}

.container_list {
  display: flex;
  flex-direction: column;
}

.select_input {
  border-radius: 5px;
  width: 13vw;
  height: 30px;
  margin-bottom: 50px;
  border: 2px solid #5f5f5f6b;
  cursor: pointer;
}

.height_container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.price_container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-left: 20px;
}

.blur {
  filter: opacity(0.5);
}

.container_piramid {
  display: flex;
  flex-direction: column;
  color: white;
  width: 19vw;
  border-radius: 10px;
  box-shadow: 3px 3px 10px #00000066;
}

.b_class {
  display: flex;
  margin: 25px 0 5px 20px;
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
</style>
