---
title: Process Wiki Dump in Spark 2.3.0 
layout: post
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

In Spark 2.3.0, the ml Feature Extractor classes have fewer parameters than the counterpart classes in scikit-learn and would produce `Dataset` only for ml Machine Learning classes. If we want to extract the vectors inside the `Dataset` for other use we'd have to start from the scratch; however it's much easier with the vocabulary the `CountVectorizer` computed. 

```scala
  /** Reads Wiki dump, outputs bag-of-words RDD and vocabulary
    *
    * @param spark          SparkSession
    * @param dumpUri        URI of the Wiki Pages Articles dump file
    * @param maxFeatures    Maximum number of features
    * @return               RDD of bag-of-words and the vocabulary
    */
  def readWikiPagesArticlesDump(spark: SparkSession,
                                dumpUri: String,
                                maxFeatures: Int)
  : (RDD[(Long, (Int, Double))], Array[String]) = {
    // As of v2.3.0, the spark ml Vector cannot be
    // extracted for missing Encoder class, we thus proceed by
    //
    // 1. Compute the vocabulary with the spark ml classes
    // 2. Process the document texts again with the vocabulary
    //    and map the tokens into their indices to get the RDD
    //    bag-of-words

    import org.apache.spark.sql.Row
    import org.apache.spark.ml.feature.{RegexTokenizer,
      StopWordsRemover, CountVectorizer}

    val df = spark.read
      .format("com.databricks.spark.xml")
      .option("rowTag", "revision")
      .load(dumpUri)
      .filter("text._VALUE is not null")

    val regexTokenized = new RegexTokenizer()
      .setInputCol("text_value")
      .setOutputCol("words")
      .setToLowercase(true)
      .setPattern("\\W+")
      .transform(df.select(df.col("text._VALUE").as( "text_value")))

    val stopWordsRemoved = new StopWordsRemover()
      .setInputCol("words")
      .setOutputCol("stop_words_removed")
      .transform(regexTokenized.select("words"))

    val wikiVectorizerModel = new CountVectorizer()
      .setInputCol("stop_words_removed")
      .setOutputCol("features")
      .setVocabSize(maxFeatures)
      .setMinDF(1)
      .fit(stopWordsRemoved)

    val vocab = wikiVectorizerModel.vocabulary
    val vocabToIndexMap = vocab.zipWithIndex
      .map {
        case (w, wid) => (w, wid.toInt)
      }
      .toMap

    val bow = df.select("text._VALUE")
      .rdd
      .map {
        case Row(documentText: String) =>
          documentText
            .toLowerCase()
            .split("\\W+")
            .collect {
              case w if vocabToIndexMap contains w => vocabToIndexMap(w)
            }
      }
      .zipWithIndex
      .flatMap {
        case (wids, docid) =>
          wids.map {
            case wid => ((docid, wid), 1.0)
          }
      }
      .reduceByKey(_ + _)
      .map {
        case ((docid, wid), c) => (docid, (wid, c))
      }

    (bow, vocab)
  }
```
