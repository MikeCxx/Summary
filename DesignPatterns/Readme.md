# 策略模式
```JavaScript
// 表单数据
    const data = {
        info: ''
    }
    // 策略类
    const reportStrategy = {
        /**
        * 判断是否为空？
        * @param {any} val 值
        * @param {string} errorMsg 报错信息
        * @returns string | undeifined
        */
        empty(val, errorMsg) {
            if (Array.isArray(val)) {
                if (val.length === 0) {
                    return errorMsg
                }
            } else {
                if (val !== 0 && !val) {
                    return errorMsg
                }
            }
        },
        /**
        * 传入一个数组，判断至少一个值不能为空
        * @param {Array} val
        * @param {string} errorMsg
        */
        atLeastOne(val, errorMsg) {
            if (!val.some(v => v !== 0 && v)) {
                return errorMsg
            }
        }
    }

    // 验证类
    class Validator {
        constructor(strategys) {
            this.rules = []
            this.strategys = strategys
        }
        /**
        * 添加规则
        * @param {any} val 需要验证的值
        * @param {string} ruleName 规则名
        * @param {string} errMsg 验证提示
        */
        add(val, ruleName, errMsg) {
            this.rules.push({
                val: val,
                ruleName: ruleName,
                errMsg: errMsg
            })
            // 链式调用
            return this 
        }
        /**
        * 验证规则
        */
        check() {
            for (const item of this.rules) {
                const { val, ruleName, errMsg } = item
                const res = this.strategys[ruleName](val, errMsg)
                if (res) {
                    Toast(res)
                    return false
                }
            }
            return true
        }
    }

    // 初始化实例
    const validator = new Validator(reportStrategy)
    validator.add(
        data.info,
        'empty',
        '信息不能为空'
    )
    validator.check() // false
```