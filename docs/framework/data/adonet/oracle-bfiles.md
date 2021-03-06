---
title: Oracle BFILE
ms.date: 03/30/2017
ms.assetid: 341bbf84-4734-4d44-8723-ccedee954e21
ms.openlocfilehash: 40060a7ea8576e08140d972072d086606d640366
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79149435"
---
# <a name="oracle-bfiles"></a>Oracle BFILE
Oracle .NET Framework 数据提供程序包括 <xref:System.Data.OracleClient.OracleBFile> 类，该类用于使用 Oracle <xref:System.Data.OracleClient.OracleType.BFile> 数据类型。  
  
 Oracle **BFILE**数据类型是 Oracle **LOB**数据类型，其中包含对最大大小为 4 GB 的二进制数据的引用。 Oracle **BFILE**与其他 Oracle **LOB**数据类型不同，其数据存储在操作系统中的物理文件中，而不是存储在服务器上。 请注意 **，BFILE**数据类型提供对数据的只读访问。  
  
 **BFILE**数据类型区别于**LOB**数据类型的其他特征是：  
  
- 包含非结构化数据。  
  
- 支持服务器端分块。  
  
- 使用引用复制语义。 例如，如果对**BFILE**执行复制操作，则仅复制**BFILE**定位器（即对文件的引用）。 而不会复制文件中的数据。  
  
 **BFILE**数据类型应用于引用大尺寸的 LOB，因此，在数据库中存储起来不可行。 与**LOB**数据类型相比，使用**BFILE**数据类型时，涉及更多的客户端、服务器和通信开销。 如果您只需要获取少量数据，则访问**BFILE**会更有效。 如果需要获取整个对象，访问数据库驻留的 LOB 会更加有效。  
  
 每个非 NULL **OracleBFile**对象都与定义基础物理文件位置的两个实体相关联：  
  
1. 一个 Oracle DIRECTORY 对象，它是文件系统中一个目录的数据库别名，以及  
  
2. 基础物理文件的文件名，它位于与 DIRECTORY 对象关联的目录中。  
  
## <a name="example"></a>示例  
 以下 C# 示例演示如何在 Oracle 表中创建**BFILE，** 然后以**OracleBFile**对象的形式检索它。 该示例演示了使用<xref:System.Data.OracleClient.OracleDataReader>对象和**OracleBFile** **查找**和**读取**方法。 请注意，要使用此示例，必须首先在 Oracle 服务器上创建名为"c：\\\bfile"的目录和名为"MyFile.jpg"的文件。  
  
```csharp  
using System;  
using System.IO;  
using System.Data;  
using System.Data.OracleClient;  
  
public class Sample  
{  
   public static void Main(string[] args)  
   {  
      OracleConnection connection = new OracleConnection(  
        "Data Source=Oracle8i;Integrated Security=yes");  
      connection.Open();  
  
      OracleCommand command = connection.CreateCommand();  
      command.CommandText =
        "CREATE or REPLACE DIRECTORY MyDir as 'c:\\bfiles'";  
      command.ExecuteNonQuery();  
      command.CommandText =
        "DROP TABLE MyBFileTable";  
      try {  
        command.ExecuteNonQuery();  
      }  
      catch {  
      }  
      command.CommandText =
        "CREATE TABLE MyBFileTable(col1 number, col2 BFILE)";  
      command.ExecuteNonQuery();  
      command.CommandText =
        "INSERT INTO MyBFileTable values ('2', BFILENAME('MyDir', " +  
        "'MyFile.jpg'))";  
      command.ExecuteNonQuery();  
      command.CommandText = "SELECT * FROM MyBFileTable";  
  
        byte[] buffer = new byte[100];  
  
      OracleDataReader reader = command.ExecuteReader();  
      using (reader) {  
          if (reader.Read()) {  
                OracleBFile bFile = reader.GetOracleBFile(1);  
                using (bFile) {  
                  bFile.Seek(0, SeekOrigin.Begin);  
                  bFile.Read(buffer, 0, 100);  
              }  
          }  
      }  
  
      connection.Close();  
   }  
  
}  
```  
  
## <a name="see-also"></a>另请参阅

- [Oracle 和 ADO.NET](oracle-and-adonet.md)
- [ADO.NET 概述](ado-net-overview.md)
