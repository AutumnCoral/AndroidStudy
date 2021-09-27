[参考](https://blog.csdn.net/qq_15975081/article/details/53391640)

1，接收数据，实体类不确定的时候要用map来接收

java动态解析

Map<String, Object> map = new HashMap<>();
JSONObject jsonObject = new JSONObject();
Iterator<String> it = jsonObject.keys();
while (it.hasNext()) {
    String key = it.next();
    Object value = jsonObject.get(key);
    map.put(key, value);
}

# com.yl.clean

1,![错误1--APP报错](../media/pictures/Bug.assets/错误1--APP报错.png)

分析：1，没有找到启动类，在Mainfest
