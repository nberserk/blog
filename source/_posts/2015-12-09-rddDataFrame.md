---
layout: post
title: RDD and DataFrame
tags: spark
date: 2015-12-09
---


앞서 저장 용량과 분석 속도를 위해서 parquet을 [도입했다는 얘기](http://nberserk.github.io/2015/12/09/parquet/)를 했는데, parquet 으로 로드를 하면 DataFrame으로 로딩이 된다. DataFrame은 RDD + Schema로 볼 수 있겠다.

DataFraem의 경우 쿼리에는 최적화 되어 있지만(읽기 전용), 변환을 하고자 한다면 아무래도 rdd가 더 편한것이 사실이다. 이것을 변환을 하려면 RDD로 변경을 하고, 다시 DataFrame으로 변경을 해야 한다. 이것에 관련된 것들을 알아보자.

## DataFrame <-> RDD
DataFrame을 뭔가 변환을 하려면, rdd로 변환후 하는게 편한데 변환은 아주 쉽다. DataFrame은 rdd라는 함수를 가지고 있으니 그것만 불러주면 된다.

RDD를 DataFrame으로 변환하는 방법은 크게 2가지,
- case class를 사용하거나
- 스키마를 사용하거나

아래 예제 참고

```scala
  // from dataframe to rdd
  val df = xxx
  val rdd = df.rdd

  // create DataFrame from RDD[Row]
  val schema = StructType(
    StructField("name", StringType, false) ::
      StructField("age", IntegerType, true) :: Nil)
  val df1 = sqlContext.createDataFrame(rdd, schema)

  // create DataFrame from RDD[User] using case class
  import sqlContext.implicits._
  case class User(name:String, age:Int)
  val rddUser = sc.textFile("xx").map(l=> User(l.split(",")(0), l.split(",")(1)))
  val df2 = rddUser.toDF()
```

## Row and schema
DataFrame.rdd를 하면 RDD[Row]가 리턴되는데, Row는 getLong(i),getString(i) ... 등의 함수를 가지고 있다. 그래서 타입은 DataFrame의 스키마로 알수가 있고 컬럼 명을 알면 값을 구할 수 있다.

```scala
val rdd = df.rdd
val idxName = df.schema.fieldIndex("name")
val idxAge = df.schema.fieldIndex("age")

// age만 남게 변환
rdd.map(r => _.getInt(idxAge))
```

## revision history
- 2015/12/17 initial draft

