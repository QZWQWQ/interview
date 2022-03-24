# react性能优化



使用react-addons-pure-render-mixin或React.PureComponent可以让组件自行判断props（浅层）的变化；帮助我们实现shouldComponentUpdate的控制，减少其他非组件相关数据变化引起的重新渲染

### 禁止传递对象字面量给组件

基于上面的情况，提供了一个eslint插件 `eslint-plugin-weapp` ，插件中的增加了一项校验规则 `jsx-no-raw-object` ，用于检查代码中传递对象字面量给组件的地方。在 `.eslintrc.json` 配置文件中，加上规则即可

```json
// .eslintrc.json
{
  "plugins": ["weapp"],
  "rules": {
    "weapp/jsx-no-raw-object": [2], // 启用 jsx-no-raw-object 规则，并设置为报错提示
  }
}
```

