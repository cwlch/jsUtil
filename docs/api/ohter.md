# 其他类
## extend
### 对象拷贝，与JQ的extend类似（对象继承）
### 支持(深拷贝，浅拷贝)，对象中的方法同样也会拷贝（继承）
:::tip 参数
+ target {Object | Array} 第一个对象参数，为继承者。表示后面的所有args参数都会拷贝至这个对象
+ [...args]{Object | Array} 可以为无限个对象。被继承者，后一个覆盖前一个的属性
+ deep {Blooen} 最后一位参数，如果为true或是正常的agrs参数则执行深拷贝，如果为false，则执行前拷贝
:::

- 示例
```javascript
    let myA = {
        name : 'a',
        arr : [{arr1:'arr1'},{arr2:'arr2'},{arr3:'arr3'},{arr4:'arr4'}],
        obj : {
            obj1 : 'obj1',
            obj2 : {
                obj21 : 'obj21',
                obj22 : 'obj22'
            },
            fn (){
                alert('a')
            }
        }
        
    }
    let copayA = {
        name : 'copay-a',
        age : 'copay-age',
        arr : [{arr1 : 'copay-arr1'},{arr22:'copay-arr22'},'copay-arr3'],
        obj : {
            obj2 : {
                obj22 : 'copy-obj22'
            },
            fn(){
                alert('copy-a')
            }
        }
    }
    // demo一 深拷贝
    myA.obj.fn()  //alert 'a'
    extend(myA,copayA);
    myA.obj.fn() //alert 'copy-a'
    console.log('myA:',myA,'copayA:',copayA)
    myA = {
        'name' : 'copay-a',
        'age' : 'copay-age',
        'arr' : [{arr1:'copay-arr1'},{arr2:'arr2',arr22:'copay-arr22'},'copay-arr3',{arr4:'arr4'}],
        'obj' : {
            obj1 : 'obj1',
            obj2 : {
                obj21 : 'obj21',
                obj22 : 'copy-obj22'
            },
            fn (){
                alert('copy-a')
            }
        }
    }
    
    // demo二 浅拷贝 每次只测试一种demo
    // myA.obj.fn();
    // extend(myA,copayA,false);
    // myA.obj.fn();
    // console.log('myA:',myA,'-----copayA:',copayA);
    
    // demo三 如果要想不改变原有的对象，第一个参数传个空对象或空数组即可
    // myA.obj.fn();
    // const newObj = extend({},myA,copayA);
    // myA.obj.fn();
    // newObj.obj.fn()
    // console.log('newObj:',newObj, '------myA:',myA,'-----copayA:',copayA);

    // demo四 多个对象拷贝,并且执行浅拷贝
    // myA.obj.fn();
    // const newObj = extend({},copay1,copayn2,....,copayn+,false);
    // myA.obj.fn();
    // newObj.obj.fn()
    // console.log('newObj:',newObj, '------myA:',myA,'-----copayA:',copayA);
```

## loadScript
### 动态加载JS（并联）如果是多个js，他会同时加载
:::tip 参数
+ scriptStr {String | Array}  js地址，单个可以直接使用字符串，多个使用数组形式； 
:::
:::tip return
+ Promise 所有JS加载完成后，执行Promise成功回调
:::
- 示例
```javascript
    // a.js console.log('1') 
    // b.js console.log('2')
    // c.js console.log('3')

    // 加载多个JS
    loadScript(['./a.js','./b.js','./c.js']).then(res=>{
        console.log('加载成功') //此处输出一定会在其他JS都输出完才会执行
    }); //输出2,3,1 顺序不固定


    // 加载单个JS
    loadScript('./a.js').then(res=>{
        console.log('加载成功') //此处输出一定会在其他JS都输出完才会执行
    }); 
    loadScript(['./a.js']).then(res=>{
        console.log('加载成功') //此处输出一定会在其他JS都输出完才会执行
    }); 
```
## loadScriptAwait
### 动态加载JS（串联）如果是多个js，他会按照数组顺序一个一个的加载
:::tip 参数
+ scriptStr {String | Array}  js地址，单个可以直接使用字符串，多个使用数组形式； 
:::
:::tip return
+ Promise 所有JS加载完成后，执行Promise成功回调
:::

- 示例
```javascript
    // a.js console.log('1') 
    // b.js console.log('2')
    // c.js console.log('3')

    // 加载多个JS
    loadScriptAwait(['./a.js','./b.js','./c.js']).then(res=>{
        console.log('加载成功') 
    }); 
    //输出1，2，3，加载成功，会按照数组顺序固定输出，最后输出then方法的回调
```
:::warning 建议
如果加载单个JS建议使用 loadScript
:::


## fillZero
### 小于10正整数补0
:::tip 参数
+ number {Number | String} 数字
:::
:::tip return
+ String | Number 小于10的正整数，返回带不齐0的字符串，其他原样返回参数
:::
- 示例
```javascript
    fillZero(1)    // 01
    fillZero(0.1)    // 0.1
    fillZero(-0.1)    // -0.1
    fillZero('2')  // 02
    fillZero('11') // 11
```
## creatUuid
### 创建一个UUID
:::tip 参数
+ len 指定长度，非必传
+ radix 基数,非必传
:::

- 示例
```javascript
    creatUuid() // EB4479E4-CA4A-4377-AC97-5C3424CCA6EF
    creatUuid(32, 2)  //  "10010100000011100011101010001011"
    creatUuid(32, 10) // "82841022800573848567251899801403"
    creatUuid(32, 16) // "F743098834031FDFCE1D8976F9769222"
```
## downBlobFile
### 下载二进制流文件到本地
:::tip 参数
+ blob {Blob} 二进制文件流对象 
+ fileOptin {Object} 下载文件配置参数
    + name {String} 文件名，非必传。不传时自动取blob对象的name属性，如果没有name属性取当前时间戳
    + suffix {String} 扩展名，非必传。不传时从blob对象的name属性截取，如果没有name属性，则通过blob对象的type判断
:::

- 示例
```html

    <input type="file" id="input">
    <button id='btn'>下载刚上传的图片</button>
    <script>
        const $input =  document.querySelector('#input');
        const $btn =  document.querySelector('#btn');

        $btn.addEventListener('click',(e)=>{
            let myBlob = $input.files[0];
            // 下载二进制文件流到本地
            downBlobFile(myBlob);
            downBlobFile(myBlob,{name:'我的名字'});
            downBlobFile(myBlob,{suffix:'gif'});
            downBlobFile(myBlob,{name:'你的名字',suffix:'gif'});
        })
    </script>

```
## loactionReplace
### 添加了兼容的浏览器location.replace方法
:::tip 参数
+ url {String} 跳转地址
:::
- 示例
```javascript
    loactionReplace('./demo.html');
    loactionReplace('http://www.baiud.com');
```