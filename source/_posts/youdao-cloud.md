---
title: 2021.2项目问题总结
date: 2021-03-09 18:06:08
tags:
---
# 项目问题总结
>### el-input***正则限制***
```
//输入必须为正整数
<el-input v-model="num" onkeyup="value=value.replace(/^(0+)|[^\d]+/g,'')"></el-input>
//只能输入数字
oninput="value=value.replace(/[^\d]/g,'')"
//只能输入数字和小数，小数且只能输入2位，第一位不能输入小数点
oninput="value=value.replace(/[^\d.]/g, '').replace(/\.{2,}/g, '.').replace('.', '$#$')  
.replace(/\./g, '').replace('$#$', '.')  
.replace(/^(\-)*(\d+)\.(\d\d).*$/, '$1$2.$3').replace(/^\./g, '')"
//element ui 自带的只能输入数字，且只有2位小数
:controls="false"去掉按钮
:precision="2"只能输入2位小数
如需要输入整数去掉precision就可
```
---
>### el-select***可选择可输入***实现
    <html>
        <el-select
          v-model="saveWardForm.wardCode"
          placeholder=""
          filterable
          @blur="selectBlur">
          <el-option
            v-for="(item, index) in virtualWardData"
            :label="item.wardName"
            :value="item.wardCode"
            :key="index"
          ></el-option>
        </el-select>
    </html>
```
methods: {
    selectBlur(e){
      this.saveWardForm.wardCode = e.target.value
    }
}
```
---
>### element-ui上传组件上传文件***闪动***的问题
```
//根本原因是使用el-upload自带的success方法进行文件操作,底层逻辑是因为  
//其自带的数组含有__ob__，自己定义的没有这个参数。
handleUploadPatternListSuccess(response, file, fileList) {
  // 成功实现的版本1
  this.infoForm.effect.push(file)

  // 成功实现的版本2
  this.infoForm.effect = fileList
}
```
---
### 浏览器查询链接
[必应](https://www.bing.com, '好用的浏览器')  
<https://www.baidu.com>
[![美女](https://desk-fd.zol-img.com.cn/t_s640x530c5/g6/M00/03/05/ChMkKmAd-R-IGbg9AAmT2xHAEmcAAJfgwPApwUACZPz463.jpg)](https://desk.zol.com.cn/bizhi/9569_116032_2.html)

---
>### highcharts中时间轴间隔问题
```
xAxis: {
    //表示为时间，注意大小写
    type: 'datetime',
    //间距，时间戳，以下表示间距为1天，如果想表示间距为1周，就这么写
    //7*24*3600*1000
    tickInterval:  24 * 3600 * 1000,
    //格式化时间，day,week....
    dateTimeLabelFormats: {
        day: '%Y-%m-%d'
    }
}
```