
## 框架介绍
1. Mocha [ˈmoʊkə]摩卡
2. istanbul [ɪstænˈbul]伊斯坦布尔
3. chai 
Mocha是基于node的测试框架，

<img src="http://cldup.com/xFVFxOioAU.svg" width = "100" height = "100" style="float:left"/>
<img src="https://camo.githubusercontent.com/431283cc1643d02167aac31067137897507c60fc/687474703a2f2f636861696a732e636f6d2f696d672f636861692d6c6f676f2e706e67" width = "90" height = "100" />

### Mocha
>  测试框架


Should
```
chai.should();
foo.should.be.a('string');
foo.should.equal('bar');
foo.should.have.lengthOf(3);
tea.should.have.property('flavors')
  .with.lengthOf(3);
```
                
Visit Should Guide →
Expect
```
var expect = chai.expect;

expect(foo).to.be.a('string');
expect(foo).to.equal('bar');
expect(foo).to.have.lengthOf(3);
expect(tea).to.have.property('flavors')
  .with.lengthOf(3);
```              

Assert
```
var assert = chai.assert;

assert.typeOf(foo, 'string');
assert.equal(foo, 'bar');
assert.lengthOf(foo, 3)
assert.property(tea, 'flavors');
assert.lengthOf(tea.flavors, 3);
```
<img src="http://cldup.com/xFVFxOioAU.svg" width = "100" height = "100" alt="图片名称" align=center />
<img src="https://camo.githubusercontent.com/431283cc1643d02167aac31067137897507c60fc/687474703a2f2f636861696a732e636f6d2f696d672f636861692d6c6f676f2e706e67" width = "90" height = "100" alt="图片名称" align=center />

istanbul [ɪstænˈbul]伊斯坦布尔

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
