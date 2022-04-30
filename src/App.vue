<template>
  <div>
    <MyHeader :index="this.index" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <MyContent
            v-if="questionlist.length"
            :cQuestion="questionlist[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import MyContent from "./components/MyContent.vue";
import MyHeader from "./components/MyHeader.vue";

export default {
  name: "App",
  components: {
    MyContent,
    MyHeader,
  },
  data() {
    return {
      questionlist: [],
      index: 0,
      correctcount: 0,
      totalcount: 0,
    };
  },
  methods: {
    next() {
      if (this.index < 9) this.index++;
    },

    increment(is_correct) {
      if (is_correct) {
        this.correctcount++;
      }
      this.totalcount++;
    },
  },

  mounted: function () {
   // debugger;
    fetch("https://opentdb.com/api.php?amount=11&category=11", {
      method: "get",
    })
      .then((response) => {
        return response.json();
        
      })
      .then((result) => {
        console.log("jjjjjjjjj",result);
        this.questionlist = result.results;
      });
  },
};
</script>   
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #0b0125;
  margin-top: 60px;
}
</style>
