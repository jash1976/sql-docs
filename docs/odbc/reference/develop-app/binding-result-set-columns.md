---
title: "Binding Result Set Columns"
description: "Binding Result Set Columns"
author: David-Engel
ms.author: davidengel
ms.date: "01/19/2017"
ms.service: sql
ms.subservice: connectivity
ms.topic: conceptual
helpviewer_keywords:
  - "result sets [ODBC], binding columns"
  - "binding columns [ODBC]"
---
# Binding Result Set Columns
Applications can bind as many or as few columns of the result set as they choose, including binding no columns at all. When a row of data is fetched, the driver returns the data for the bound columns to the application. Whether the application binds all of the columns in the result set depends on the application. For example, applications that generate reports usually have a fixed format; such applications create a result set containing all of the columns used in the report and then bind and retrieve the data for all of these columns. Applications that display screens full of data sometimes allow the user to decide which columns to display; such applications create a result set containing all columns the user might want, but bind and retrieve the data only for those columns chosen by the user.  
  
 Data can be retrieved from unbound columns by calling **SQLGetData**. This is commonly called to retrieve long data, which often exceeds the length of a single buffer and must be retrieved in parts.  
  
 Columns can be bound at any time, even after rows have been fetched. However, the new bindings do not take effect until the next time a row is fetched; they are not applied to data from rows already fetched.  
  
 A variable remains bound to a column until a different variable is bound to the column, until the column is unbound by calling **SQLBindCol** with a null pointer as the variable's address, until all columns are unbound by calling **SQLFreeStmt** with the SQL_UNBIND option, or until the statement is released. For this reason, the application must be sure that all bound variables remain valid as long as they are bound. For more information, see [Allocating and Freeing Buffers](../../../odbc/reference/develop-app/allocating-and-freeing-buffers.md).  
  
 Because column bindings are just information associated with the statement structure, they can be set in any order. They are also independent of the result set. For example, suppose an application binds the columns of the result set generated by the following SQL statement:  
  
```  
SELECT * FROM Orders  
```  
  
 If the application then executes the SQL statement  
  
```  
SELECT * FROM Lines  
```  
  
 on the same statement handle, the column bindings for the first result set are still in effect because those are the bindings stored in the statement structure. In most cases, this is a poor programming practice and should be avoided. Instead, the application should call **SQLFreeStmt** with the SQL_UNBIND option to unbind all the old columns and then bind new ones.
