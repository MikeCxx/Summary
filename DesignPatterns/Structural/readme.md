# 观察者

```JavaScript
// 被观察者
class Subject {
    constructor(name) {
        this.name = name || '无情的被观察者'
        this.list = []
    }
    addObs(ob) {
        this.list.push(ob)
    }
    // 更新发布
    publish() {
        this.list.forEach( o => o.update(this))
    }
}

// 观察者
class Observer {
    constructor(name) {
        // 观察者名字
        this.name = name || '冷漠的机器人'
        
    }
    update(subject) {
        console.error(`${subject.name}改变了被观察者${this.name}发现了`)
    }
}

// 实例化被观察者
var ob1 = new Subject('机器人一号')
var ob2 = new Subject('机器人二号')

// 实例化观察者
var god = new Observer('god')
// 被观察
ob1.addObs(god)
// 发布
ob1.publish() // console:机器人一号改变了被观察者god发现了
```

# 发布订阅

```JavaScript
class Event {
    constructor() {
        this.eList = {}
    }
    // 监听
    on(type, fn) {
        if (!this.eList[type]) {
            this.eList[type] = []
            this.eList[type].push(fn)
        } else {
            this.eList[type].push(fn)
        }
    }
    // 发布
    publish(type) {
        this.eList[type].forEach((fn) => {
            fn()
        })
    }
    // 卸载
    off (type, fn) {
        const index = this.eList[type].findIndex(f => f === fn)
        this.eList[type].splice(index, 1)
    }
    // 一次调用,封装函数，调用一次后卸载
    once(type, fn) {
        const one = () => {
            fn()
            this.off(type, fn)
        }
        this.on(type, one)
    }
}

const e1 = new Event()
e1.on('test', () => {
    console.error('高质量')
})
e1.once('test', f1)
const f1 = () => {
    console.error('what do you want from me!')
}

e1.publish('test')
console.error('-----------------')
e1.publish('test')


```