<template>
  <div class="chioce" v-show=isShow>
    <div v-for="item in items" :key="item.id" class="cellChioce">
      <button v-on:click="selectNumber(item.number)" :class="{highlight:item.except == item.id}" >
        {{ item.number }}
      </button>
    </div>
  </div>
</template>

<script>
import Func from "../components/Func";
export default {
  data() {
    return {
      items: Array.apply(null, { length: 9 }).map(function(_, index) {
        return {
          id: index + 1,
          number: index + 1,
          except: ""
        };
      })
    };
  },
  props: ["isShow", "disablenumber"],
  methods: {
    selectNumber(item) {
      this.$emit("clicked", item);
    }
  },
  components: {
    Func
  },
  watch: {
    disablenumber: function() {
      this.items.map((item, i) => {
        item.except = this.disablenumber.find(x => {
          if (x == item.id) {
            return true;
          } else {
            return false;
          }
        });
      });
      //console.log(JSON.stringify(this.items, null, 2));
    }
  }
};
</script>

<style scoped>
.chioce {
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.cellChioce button {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 40px;
  height: 40px;
  border: 1px solid #aaa;
}
.highlight {
  border: 2px red solid !important;
}
</style>
