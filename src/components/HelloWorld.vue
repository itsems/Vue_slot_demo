<template>
  <div class="hello">
    <h1>[Vue] Slot Demo</h1>

    <!-- component-basic -->
    <div class="basic">
      <h2>基本使用 (Basic Usage)</h2>
      <h3>一般Component</h3>
      <component-basic></component-basic>
      <hr />
    </div>

    <!-- component-slot -->
    <div class="component-slot">
      <h3>帶有slot的component</h3>
      <h4>直接帶入component</h4>
      <component-slot></component-slot>
      <h4>指定slot內容</h4>
      <component-slot>我是指定的內容</component-slot>
      <hr />
    </div>

    <!-- component-slot-compile -->
    <div class="component-slot-compile">
      <h2>編譯作用域 (Compilation Scope)</h2>
      <component-slot-compile>HelloWorld 的 data : {{mes}}</component-slot-compile>
      <hr />
    </div>

    <!-- component-slot-name -->
    <div class="component-slot-name">
      <h2>具名插槽</h2>
      <p>這裡要帶入一個有四個slot位置的component，由 slot template 決定位置，在哪邊帶入不受影響</p>
      <component-slot-name>
        <template v-slot:footer>我要指定 name 是 footer 的 slot 內容</template>
        <template v-slot:header>我要指定 name 是 header 的 slot 內容</template>
        <template>我要指定沒有 name(default) 的 slot 內容</template>
        <template v-slot:content>我要指定 name 是 content 的 slot 內容</template>
      </component-slot-name>
      <hr />
    </div>

    <!-- component-scoped -->
    <div class="component-scoped">
      <h2>作用域插槽 ( Scoped Slots )</h2>
      <!-- :default 也可以省略 -->
      <component-scoped
        v-slot="slotProps"
      >我是父層，想要看到 component 裡面的 firstName：{{ slotProps.user.firstName }}</component-scoped>
      <hr />
    </div>

    <!-- 一般的props -->
    <div class="component-props">
      <h2>一般的props</h2>
      <component-props :childReceive="parentValue"></component-props>
      <hr />
    </div>

    <!-- 解構插槽Prop (Destructuring Slot Props) -->
    <div class="Destructuring">
      <h2>解構插槽Prop (Destructuring Slot Props)</h2>
      <component-scoped v-slot="{ user }">解構：{{ user.firstName }}</component-scoped>
      <component-scoped v-slot="{ user:info }">解構：{{ info.firstName }}</component-scoped>
      <hr />
    </div>

    <!-- 具名插槽的縮寫(Named Slots Shorthand) -->
    <div class="component-slot-name">
      <h2>具名插槽的縮寫</h2>
      <component-slot-name>
        <template #footer>name 是 footer 的 slot 內容</template>
        <template #header>name 是 header 的 slot 內容</template>
        <template>沒有 name(default) 的 slot 內容</template>
        <template #content>name 是 content 的 slot 內容</template>
      </component-slot-name>
      <component-scoped #default="{ user }">解構：{{ user.firstName }}</component-scoped>
      <hr />
    </div>

    <!-- 列表範例 -->
    <div class="component-list">
      <component-list :lists="lists">
        <template #item="props">
          <li>{{ props.text }}</li>
        </template>
      </component-list>
    </div>
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
  template: `<div>
      <slot></slot>
      <p>我是component-slot-compile自己的data : {{mes}}</p>
    </div>`,
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
  template: `<div>
              <p><slot :user='slotuser'></slot></p>
            </div>`,
  data() {
    return {
      slotuser: {
        firstName: "emma"
      }
    };
  }
});

Vue.component("component-props", {
  template: `<div>
              <p>{{childReceive}}</p>
            </div>`,
  props: ["childReceive"]
});

Vue.component("component-list", {
  props: ["lists"],
  template: `
        <ul>
            <slot name="item" v-for="item in lists" :text="item.name"></slot>
        </ul>
    `
});

export default {
  name: "HelloWorld",
  data() {
    return {
      mes: "hehe",
      age: 18,
      parentValue: "dataInParent",
      lists: [
        { id: 1, name: "emma" },
        { id: 2, name: "hehe" },
        { id: 3, name: "haha" }
      ]
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
