## Python 将Excel表格数据导入MySQL数据库

利用Python代码，将EXCEL表格数据导入到MySQL数据库中！话不多说，下面代码示例。

![Excel表格数据.jpg](https://raw.githubusercontent.com/bexonpak/markdown-pic/master/img/20190617230229.jpg)

**1示例代码：**

```python
import xlrd
import pymysql
#打开数据所在的工作簿，以及选择存有数据的工作表
book = xlrd.open_workbook("students.xls")
sheet = book.sheet_by_name("sheet1")
#建立一个MySQL连接
conn = pymysql.connect(
        host='localhost', 
        user='root', 
        passwd='python',  
        db='python',  
        port=3306,  
        charset='utf8'
        )
# 获得游标
cur = conn.cursor()
# 创建插入SQL语句
query = 'insert into student_tbl (name,sex,minzu,danwei_zhiwu,phone_number,home_number) values (%s, %s, %s, %s, %s, %s)'
# 创建一个for循环迭代读取xls文件每行数据的, 从第二行开始是要跳过标题行
for r in range(1, sheet.nrows):
      name      = sheet.cell(r,1).value
      sex       = sheet.cell(r,2).value
      minzu          = sheet.cell(r,3).value
      danwei_zhiwu     = sheet.cell(r,4).value
      phone_number       = sheet.cell(r,5).value
      home_number = sheet.cell(r,6).value
      values = (name,sex,minzu,danwei_zhiwu,phone_number,home_number)
      # 执行sql语句
      cur.execute(query, values)
cur.close()
conn.commit()
conn.close()
columns = str(sheet.ncols)
rows = str(sheet.nrows)
print ("导入 " +columns + " 列 " + rows + " 行数据到MySQL数据库!")
```

**2.导入效果：**

![python执行效果.jpg](https://raw.githubusercontent.com/bexonpak/markdown-pic/master/img/20190617230411.png)

![MySQL数据.jpg](https://raw.githubusercontent.com/bexonpak/markdown-pic/master/img/20190617230650.png)

**3.代码解析：**

这个Python脚本，用到了两个Python库，第一个是xlrd，这个库是用来操作Excel文件的，在上述代码中这个库的使用我都写了注释，可以看出来。它的使用还是比较方便的。第二个库就是pymysql，他的作用是链接MySQL数据库，在Python2.X中使用的是MySQLdb这个库链接数据库，但是MySQLdb不支持Python3.X，所以在Python3.X中用pymysql，作用都是一样的。
