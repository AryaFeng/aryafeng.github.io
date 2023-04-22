# tensorlog介绍
![image](https://user-images.githubusercontent.com/69587189/233777939-5d5181b5-f878-407d-a400-c0bcadbacf43.png)
# 简单代码demo
```python
<pre><code class="language-python"><span class="hljs-comment"># 导入tensorlog模块</span>
<span class="hljs-keyword">import</span> tensorlog

<span class="hljs-comment"># 定义一个知识库</span>
db = tensorlog.parseDBSpec(<span class="hljs-string">'''
HasOfficeInCity(New York, Uber)
CityInCountry(USA, New York)
HasOfficeInCity(Beijing, Baidu)
CityInCountry(China, Beijing)
'''</span>)

<span class="hljs-comment"># 定义一个逻辑规则</span>
prog = tensorlog.parseProgSpec(<span class="hljs-string">'''
HasOfficeInCountry(Y,X) :- HasOfficeInCity(Z,X), CityInCountry(Y,Z)
'''</span>)

<span class="hljs-comment"># 定义一个查询</span>
query = tensorlog.parseTerm(<span class="hljs-string">'HasOfficeInCountry(USA,X)'</span>)

<span class="hljs-comment"># 创建一个tensorlog对象</span>
tlog = tensorlog.TensorLog()

<span class="hljs-comment"># 编译逻辑规则和查询</span>
fun = tlog.<span class="hljs-built_in">compile</span>(prog.db,prog.rules,query)

<span class="hljs-comment"># 执行推理并输出结果</span>
result = fun.<span class="hljs-built_in">eval</span>()
<span class="hljs-built_in">print</span>(result)
</code></pre>
```
