---
title: 使用Blob将json格式数据以本地方式保存为csv文件
---
项目中遇到标题中需求，json格式数据保存为csv文件,
直接上代码。
```javascript
let json = [{
                id: 1,
                name: "小王",
                sex: "男"
            }, {
                id: 2,
                name: '小李',
                sex: "男"
            },
            {
                id: 3,
                name: '小芳',
                sex: "女"
            }]
            let outData = []
            let data = []
            for (let i in json[0]) {
                data.push(i)
            }
            outData.push(data.join())
            for (let i in json) {
                let data = []
                for (let j in json[i]) {
                    data.push(json[i][j])
                }
                outData.push(data.join())
            }
            let blobObj = new Blob(["\ufeff" + outData.join("\n")])
            let a = document.createElement('a')
            a.href = URL.createObjectURL(blobObj)
            a.download = "111.csv"
            a.click()
```  
可以注意到，csv格式的文件需要的数据格式为字符串，在同一行不同列的数据需要用“，”隔开，不同行数据需要用“\n”隔开，因此这样做可以达到需求。

#### 思考：chrome中如何把得到的csv文件进行另存为操作？
由于当前高级浏览器的安全策略限制，不能使用js获取本地文件物理地址，chrome浏览器的默认下载地址也是固定的，当然可以去进行修改达到每次下载文件时选择存储路径的目的，不过这样需要用户手动设置，不是很好。值得一提的是IE浏览器中可以使用
```javasript
document.exeCommand()
```
来实现另存为,也可以使用IE10的msSaveBlob和msSaveOrOpenBlob方法来实现另存为。
```javasript
//提供一个保存按钮
window.navigator.msSaveBlob(blobObject,"xxx.txt")
//提供打开和保存按钮
window.navigator.msSaveOrOpenBlob(blobObject,"xxx.txt")
