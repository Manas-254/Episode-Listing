<template>
  <h1 style="text-align : center;color:#5ea3ab;">Episodes Name Listing in Alphabetical order</h1>
  <br />
  <div v-for="(seasonData, group, index) in groupbySeason" :key="index">
    <h3  v-randomColor=" ['blue','brown','green']">Episode names starts  with letter {{ group }}</h3>
    <hr />
    <EpisodeDesc :episodeLists="seasonData">
        <template v-slot:btn1>
          <button>Button 1</button>
        </template>
                <template v-slot:btn2>
          <button>Button 2</button>
        </template>
        </EpisodeDesc>
  </div>
</template>

<script>
import { defineAsyncComponent } from "vue";
const EpisodeDesc = defineAsyncComponent({
  loader: () =>
    import('./EpisodeDesc.vue'/*webpackChunkName: "Episode Name"*/),
  delay: 500
});
import data from "@/data.json";



export default {
  components: {
    EpisodeDesc
  },
  name: "home",
  data() {
    return {
      episodeLists: [],
    };
  },
  computed: {
    groupbySeason() {
      let list = {};
      let alpha = [];
      for (let i = 65; i <= 90; i++) {
        alpha.push(String.fromCharCode(i))
      }

      this.episodeLists.forEach((ele) => {
        var x = ele.name.charAt(4);

        for (let j = 0; j < 26; j++) {
          if (alpha[j] === x) {
            if (Object.keys(list).includes(x)) {
              list[x] = [...list[x], ele];
            } else {
              list[x] = [ele];
            }
          }
        }

      });

      console.log({ list });
      list = Object.entries(list).sort((a, b) => {
        if (b > a) {
          return -1;
        } else if (b < a) {
          return 1;
        } else return 0;
      });
      list = Object.fromEntries(list);
      console.log();
      return list;
    },
  },
  created() {
    this.episodeLists = data && data._embedded && data._embedded.episodes;
    console.log(this.episodeLists);
  },

  methods: {},
};
</script>

<style>
hr {
  border: 4px solid #5e5fab6c;
  border-radius: 5px;
}
</style>
