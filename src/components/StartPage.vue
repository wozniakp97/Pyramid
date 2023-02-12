<template>
  <div >
    <div class="button_container">

      <div>
        <button
      class="button_change_title"
      v-for="(element, index) in tableTitle"
      :key="index"
      :id="element.name"
      :class="element.order"
      @click="e => funkcja(e)"
      >
      {{ element.name }}
        </button>
       
      </div>
    </div>
    <div>
          <AllPyramids :parsedResponse="parsedResponse" v-if="tableTitle.find(el => el.order == 'start').name == 'All pyramids'"></AllPyramids>
          <StatisticsPyramids :parsedResponse="parsedResponse" v-if="tableTitle.find(el => el.order == 'start').name == 'Pyramids trivia & data'"></StatisticsPyramids>
        </div>
  </div>
</template>

<script>
import { ref, reactive, toRefs} from "vue";
import AllPyramids from "./AllPyramids.vue";
import StatisticsPyramids from "./StatisticsPyramids.vue";
export default {
    name: "StartPage",
    comments: {
      AllPyramids,
      StatisticsPyramids
    },
    props: {
        msg: String
    },
    async setup() {
        const control = ref("AllPyramids");
        const selected = ref("min");
        const menu = reactive({
            tableTitle: [
                {
                    name: "All pyramids",
                    order: "start",
                },
                {
                    name: "Pyramids trivia & data",
                    order: "next",
                }
            ]
        });
        function funkcja(x) {
            menu.tableTitle.forEach(el => {
                el.order = "next";
            });
            menu.tableTitle.find(el => el.name == x.target.id).order = "start";
        }
        const response = await fetch("https://pyramids.mouflon.xyz/");
        let parsedResponse = await response.json();

        function ff(){
          
          if(selected.value == 'min'){
            parsedResponse = parsedResponse.sort((a, b) => b.name.localeCompare(a.name))
          }
          else{
            parsedResponse = parsedResponse.sort((a, b) => a.name.localeCompare(b.name))
          }
        }

        return { ...toRefs(menu), control, funkcja, parsedResponse, ff, selected};
    },
    components: { AllPyramids, StatisticsPyramids }
}
</script>
<style lang="scss">

.button_container{
  display: flex;
  justify-content: space-between;
  margin-top: 60px;
}
.button_change_title {
  cursor: pointer;
  background-color: #ffffff;
  border: 1px solid #ffffff;
  font-family: "SF Pro Display";
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  color: #2c3e50;
}
.start{
    border-bottom: 3px solid #2c3e50 !important;
}

.next{
    color: #9896A1 !important;
    font-weight: 400 !important;
}
p {
  color: rgb(163, 162, 162);
  font-weight: 600;
  font-size: 11px;
  margin: 0;
}

b{
  color: black;
  font-size: 17px;
  font-weight: 700;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
