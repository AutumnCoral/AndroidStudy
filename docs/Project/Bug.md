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

An exception occurred applying plugin request [id: 'com.android.application']
> Failed to apply plugin 'com.android.internal.application'.
> Android Gradle plugin requires Java 11 to run. You are currently using Java 1.8.
> You can try some of the following options:
>      - changing the IDE settings.
>           - changing the JAVA_HOME environment variable.
>           - changing `org.gradle.java.home` in `gradle.properties`.

原因，jdk版本太低

**"{\"buildingCategory\":\"96\",\"buildingType\":\"单元楼\",\"collecterId\":\"fa086ae0807349bcaa90d6e95589ae96\",\"collecterName\":\"admin\",\"collecterRegion\":\"达尔塘警务站\",\"communityName\":\"无名称建筑\",\"county\":\"巴青县\",\"houseNumber\":\"3\",\"id\":\"3406\",\"road\":\"测试道路\",\"status\":3,\"townShip\":\"达尔塘警务站管辖区\",\"typeOfBuildingStructure\":\"砖混\",\"unit\":\"\",\"wkt\":\"POLYGON ((94.05647507200007 31.92111856400004,94.05647060600006 31.920968945000027,94.05674332700005 31.92096280100003,94.05674779300006 31.921112421000032,94.05647507200007 31.92111856400004))\"}"**

问题：多转了一道字符串

3巴青

```
@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (data == null) return;
    if (requestCode == WatermarkCameraConfig.REQUEST_WATERMARK_CAMERA_IMAGE && resultCode == AffixManagement.IMAGE) {
        String videoImgUrl = data.getStringExtra("videoImgUrl");
        addPictureList(videoImgUrl, true);
    }
    if (requestCode == PersonInformationActivity.REQUEST_CODE_PERSON_INFORMATION && resultCode == PersonInformationActivity.RESULT_CODE_PERSON_INFORMATION) {
        PersonInformationBean newPersonInformationBean = (PersonInformationBean) data.getSerializableExtra("personInformationBean");
        L.e("newPersonInformationBean1" + newPersonInformationBean);
        if (newPersonInformationBean != null) {
            int from = data.getIntExtra("from", 0);
            PersonInformationBean oldPersonInformationBean = PersonInformationBean.findById(newPersonInformationBean.getId(), personList);
            if (from == 1) {
                if (oldPersonInformationBean == null) {
                    personList.add(newPersonInformationBean);
                    houseAdapter.notifyDataSetChanged();
                } else {
                    int index = personList.indexOf(oldPersonInformationBean);
                    personList.remove(oldPersonInformationBean);
                    personList.add(index, newPersonInformationBean);
                    houseAdapter.notifyDataSetChanged();
                }
            } else if (from == 2) {
                personList.remove(oldPersonInformationBean);
                houseAdapter.notifyDataSetChanged();
            }
        }
    }
}
```



## ERROR: MobSDK已停止支持非严格模式版本，请按上面编译告示接入合规版本！

 在/gradle.properties文件中加

```
MobSDK.spEdition=FP
```
