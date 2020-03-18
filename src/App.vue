<template>
  <div id="app">
    {{count}}
    <button v-stream:click="plus$">+</button>
    <div>{{increment}}</div>
    <ChieldComponent v-stream:increment="{subject: plusFromChield$}"></ChieldComponent>
  </div>
</template>

<script>
import ChieldComponent from "./components/ChieldButton";
import { Subject } from "rxjs";
import { map, startWith, scan } from "rxjs/operators";
export default {
  name: "App",
  components: {
    ChieldComponent
  },
  methods: {
    // increment() {
    //   console.log("clicked");
    //   this.plus$.next();
    // }
  },
  subscriptions() {
    // declare the receiving Subjects
    this.plus$ = new Subject();
    this.plusFromChield$ = new Subject();
    // ...then create subscriptions using the Subjects as source stream.
    // the source stream emits in the format of `{ event: HTMLEvent, data?: any }`
    return {
      count: this.plus$.pipe(
        map(() => 2),
        startWith(10),
        scan((total, change) => total + change)
      ),
      increment: this.plusFromChield$.pipe(
        map(val => {
          console.log(val);
          return val.event.msg;
        })
      )
    };
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
