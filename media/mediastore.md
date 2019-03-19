## 视频抽取截图

知道视频地址 获取视频第一帧 单个可以 但是在视频列表会非常卡 不建议使用
```java
 try {
        MediaMetadataRetriever media = new MediaMetadataRetriever();
        media.setDataSource("http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4",new HashMap<String, String>());
        Bitmap bitmap  = media.getFrameAtTime(1, MediaMetadataRetriever.OPTION_CLOSEST_SYNC );
        imageView.setImageBitmap(bitmap);//对应的ImageView
    }catch (IllegalArgumentException ex){
        ex.printStackTrace();
    }
```

---

查询本地所有视频
```java
 Cursor c = null;
    try {
        c = mContext.getContentResolver().query(MediaStore.Video.Media.EXTERNAL_CONTENT_URI,
                null, null, null, MediaStore.Video.Media.DEFAULT_SORT_ORDER);
        while (c.moveToNext()) {
            String path = c.getString(c.getColumnIndexOrThrow(MediaStore.Video.Media.DATA));// 路径
            int id = c.getInt(c.getColumnIndexOrThrow(MediaStore.Video.Media._ID));// 大小
            String name = c.getString(c.getColumnIndexOrThrow(MediaStore.Video.Media.DISPLAY_NAME)); // 歌曲名
            String resolution = c.getString(c.getColumnIndexOrThrow(MediaStore.Video.Media.RESOLUTION)); //
            long size = c.getLong(c.getColumnIndexOrThrow(MediaStore.Video.Media.SIZE));// 大小
            long duration = c.getLong(c.getColumnIndexOrThrow(MediaStore.Video.Media.DURATION));// 时间
            long date = c.getLong(c.getColumnIndexOrThrow(MediaStore.Video.Media.DATE_MODIFIED));
        }
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        if (c != null) {
            c.close();
        }
    }
```
---
```java
知道视频对应的id 获取缩略图地址
   public static String[] thumbColumns = {MediaStore.Video.Thumbnails.DATA};

public static String getThumbnailPathForLocalFile(Context context, long fileId) {

    MediaStore.Video.Thumbnails.getThumbnail(context.getContentResolver(),
            fileId, MediaStore.Video.Thumbnails.MICRO_KIND, null);

    Cursor thumbCursor = null;
    try {

        thumbCursor = context.getContentResolver().query(
                MediaStore.Video.Thumbnails.EXTERNAL_CONTENT_URI,
                thumbColumns, MediaStore.Video.Thumbnails.VIDEO_ID + " = "
                        + fileId, null, null);

        if (thumbCursor.moveToFirst()) {
            String thumbPath = thumbCursor.getString(thumbCursor
                    .getColumnIndex(MediaStore.Video.Thumbnails.DATA));

            return thumbPath;
        }

    } finally {
        thumbCursor.close();
    }

    return null;
}
```