---
title: CREO vbapi二次开发-实用小工具-批量参数操作
tags:
  - CREO
  - VBAPI
  - CREO小工具
  - CREO二次开发
comments: true
category: CREO二次开发
date: 2017-12-02
---

本文继续做一个稍微实用的小工具，实现批量将Creo零件的参数批量增加、删除和打勾操作。为证明之前说明可以使用其他语言也可以开发，本工具在excel下的vba环境进行开发。小工具主要实现以下功能：

+ 批量导入参数。实现将选定目录下所有prt加入excel表给定的参数。当参数存在时，修改其值;

+ 批量删除参数。实现删除选定目录下所有prt中excel表中给定的参数;

+ 批量打勾参数。实现将选定目录下所有prt参数列表中的“指定”栏打勾。

程序运行界面如下图所示：

<div align="center">
    <img src="/img/proe/CreoParamTool.png" style="width:80%" align="center"/>
    <p>图 批量参数操作运行界面</p>
</div>

使用时，首先在*Creo程序*和*Creo源文件目录*两个单元格内设置好正确的目录。第七行开始设定需要导入的参数或删除的参数(删除参数时只需要填写参数名即可)，如果不够可以增加行。注意我已在参数类型单元格设置好了下拉格式，建议需添加参数时采用复制粘贴的方式保证数据的正确性。当excel表格输入完成后，点击对应的按钮，等待片刻即可完成操作。注意由于采用vba宏，所有打开xls时需要启用宏。  

excel文件可在<a href="https://github.com/slacker-HD/creo_vbapi" target="_blank">Github.com</a>下载。
