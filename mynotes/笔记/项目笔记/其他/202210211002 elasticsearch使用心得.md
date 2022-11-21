## es搜索"-"字符

设置增加char_filter，将"-"替换成"_"

增加analyzer，使用这个char_filter

针对字段设置analyzer和search_analyzer

搜索时搜索该字段的keywords

## 已使用场景1

抽取腾讯教育邮箱全部用户信息，导入到es中。

使用体验与普通数据较类似，没有发挥es的分词等优势，反因es的分词符而受阻。