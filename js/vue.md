#### jsx写法相关
##### createElement
```js
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // 一个 HTML 标签名、组件选项对象，或者
  // resolve 了上述任何一种的一个 async 函数。必填项。
  'div',

  // {Object}
  // 一个与模板中属性对应的数据对象。可选。
  {
    // (详情见下一节)
  },

  // {String | Array}
  // 子级虚拟节点 (VNodes)，由 `createElement()` 构建而成，
  // 也可以使用字符串来生成“文本虚拟节点”。可选。
  [
    '先写一些文字',
    createElement('h1', '一则头条'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)

// 数据对象:
{
  // 与 `v-bind:class` 的 API 相同，
  // 接受一个字符串、对象或字符串和对象组成的数组
  'class': {
    foo: true,
    bar: false
  },
  // 与 `v-bind:style` 的 API 相同，
  // 接受一个字符串、对象，或对象组成的数组
  style: {
    color: 'red',
    fontSize: '14px'
  },
  // 普通的 HTML 特性
  attrs: {
    id: 'foo'
  },
  // 组件 prop
  props: {
    myProp: 'bar'
  },
  // DOM 属性
  domProps: {
    innerHTML: 'baz'
  },
  // 事件监听器在 `on` 属性内，
  // 但不再支持如 `v-on:keyup.enter` 这样的修饰器。
  // 需要在处理函数中手动检查 keyCode。
  on: {
    click: this.clickHandler
  },
  // 仅用于组件，用于监听原生事件，而不是组件内部使用
  // `vm.$emit` 触发的事件。
  nativeOn: {
    click: this.nativeClickHandler
  },
  // 自定义指令。注意，你无法对 `binding` 中的 `oldValue`
  // 赋值，因为 Vue 已经自动为你进行了同步。
  directives: [
    {
      name: 'my-custom-directive',
      value: '2',
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ],
  // 作用域插槽的格式为
  // { name: props => VNode | Array<VNode> }
  scopedSlots: {
    default: props => createElement('span', props.text)
  },
  // 如果组件是其它组件的子组件，需为插槽指定名称
  slot: 'name-of-slot',
  // 其它特殊顶层属性
  key: 'myKey',
  ref: 'myRef',
  // 如果你在渲染函数中给多个元素都应用了相同的 ref 名，
  // 那么 `$refs.myRef` 会变成一个数组。
  refInFor: true
}
```

##### slot
###### 单文件与jsx写法
```js
// App.vue
<template>
  <div id="app">
    <TestJsx>
      <template v-slot:s1>
         <div>slot</div>
      </template>
      <template v-slot:s3="{ name }">
         <div>{{name}}</div>
      </template>
    </TestJsx>
  </div>
</template>

//testjsx.vue
<script>
  export default {
    name: 'testjsx',
    data(){
      return {
        nickName: 'lucy',
      }
    },
    render(){
      // 作用域插槽scopedSlots
      return (
        <div>
          {this.$slots.s1}
          {this.$scopedSlots.s3({
            name: this.nickName
          })}
        </div>
      )
    }
  }
</script> 
```
###### jsx与jsx写法
```js
//testjsx.vue
<script>
  export default {
    name: 'testjsx',
    data(){
      return { 
        
      }
    },
    render(){
      return (
        <div>
          <Son {...{scopedSlots: { s2: props => <span>{props.text}</span>}, on: { respond: () => {}} }}/>
        </div>
      )
    }
  }
</script> 

// son.vue
<script>
  export default {
    data(){
      return {
        msg: 'son  son'
      }
    },
    methods: {
    },
    render(){
      return (
        <div>
          {this.$scopedSlots.s2({ text: this.msg })}
        </div>
      )
    }
  }
</script>
```

##### 函数式组件
```js
//app.vue
<template>
  <div id="app">
    <test-fun :count="123" class="test"/>
  </div>
</template>


// testfun.vue
<script>
  export default {
    name: 'test',
    functional: true, // 函数式组件
    props: {
      count: {
        type: Number,
        default: null,
      }
    },
    render(h, ctx){
      console.log(ctx)
      // 传递 ctx.data 外部的class="test"才会出现在组件的根元素上
      return <div {...ctx.data}>hello {ctx.props.count}</div>
    }
  }
</script> 
```