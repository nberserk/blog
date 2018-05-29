---
layout: post
title: merge sequence file in spark
tags: spark
date: 2016-1-8
---

## merging sequence files
spark로 파일을 저장하면 sequence 파일로 저장이 되는데, 이 파일을 합쳐서 하나의 파일로 만들고 싶을때 아래 scala function을 사용하면 된다.


```java
def merge(srcPath: String, dstPath: String): Unit =  {
    import org.apache.hadoop.fs.{FileSystem, FileUtil, Path}
    import org.apache.hadoop.hdfs.HdfsConfiguration
    import org.apache.hadoop.conf.Configuration
    import java.net.URI

    val config = new Configuration()
    //config.set("fs.s3.impl", "org.apache.hadoop.fs.s3.S3FileSystem")
    val fs = FileSystem.get( URI.create(srcPath), config)
    FileUtil.copyMerge(fs, new Path(srcPath), fs, new Path(dstPath), false, config, null)
  }
```

## revision history
- 2016/1/11 initial draft

