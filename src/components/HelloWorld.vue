<template>
  <div class="hello">
    <h1>[Vue] Slot Demo</h1>

    <h2>基本使用 (Basic Usage)</h2>
    <h3>一般Component</h3>
    <component-basic></component-basic>
    <h3>帶有slot的component</h3>
    <h4>直接帶入component</h4>
    <component-slot></component-slot>
    <h4>指定slot內容</h4>
    <component-slot>我是指定的內容</component-slot>
    <hr />
    <h2>編譯作用域 (Compilation Scope)</h2>
    <component-slot-compile mes="mes">HelloWorld 的 data : {{mes}}</component-slot-compile>
    <hr />
    <h2>具名插槽</h2>
    <p>這裡要帶入一個有四個slot位置的component，由template決定位置，在哪邊帶入不受影響</p>
    <component-slot-name>
      <template v-slot:footer>我要指定name是footer的slot內容</template>
      <template v-slot:header>我要指定name是header的slot內容</template>
      <template>我要指定沒有name(default)的slot內容</template>
      <template v-slot:content>我要指定name是content的slot內容</template>
    </component-slot-name>
    <hr />
    <h2>作用域插槽 ( Scoped Slots )</h2>
    <component-scoped>
      <template v-slot:default="slotProps">{{ slotProps.user.lastName }}</template>
    </component-scoped>
  </div>
</template>

<script>
import Vue from "vue/dist/vue.js";

Vue.component("component-basic", {
  template: "<div>我是一般的component</div>"
});

Vue.component("component-slot", {
  template:
    "<div><slot>我會在沒有內容的時候出現，指定的內容會長在這裡</slot></div>"
});

// <p>我想抓parent component(HelloWorld的data) : {{age}} 抓不到</p>
Vue.component("component-slot-compile", {
  template:
    "<div><slot>我會在沒有內容的時候出現，指定的內容會長在這裡</slot><p>我是component-slot-compile自己的data : {{mes}}</p></div>",
  data() {
    return {
      mes: "haha"
    };
  }
});

Vue.component("component-slot-name", {
  template: `<div>
      <slot name='header'></slot><br />
      <slot name='content'></slot><br />
      <slot></slot><br />
      <slot name='footer'></slot>
    </div>`
});

Vue.component("component-scoped", {
  template:
    "<div><slot :user='user'>component-scoped裡面的firstName : {{user.firstName}}</slot></div>",
  data() {
    return {
      user: {
        firstName: "emma",
        lastName: "lin"
      }
    };
  }
});

export default {
  name: "HelloWorld",
  data() {
    return {
      mes: "hehe",
      age: 18
    };
  },
  created() {
    console.clear();
  }
};
</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
