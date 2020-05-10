# 反序列化
json_object = json.loads(json_str)

#序列化
json.dumps

#json对象，json， json字符串


json 可以看成和js平级的语言，实现ecmascript

json有自己的数据类型，和js相似


JSON.parse() 和 JSON.stringify()
parse用于从一个字符串中解析出json对象,如
var str = '{"name":"huangxiaojian","age":"23"}'
结果：
JSON.parse(str)
Object
1. age: "23"
2. name: "huangxiaojian"
3. __proto__: Object
注意：单引号写在{}外，每个属性名都必须用双引号，否则会抛出异常。
stringify()用于从一个对象解析出字符串，如
var a = {a:1,b:2}
结果：
JSON.stringify(a)
"{"a":1,"b":2}"