---
layout: article
title:  "Python read and write excel file"
rootCate: "work"
show_edit_on_github: false
pageview: true
comment: true
key: work_python_read_write_excel_file_2019-0617
categories:
- Python
tags:
- work
- Python3
---

总结 Python 中 读取和写入 Excel/CSV 文件的方法。

<!---more--->

## Preperation
通过 `pip install pandas` 安装 `pandas` library 。

## CSV
```
import csv
with open('FILE_NAME.csv') as f:
    f_csv = csv.reader(f)
    headers = next(f_csv)
#     print(line)
    data = []
    for row in f_csv:
        data.append(row[0])  # 把第一列的值放入data
```

## Excel file
#### Read from excel file using pandas

```
import pandas as pd
from pandas import ExcelWriter
from pandas import ExcelFile

df = pd.read_excel('../文档.xlsx', sheetname='Sheet1')

print("Column headings:")
print(df.columns[0])  # 列名

data = []
for x in df["COLUMN_NAME"]:
    numbers.append(x)
```

#### Write into excel file using pandas
```
from pandas import DataFrame

Cars = {'Brand': ['Honda Civic','Toyota Corolla','Ford Focus','Audi A4'],
        'Price': [32000,35000,37000,45000]
        }

df = DataFrame(Cars, columns= ['Brand', 'Price'])

export_excel = df.to_excel (r'./export_dataframe.xlsx', index = None, header=True) #Don't forget to add '.xlsx' at the end of the path

print (df)
```

