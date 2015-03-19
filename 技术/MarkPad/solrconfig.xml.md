# Solr solrconfig.xml configuration

Lucene version
```
<luceneMatchVersion>LUCENE_36</luceneMatchVersion>
```

配置SOLR的依赖包，可以设置多个。依赖包的配置有多种方式，具体可参考配置文件的说明。
```
<lib dir="../../dist" regex=".*\.jar" />
```

数据存储目录
```
<dataDir>/var/lib/solr/data</dataDir>
```

写锁超时时间，默认为1000ms；优化建议为:20000ms
```
<writeLockTimeout>1000</writeLockTimeout>
```

是否使用复合文件，集合这个功能将会很少使用index的文件数量，使用很少的文件描述器会以性能的降低为代价。Lucene默认是true，Solr默认是false.
```
<useCompoundFile>false</useCompoundFile>
```

合并策略。在Lucene中，合并策略控制着片段是如何合并
```
<mergePolicy class="org.apache.lucene.index.TieredMergePolicy">
  <int name="maxMergeAtOnce">10</int>
  <int name="segmentsPerTier">10</int>
</mergePolicy>
```

合并因子控制着多少个分片将会一次性合并。对于层叠合并策略（TieredMergePolicy）.合并因子一个使得的参数，它将一次性设置MaxMergeAtOnce和SegmentsPerTier的值。对于日志字节大小合并策略，合并因子决定了多个片段合并到一个片段前允许有多少个片段。默认这两种策略的默认值为10.
```
<mergeFactor>10</mergeFactor>
```

指定合并调度程序，在Lucene中，合并调度程序控制着如何合并分片的实现。
```
<mergeScheduler class="org.apache.lucene.index.ConcurrentMergeScheduler"/>
```

锁工厂。这个选择指定哪个Lucene工厂实现将会使用。1）single=SingleInstanceLockFactory，只读不能修改的索引文件。2）native=NativeFSLockFactory。使用OS本地文件锁。注意，多个SolrWeb应用在相同的JVM上而尝试去分享单一的索引文件是，不适用这个锁工厂。3）simple=SimpleFSLockFactory，使用普通的文件锁。
```
<lockType>native</lockType>
```

在启动时是否加锁。如果使用single锁工厂，这个锁项就没有必要了。默认为false.
```
<unlockOnStartup>false</unlockOnStartup>
```


## 配置注意事项

`mergeFactor` 合并因子
