---
title: 如何调试空查询结果集 (C#)
description: 了解如何调试空查询结果集。 如果开发人员在编写查询时将 XML 视为不在命名空间内，则可能会出现这些集。
ms.date: 07/20/2015
ms.assetid: b569f0dc-425e-45a6-acbf-770fb761c981
ms.openlocfilehash: ad6d39697e5a59fe23ca700ceeb2a9860d05bb94
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302901"
---
# <a name="how-to-debug-empty-query-results-sets-c"></a>如何调试空查询结果集 (C#)
查询 XML 树时遇到的一个最常见问题是，如果 XML 树具有默认命名空间，开发人员在编写查询时，有时会将 XML 视为不在命名空间内。  
  
 本主题的第一个示例集演示一种加载并按不正确方式查询默认命名空间中的 XML 的典型方式。  
  
 第二个示例集演示必需的更正，以便可以查询命名空间中的 XML。  
  
 有关详细信息，请参阅[命名空间概述(LINQ to XML) (C#)](namespaces-overview-linq-to-xml.md)。  
  
## <a name="example"></a>示例  
 此示例演示如何在命名空间中创建 XML 和一个返回空结果集的查询。  
  
```csharp  
XElement root = XElement.Parse(  
@"<Root xmlns='http://www.adventure-works.com'>  
    <Child>1</Child>  
    <Child>2</Child>  
    <Child>3</Child>  
    <AnotherChild>4</AnotherChild>  
    <AnotherChild>5</AnotherChild>  
    <AnotherChild>6</AnotherChild>  
</Root>");  
IEnumerable<XElement> c1 =  
    from el in root.Elements("Child")  
    select el;  
Console.WriteLine("Result set follows:");  
foreach (XElement el in c1)  
    Console.WriteLine((int)el);  
Console.WriteLine("End of result set");  
```  
  
 此示例产生下面的结果：  
  
```output  
Result set follows:  
End of result set  
```  
  
## <a name="example"></a>示例  
 本示例演示如何在命名空间中创建 XML 和一个正确编码的查询。  
  
 解决方案为声明和初始化一个 <xref:System.Xml.Linq.XNamespace> 对象，并在指定 <xref:System.Xml.Linq.XName> 对象时使用该对象。 在这种情况下，<xref:System.Xml.Linq.XContainer.Elements%2A> 方法的参数是一个 <xref:System.Xml.Linq.XName> 对象。  
  
```csharp  
XElement root = XElement.Parse(  
@"<Root xmlns='http://www.adventure-works.com'>  
    <Child>1</Child>  
    <Child>2</Child>  
    <Child>3</Child>  
    <AnotherChild>4</AnotherChild>  
    <AnotherChild>5</AnotherChild>  
    <AnotherChild>6</AnotherChild>  
</Root>");  
XNamespace aw = "http://www.adventure-works.com";  
IEnumerable<XElement> c1 =  
    from el in root.Elements(aw + "Child")  
    select el;  
Console.WriteLine("Result set follows:");  
foreach (XElement el in c1)  
    Console.WriteLine((int)el);  
Console.WriteLine("End of result set");  
```  
  
 此示例产生下面的结果：  
  
```output  
Result set follows:  
1  
2  
3  
End of result set  
```  
