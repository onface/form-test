# form-test

> The form data validation library.Does not contain UI.

## required

````js
var Test = require('form-test')
console.info('---------- required -----------')
new Test().check('some', {
    name: 'user',
    tests: [
        {
            rule: 'required'
        }
    ]
}, {
    finish: function () {
        console.log('"some"', 'finish')
    },
    pass: function () {
        console.info('"some"', 'pass')
    },
    fail: function (errros) {
        console.error('"some"', errros[0].errorMsg)
    }
})

new Test().check('', {
    name: 'user',
    tests: [
        {
            rule: 'required'
        }
    ]
}, {
    finish: function () {
        console.log('""', 'finish')
    },
    pass: function () {
        console.log('""', 'pass')
    },
    fail: function (errros) {
        console.error('""', errros[0].errorMsg)
    }
})
````

## email

````js
console.info('---------- email -----------')
new Test().check('', {
    name: 'user email',
    tests: [
        {
            rule: 'required'
        },
        {
            rule: 'email'
        }
    ]
}, {
    finish: function () {
        console.log('""', 'finish')
    },
    pass: function () {
        console.log('""', 'pass')
    },
    fail: function (errros) {
        console.error('""', errros[0].errorMsg)
    }
})
new Test().check('mail@qq', {
    name: 'user email',
    tests: [
        {
            rule: 'required'
        },
        {
            rule: 'email'
        }
    ]
}, {
    finish: function () {
        console.log('"mail@qq"', 'finish')
    },
    pass: function () {
        console.info('"mail@qq"', 'pass')
    },
    fail: function (errros) {
        console.error('"mail@qq"', errros[0].errorMsg)
    }
})
new Test().check('mail@qq.cc', {
    name: 'user email',
    tests: [
        {
            rule: 'required'
        },
        {
            rule: 'email'
        }
    ]
}, {
    finish: function () {
        console.log('"mail@qq.cc"', 'finish')
    },
    pass: function () {
        console.info('"mail@qq.cc"', 'pass')
    },
    fail: function (errros) {
        console.error('"mail@qq.cc"', errros[0].errorMsg)
    }
})
````

## msg

````js
console.info('---------- msg -----------')
new FormTest().check('123', {
    name: '用户名',
    tests: [
        {
            regexp: /abc/,
            be: true,
            some: '123',
            msg: '{{ name }}必须存在abc {{self.some}}'
        }
    ]
}, {
    fail: function (errors) {
        console.log(errors[0])
    }
})
````

## default rule

`required` `email` `url` `easyurl` `mobile`

## addRule

````js
console.info('---------- addRule -----------')
var test = new FormTest()
test.addRule('sensitiveWord', {
    regexp: /(yamadie|yikuyiku)/,
    be: false,
    msg: "{{name}}存在敏感词"
})

test.check('yamadie', {
    name: '昵称',
    tests: [
        {
            rule: 'sensitiveWord'
        }
    ]
}, {
    fail: function (errors) {
        console.log(errors[0])
    }
})
````

## async

````js
console.info('---------- async -----------')
new FormTest().check('abc', {
    tests: [
        {
            async: function (pass, fail) {
                // mock ajax
                setTimeout(function () {
                    fail('异步错误消息')
                }, 200)
            }
        }
    ]
}, {
    asyncFail: function (error) {
        console.error(error.errorMsg)
    },
    finish: function () {
        console.log('async finish')
    }
})
````

## equal

````js
console.info('---------- equal -----------')
new FormTest().check('123', {
    name: '重复密码',
    tests: [
        {
            equal: '1234',
            msg: '两次输入密码不一致'
        }
    ]
}, {
    fail: function (errors) {
        console.error(errors[0])
    }
})
new FormTest().check('123', {
    name: '重复密码',
    tests: [
        {
            equal: '123',
            msg: '两次输入密码不一致'
        }
    ]
}, {
    pass: function () {
        console.info('重复密码验证通过')
    }
})
````

## every

````js
console.info('---------- every -----------')
new FormTest().check('abc', {
    name: '用户名',
    every: true,
    tests: [
        {
            regexp: /\d/,
            be: true,
            msg: '{{name}}必须存在数字'
        },
        {
            min: 5,
            msg: '{{name}}必须大于或等于5位'
        },
        {
            async: function (pass, fail) {
                setTimeout(function () {
                    fail('异步错误消息')
                }, 200)
            }
        }
    ]
}, {
    asyncFail: function (error) {
        console.error(error)
    },
    fail: function (errors) {
        console.error('every')
        console.table(errors)
    }
})
````


## fn

````js
console.info('---------- fn -----------')
new FormTest().check('123', {
    name: '函数校验',
    tests: [
        {
            fn: function (value) {
                // fail
                return "fn 错误消息"
                // pass
                // return
            }
        }
    ]
}, {
    fail: function (errors) {
        console.error(errors[0].errorMsg)
    }
})
````

## min-max

````js
console.info('---------- min-max -----------')
new FormTest().check('123', {
    name: '密码',
    tests: [
        {
            min: 5,
            msg: '密码最少{{self.min}}位'
        }
    ]
}, {
    fail: function (errors) {
        console.log(errors[0])
    }
})
````

## regexp

## be-true

````js
console.info('---------- regexp -----------')
new FormTest().check('123', {
    name: '用户名',
    tests: [
        {
            regexp: /abc/,
            be: true,
            msg: '{{ name }}必须存在abc'
        }
    ]
}, {
    fail: function (errors) {
        console.error(errors[0])
    }
})
````

### be-false

````js
new FormTest().check('123', {
    name: '用户名',
    tests: [
        {
            regexp: /abc/,
            be: false,
            msg: '{{ name }}必须存在abc'
        }
    ]
}, {
    pass: function () {
        console.info('regexp be-false pass')
    }
})
````

## replaceRule

````js
console.info('---------- replaceRule -----------')
var test = new FormTest()
test.addRule('sensitiveWord', {
    regexp: /(yamadie|yikuyiku)/,
    be: false,
    msg: "{{name}}存在敏感词"
})
test.replaceRule('sensitiveWord', {
    regexp: /(yamadie|yikuyiku)/,
    be: false,
    msg: "{{name}}敏感!!!!!!!"
})
test.check('yamadie', {
    name: '昵称',
    tests: [
        {
            rule: 'sensitiveWord'
        }
    ]
}, {
    fail: function (errors) {
        console.error(errors[0])
    }
})
````