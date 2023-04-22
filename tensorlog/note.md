# tensorlog介绍
![image](https://user-images.githubusercontent.com/69587189/233777939-5d5181b5-f878-407d-a400-c0bcadbacf43.png)
# 简单代码demo
```python3
# 导入tensorlog模块
import tensorlog

# 定义一个知识库
db = tensorlog.parseDBSpec('''
HasOfficeInCity(New York, Uber)
CityInCountry(USA, New York)
HasOfficeInCity(Beijing, Baidu)
CityInCountry(China, Beijing)
''')

# 定义一个逻辑规则
prog = tensorlog.parseProgSpec('''
HasOfficeInCountry(Y,X) :- HasOfficeInCity(Z,X), CityInCountry(Y,Z)
''')

# 定义一个查询
query = tensorlog.parseTerm('HasOfficeInCountry(USA,X)')

# 创建一个tensorlog对象
tlog = tensorlog.TensorLog()

# 编译逻辑规则和查询
fun = tlog.compile(prog.db,prog.rules,query)

# 执行推理并输出结果
result = fun.eval()
print(result)
# 结果{'Uber':1.0}
```
