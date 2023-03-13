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
        <option         
        v-for="option in optionSelect"
        :key="option.id" :value="option.key">{{option.name}}</option>
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
              <b v-if="pyramid.available == true">${{ pyramid.price/100 }}</b>
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
    data(){
      return{
          optionSelect:[
            {id: 0, name: 'Price descending', key: 'price_min'},
            {id: 1, name: 'Price ascending', key: 'price_max'},
            {id: 2, name: 'Volume descending', key: 'volume_min'},
            {id: 3, name: 'Volume ascending', key: 'volume_max'},
            {id: 4, name: 'Height descending', key: 'height_min'},
            {id: 5, name: 'Height ascending', key: 'height_max'},
          ]
      }
        },
    props: {
        parsedResponse: {
            type: Array,
        },
    },

    setup(props) {
        const selected = ref("price_min");
        const store = props;

        function volumeDescendingSort(volume) {
          const [x, y, z] = volume.dimensions.split("x")
          return Number(x) * Number(y) * Number(z) * 0.3
      }

        function sort() {
            if (selected.value == "price_min") {
                 store.parsedResponse.sort((a, b) => b.price - a.price);
                 
            }
            if (selected.value == "price_max") {
                store.parsedResponse.sort((a, b) => a.price - b.price);
            }
            if (selected.value == "volume_min") {
                 store.parsedResponse.sort((a, b) => volumeDescendingSort(b) - volumeDescendingSort(a));
            }
            if (selected.value == "volume_max") {
              store.parsedResponse.sort((a, b) => volumeDescendingSort(a) - volumeDescendingSort(b));
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


// @media (max-width: 1000px) {
//   .container_grid {
//     display: grid;
//     place-items: center;
//     grid-template-columns: repeat(3, 1fr);
//     column-gap: 15px;
//     row-gap: 15px;
//     align-items: start;
//   }
//   .price_container {

//   margin-left: 0 !important;
// }

// .info_container {
//   display: flex;
//   margin: 10px 0 25px 20px;
//   flex-direction: column;
// }
// }

// @media (max-width: 800px) {
//   .container_grid {
//     display: grid;
//     place-items: center;
//     grid-template-columns: repeat(2, 1fr);
//     column-gap: 15px;
//     row-gap: 15px;
//     align-items: start;
//   }
// }
</style>
