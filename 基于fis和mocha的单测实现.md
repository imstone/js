## 引子和说明
1. 本文介绍的是基于FIS3针对VUEX的单测环境搭建，也可以作为其他框架的参考。
2. 不再详细介绍各个框架的详细用法，只是针对各个框架在FIS3环境下的整合。


## 框架介绍
1. Mocha [ˈmoʊkə]摩卡
2. istanbul [ɪstænˈbul]伊斯坦布尔
3. chai 


<img src="http://cldup.com/xFVFxOioAU.svg" width = "100" height = "100" style="float:left"/>
<img src="https://camo.githubusercontent.com/431283cc1643d02167aac31067137897507c60fc/687474703a2f2f636861696a732e636f6d2f696d672f636861692d6c6f676f2e706e67" width = "90" height = "100" />


## 目录和文件名约定
#### 测试目录约定
测试文件目录和源文件src平行的test目录下，test目录下又分为unit和e2e测试文件夹，以及README.md
```
-/           根目录
----|src         业务相关的 JavaScript 代码目录
        |____ JS              需要测试的JS文件
        |____ other
      ....
----|test         测试相关的 JavaScript 代码目录
        |____ unit             单元测试文件目录
                |____ JS              从这里以下按照src下的JS文件夹一一对应写单元测试文件
        |____ e2e              功能测试文件目录
        |____ README.md        
```
#### 测试文件约定
> 测试文件的目录必须与要测试的文件一一对应，包括目录结构和文件名。

文件名必须以test.js结尾。例如: site.test.js 对应要测试的文件就是site.js

##  FIS3 环境配置
#### 针对单测单独运行的FIS配置
> 使用环境查询
```
var test = fis.media('test');
```

#### 使用ES6语法
> 需要测试的JS文件和单测文件使用ES6编码，使用bable转码为es5。
```
test.match('**.js', {
    parser: fis.plugin('babel-5.x'),
});
```

#### 针对BABLE，修改require
> FIS没有针对JS里require的资源定位，所以我们需要对经过bable转义过的require地址做一些处理。

```
test.hook('nodejs');
```
##### 下面是 fis3-hook-nodejs这个插件的源码
> 主要针对一些require的相对路径转换为绝对路径
``` javascript
  var content = file.getContent();
  var path = file.fullname.split(file.subpath)[0];
  content = content.replace(/require\(\'src\/js\/api/gi , 'require\(\''+path+'/testout/test/unit/_tools/api');
  content = content.replace(/require\(\'src/gi , 'require\(\''+path+'/testout/src');
  content = content.replace(/require\(\'node_modules/gi , 'require\(\''+path+'/node_modules')
  content = content.replace(/require\(\'_mock/gi , 'require\(\''+path+'/_mock')
  content = content.replace(/require\(\'test/gi , 'require\(\''+path+'/testout/test')
```
#### 针对一些框架不输出
> 测试用例和需要测试的JS文件里会有一些框架引用，我们要忽略这些框架的输出。
``` javascript
test.match('chai/**', {
    release: false
});
test.match('axios/**', {
    release: false
});
test.match('lodash/**', {
    release: false
});
```
#### 将单测文件和需要测试的文件输出到单独文件夹。
> 经过bable转义和修改require地址的JS文件输出到一个单独文件夹，这些文件就是可测文件了
```
test.match('**.js', {
    isMod: false,
    deploy: [
        fis.plugin('local-deliver', {
            to: 'testout'
        })
    ]
});

```

### 整体fis3配置
```javascript
var test = fis.media('test');
test.set('project.files', ['./test/**',  './_mock/**']);
test.match('chai/**', {
    release: false
});
test.match('axios/**', {
    release: false
});
test.match('lodash/**', {
    release: false
});
test.match('**.js', {
    parser: fis.plugin('babel-5.x'),
});
test.hook('nodejs');
test.match('**.js', {
    isMod: false,
    deploy: [
        fis.plugin('local-deliver', {
            to: 'testout'
        })
    ]
});
```

### 测试框架 Mocha和断言库chai 整合
>  引入要测试文件,
#### 执行每个单测前进行state和mock数据的重置
```javascript
let inlineData= null;
let inlineState = null;
beforeEach(function() {
    // 每个测试用例执行前都重新cloe一份新的数据
    inlineData = _.cloneDeep(data);
    inlineState = _.cloneDeep(state);
});
```
#### 针对mutation的测试
```javascript
it('测试mutations中的SET-SITE', () => {
        // 模拟状态
        const _state = {
            all: result,
            info: {}
        };
        // 应用 mutation
        SET_SITE(inlineState, result);
        // 断言结果
        expect(inlineState).to.deep.equal(_state)
});
```
#### 针对getters的测试

#### 针对action的测试


### 数据的mock

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

1. 行覆盖率（line coverage）：是否每一行都执行了？
2. 函数覆盖率（function coverage）：是否每个函数都调用了？
3. 分支覆盖率（branch coverage）：是否每个if代码块都执行了？
4. 语句覆盖率（statement coverage）：是否每个语句都执行了？


