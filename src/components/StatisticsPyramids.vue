<template>
  <div class="statistics_container">
    <div class="column_container">
        <div class="text_image">
            <b :style="'margin-bottom: 20px'">Pyramid with the largest field</b>
            <PyramidTemplate  :parsedResponse="[storeLargestField[0]]"></PyramidTemplate>
        </div>
        <div class="text_image" :style="'margin-left: 50px'">

            <b :style="'margin-bottom: 20px'">The pyramid with the largest volume</b>
            <PyramidTemplate  :parsedResponse="[greatestVolume[0]]"></PyramidTemplate>
        </div>
    </div>

    <b class="info_color" :style="'margin-top: 50px; margin-bottom:50px'">The average volume of the pyramids: <b>{{ (volume / heightPyramid).toFixed(2) }} </b></b>
    <b class="info_color">The average cost of visiting: <b> ${{ (cost / availableTrue).toFixed(2) }} </b></b>

  </div>
</template>

<script>
import { ref } from "vue";
import PyramidTemplate from "./PyramidTemplate.vue";
export default {
    name: "StatisticsPyramids",
    props: {
        parsedResponse: {
            type: Array,
        },
    },
    components: { PyramidTemplate },
    setup(props) {
        const store = props;
        const volume = ref(0);
        const cost = ref(0);

        function volumeDescendingSort(volume) {
          const [x, y, z] = volume.dimensions.split("x")
          return Number(x) * Number(y) * Number(z) * 0.3
      }

        function largestFieldSort(volume) {
          const [x, y] = volume.dimensions.split("x")
          return Number(x) * Number(y)
      }

        const storeLargestField = store.parsedResponse.sort((a, b) => largestFieldSort(b) - largestFieldSort(a));
        const greatestVolume = store.parsedResponse.sort((a, b) => volumeDescendingSort(b) - volumeDescendingSort(a));
        const availableTrue = store.parsedResponse.filter((el) => el.available == true).length;
        const heightPyramid = store.parsedResponse.filter((el) => Number(el.dimensions.split("x")[2]) != 0).length;
        
        store.parsedResponse.forEach((el) => {
            if (el.available == true) {
                cost.value += el.price;
            }
            if (Number(el.dimensions.split("x")[2]) != 0) {
                volume.value += volumeDescendingSort(el)
            }
        });
        return {
            store,
            cost,
            volume,
            availableTrue,
            heightPyramid,
            storeLargestField,
            greatestVolume,
        };
    },

};
</script>

<style lang="scss">

.text_image{
    display: flex;
    flex-direction: column;
}
.info_color{
    color:#9896a1 !important
}
.statistics_container {
  display: flex;
  margin-top: 60px;
  flex-direction: column;
  align-items: flex-start;
}
.column_container{
    display: flex;
    margin-top: 50px
}
</style>
