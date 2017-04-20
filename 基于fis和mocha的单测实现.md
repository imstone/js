
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
语句覆盖：保证每一个语句都执行到了
判定覆盖（分支覆盖）：保证每一个分支都执行到
条件覆盖：保证每一个条件都覆盖到true和false（即if、while中的条件语句）
路径覆盖：保证每一个路径都覆盖到
