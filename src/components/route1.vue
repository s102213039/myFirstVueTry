<template>
  <div class="hello">
    <runoob></runoob>
    <h3>Route1</h3>
    <h3>Essential Links</h3>
    <br />
    {{a}}
    {{b}}
    <input v-model="a"/>
    <br />
    <ul class="navbar">
      <li> <a href="#" @click="changeTab('home')">Home</a></li>
      <li> <a href="#" @click="changeTab('about')">About</a></li>
      <li> <a href="#" @click="changeTab('contract')">Contract</a></li>
    </ul>
    <hr>
    <component :is="currentView"></component>
    <keep-alive>
      <component :is="currentView"></component>
    </keep-alive>
    <br />
    <input type="text" v-model="message" />
    <br />
    {{message | capitalize}}
    <br />
    <button @click="addCount()">+1</button>
    <button @click="minusCount()">-1</button>
    <br />
    {{count}}
    <br />
    <p v-show="isShow">{{ message }}</p>
    <p v-if="isShow">{{ reverseMessage }}</p>
    <br />
    <p>g(公克)：<input type="text" v-model="g"></p>
    <p>kg(公斤)：<input type="text" v-model="kg"></p>
    <p>t(公噸)：<input type="text" v-model="t"></p>
    <br />
    <p>{{content}}</p>
    <p>如果字數超過10個字就結取前7位+3個... : {{summary}}</p>
    <local-component></local-component>
    <br />
    <my-text :my-message="myMessage"></my-text>
    <input type="text" v-model="myMessage">
  </div>
</template>

<script>
var localComponent = {
  template: '<div><p>{{ message }}</p><button @click="notice">點我</button></div>',
  data: function () {
    return {
      message: '這是局部註冊的元件'
    }
  },
  methods: {
    notice: function () {
      alert('Local Component!')
    }
  }
}

// var myText = {
//   props: ['myMessage'],
//   template: '<p>{{ myMessage }}</p>'
// }

var home = {
  template: `<div>This is Home page.
              <p><input type="text" placeholder="Type your name here..."></p>
              </div>`
}
var about = {
  template: '<div>This is About page.</div>'
}
var contract = {
  template: '<div>This is Contract page.</div>'
}

export default {
  name: 'App',
  data () {
    return {
      message: 'Hello Test!',
      count: 1,
      isShow: true,
      content: '`computed`的功能很強大，它可以對資料做處理',
      myMessage: '這是局部註冊的元件',
      g: 0,
      kg: 0,
      t: 0,
      a: 1,
      b: 1,
      currentView: 'home'
    }
  },
  methods: {
    textofa () {
      this.a += 1
    },
    addCount () {
      this.count += 1
    },
    minusCount () {
      this.count -= 1
    },
    testFunction () {
      var self = this
      setInterval(function () {
        // 設定為!，做到閃爍效果
        self.isShow = self.isShow
      }, 1000)
    },
    changeTab (view) {
      this.currentView = view
    }
  },
  mounted () {
    // this.testFunction()
    // 上面或下面都可以用，效果相同
    const self = this
    setInterval(function () {
      // 設定為!，做到閃爍效果
      self.isShow = self.isShow
    }, 1000)
  },
  filters: {
    // filters是JavaScript函數，所以可以傳入參數
    capitalize (value) {
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  },
  computed: {
    reverseMessage () {
      return this.message.split('').reverse().join('')
    },
    summary () {
      if (this.content.length > 10) {
        return this.content.slice(0, 7) + '...'
      }
      return this.content
    }
  },
  watch: {
    g (value) {
      this.g = value
      this.kg = value / 1000
      this.t = value / 1000 / 1000
    },
    kg (value) {
      this.g = value * 1000
      this.kg = value
      this.t = value / 1000
    },
    t (value) {
      this.g = value * 1000 * 1000
      this.kg = value * 1000
      this.t = value
    },
    a (newValue, oldValue) {
      this.b = oldValue
    }
  },
  // 局部註冊的元件
  components: {
    // 可以用var變數或是直接寫在這裡面
    'local-component': localComponent,
    'my-text': {
      props: ['myMessage'],
      template: '<p>{{ myMessage }}</p>'
    },
    // 加單引號或不加都可以
    'home': home,
    about: about,
    contract: contract
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2, h3 {
  font-weight: normal;
  color: #ff0000;
}
h4 {
  color: #e52684;
}
</style>
