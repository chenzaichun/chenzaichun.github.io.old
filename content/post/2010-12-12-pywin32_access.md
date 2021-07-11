+++
categories = ["programming", "tools"]
comments = true
published = true
date = "2010-12-12T19:42:11+08:00"
status = "publish"
tags = ["python"]
title = "pywin32访问access数据"
type = "post"
description = ""
+++

pywin32真强大，可以通过com接口做你想做的事。比如操作excel, access等等。下面是访问access数据库的例子：

```python
import os
import win32com.client

conn = win32com.client.Dispatch("ADODB.Connection")

# Either way works: one is the Jet OLEDB driver, the other is the
# Access ODBC driver.  OLEDB is probably better.

db = r"c:test.mdb"
DSN="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=" + db
#DSN="Driver={Microsoft Access Driver (*.mdb)};DBQ=" + db
conn.Open(DSN)

rs = win32com.client.Dispatch("ADODB.Recordset")
rs.Open( "[test]", conn, 1, 3 )

print rs.Fields.Count, " fields found:"
for x in range(rs.Fields.Count):
    print rs.Fields.Item(x).Name,
    print rs.Fields.Item(x).Type,
    print rs.Fields.Item(x).DefinedSize,
    print rs.Fields.Item(x).Value
```
<!--more-->