# Provider

react-redux 有两个核心概念， Provider和connect. Provider代码比较少，主要是应用了React的context api，用来给react组件提供数据。

```
export function createProvider(storeKey = 'store') {
    const subscriptionKey = `${storeKey}Subscription`

    class Provider extends Component {
        getChildContext() {
          return { [storeKey]: this[storeKey], [subscriptionKey]: null }
        }

        constructor(props, context) {
          super(props, context)
          this[storeKey] = props.store;
        }

        render() {
          return Children.only(this.props.children)
        }
    }

    if (process.env.NODE_ENV !== 'production') {
      Provider.prototype.componentWillReceiveProps = function (nextProps) {
        if (this[storeKey] !== nextProps.store) {
          warnAboutReceivingStore()
        }
      }
    }

    Provider.propTypes = {
        store: storeShape.isRequired,
        children: PropTypes.element.isRequired,
    }
    Provider.childContextTypes = {
        [storeKey]: storeShape.isRequired,
        [subscriptionKey]: subscriptionShape,
    }

    return Provider
}
export default createProvider(); // 通常使用的Provider组件
```

1. 把传入的sotre保存在this[storeKey], 在getChildContext中返回store的值
2. provider不支持sotre修改，第一次改变会警告，再次修改就退出
   
getChildContext是react context相关api：react中要使用context，需要一个父组件和至少一个子组件（redux中使用Children.only检查是否是单个子节点，所以redux只支持单个节点），
父组件中，必须通过一个静态属性childContextTypes声明提供给子组件的Context对象的属性，并实现一个实例getChildContext方法，返回一个代表Context的纯对象 (plain object) 。redux就是使用context把store通过provider传递给所有子组件。