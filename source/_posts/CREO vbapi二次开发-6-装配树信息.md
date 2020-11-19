---
title: CREO vbapi二次开发-6-装配树信息
tags:
  - CREO
  - VBAPI
  - CREO二次开发
comments: true
category: CREO二次开发
date: 2018-02-07
---

本节介绍VBAPI如何获取装配树信息。  

## 1.获取树信息

如前节所述，组件可以认为是装配体的一个特征，而IpfcAssembly类本身也是IpfcSolid的子类，故获取装配体中所有组件只需调用IpfcSolid类的ListFeaturesByType方法即可获得装配体中所有组件。ListFeaturesByType方法只能获得装配树中下一层的所有节点信息，如某一组件是也是装配体时，依然需要子装配调用ListFeaturesByType方法获得装配树下一层信息。因此只需递归调用相关方法，即可获得完整的装配树信息。递归调用获取装配树信息代码如下所示：

```vb
Private Function GetSubassemblyinfo(ByRef i As Integer, ByVal ModelDescr As IpfcModelDescriptor, ByVal level As Integer) As String
  Dim model As IpfcModel
  Dim solid As IpfcSolid
  Dim components As IpfcFeatures
  Dim info As String = ""
  Dim j As Integer
  Dim tabstr As String = ""
  Dim modelItem As IpfcModelItem
  Dim componentFeat As IpfcComponentFeat
  model = asyncConnection.Session.GetModelFromDescr(ModelDescr)
  solid = CType(model, IpfcSolid)
  components = solid.ListFeaturesByType(True, EpfcFeatureType.EpfcFEATTYPE_COMPONENT)
  For j = 0 To level
    tabstr += vbTab
  Next
  For Each component As IpfcFeature In components
    modelItem = CType(component, IpfcModelItem)
    componentFeat = CType(component, IpfcComponentFeat)
    info += tabstr + "序号：" + (i + 1).ToString() + "  ID:" + modelItem.Id.ToString() + "  名称：" + componentFeat.ModelDescr.InstanceName + "  类型：" + componentFeat.ModelDescr.GetExtension() + Chr(13)
    i = i + 1
    If componentFeat.ModelDescr.Type = EpfcModelType.EpfcMDL_ASSEMBLY Then
      info += GetSubassemblyinfo(i, componentFeat.ModelDescr, level + 1)
    End If
  Next
  Return info
End Function
```

## 2.获取约束信息

获取组件的约束信息可以通过IpfcComponentFeat类的GetConstraints方法完成。GetConstraints方法返回了一个IpfcComponentConstraint类的数组，记录了组件对应的所有约束。如"CREO vbapi二次开发-6.零件装配"文中所述，IpfcComponentConstraint记录了组件对应的约束信息，在此不在赘述。获取组件所有约束类型的代码如下：

```vb
Private Function Constrains(ByVal Component As IpfcComponentFeat) As String
  Constrains = ""
  If Component.GetConstraints() IsNot Nothing Then
    For Each constrain As IpfcComponentConstraint In Component.GetConstraints()
      Constrains += "约束类型：" + constrain.Type.ToString() + ", "
      '根据constrain对象还可以获得其它信息，这里不详细展开了
    Next
  End If
  Return Constrains
End Function
```

完整代码可在<a href="https://github.com/slacker-HD/creo_vbapi" target="_blank">Github.com</a>下载。
