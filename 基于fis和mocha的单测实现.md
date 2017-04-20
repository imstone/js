
## Mocha（发音"摩卡"）
Mocha是基于node的测试框架，

```javascript

test.set('project.files', ['./test/**']);

test.match('**.js', {
    parser: fis.plugin('babel-5.x'),
    packTo: 'static/pkg/folderA.js'
});
test.match('**.js', {
    parser: fis.plugin('babel-5.x'),
    packTo: 'static/pkg/folderA.js'
});
test.match('::package', {
    // prepackager: fis.plugin('mocha'),
    postpackager: fis.plugin('loader', {
        allInOne: true
    })
});
test.match('mod-node.js', {
    packOrder: -100,
    isMod: false
});
test.match('**', {
    deploy: [
        fis.plugin('local-deliver', {
            to: 'testout'
        })
    ]
});
```
1. 行覆盖率（line coverage）：是否每一行都执行了？
2. 函数覆盖率（function coverage）：是否每个函数都调用了？
3. 分支覆盖率（branch coverage）：是否每个if代码块都执行了？
4. 语句覆盖率（statement coverage）：是否每个语句都执行了？
