---
title: "Import native & character format data from earlier versions of SQL Server"
description: In SQL Server 2019, you can use bcp to import native and character format data from other versions of SQL Server by using the -V switch with a qualifier.
author: rwestMSFT
ms.author: randolphwest
ms.date: "03/14/2017"
ms.service: sql
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: seo-lt-2019
helpviewer_keywords:
  - "earlier versions [SQL Server], import and export data formats"
  - "-V switch"
  - "data formats [SQL Server], earlier versions"
  - "previous versions [SQL Server], import and export data formats"
monikerRange: ">=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current"
---
# Import native and character format data from earlier versions of SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In [!INCLUDE [sssql14-md](../../includes/sssql14-md.md)] and later, you can use **bcp** to import native and character format data from [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], or [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] by using the **-V** switch. The **-V** switch causes [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] to use data types from the specified earlier version of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], and the data file format are the same as the format in that earlier version.  
  
To specify an earlier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version for a data file, use the **-V** switch with one of the following qualifiers:  
  
|SQL Server version|Qualifier|  
|------------------------|---------------|  
|[!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]|**-V80**|  
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|**-V90**|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|**-V100**|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|**-V 110**|  
  
## Interpretation of Data Types  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] and later versions have support for some new types. When you want to import a new data type into an earlier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version, the data type must be stored in a format that readable by the older **bcp** clients. The following table summarizes how the new data types are converted for compatibility with the earlier versions of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|New data types in SQL Server 2005|Compatible data types in version 6*x*|Compatible data types in version 70|Compatible data types in version 80|  
|---------------------------------------|-------------------------------------------|-----------------------------------------|-----------------------------------------|  
|**bigint**|**decimal**|**decimal**|*|  
|**sql_variant**|**text**|**nvarchar(4000)**|*|  
|**varchar(max)**|**text**|**text**|**text**|  
|**nvarchar(max)**|**ntext**|**ntext**|**ntext**|  
|**varbinary(max)**|**image**|**image**|**image**|  
|XML|**ntext**|**ntext**|**ntext**|  
|UDT**|**image**|**image**|**image**|  
  
 *This type is natively supported.  
  
 **UDT indicates a user defined type.  
  
## Exporting using -V 80  
 When you bulk export data by using the **-V80** switch, **nvarchar(max)**, **varchar(max)**, **varbinary(max)**, XML, and UDT data in native mode are stored with a 4-byte prefix, like **text**, **image**, and **ntext** data, rather than with an 8-byte prefix, which is the default for [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] and later versions.  
  
## Copying Date Values  
 **bcp** uses the ODBC bulk copy API. Therefore, to import date values into [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], **bcp** uses the ODBC date format (*yyyy-mm-dd hh:mm:ss*[*.f...*]).  
  
 The **bcp** command exports character format data files using the ODBC default format for **datetime** and **smalldatetime** values. For example, a **datetime** column containing the date `12 Aug 1998` is bulk copied to a data file as the character string `1998-08-12 00:00:00.000`.  
  
> [!IMPORTANT]  
>  When importing data into a **smalldatetime** field using **bcp**, be sure the value for seconds is 00.000; otherwise the operation will fail. The **smalldatetime** data type only holds values to the nearest minute. BULK INSERT and INSERT ... SELECT * FROM OPENROWSET(BULK...) will not fail in this instance but will truncate the seconds value.  
  
##  <a name="RelatedTasks"></a> Related Tasks  
 **To use data formats for bulk import or bulk export**  
  
-   [Use Character Format to Import or Export Data &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Use Native Format to Import or Export Data &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Use Unicode Character Format to Import or Export Data &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Use Unicode Native Format to Import or Export Data &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## See Also  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Data Types &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [SQL Server Database Engine Backward Compatibility](../../database-engine/discontinued-database-engine-functionality-in-sql-server.md)   
 [CAST and CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
