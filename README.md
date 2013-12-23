# trident-kairos

Trident state implementation for [KairosDB](https://code.google.com/p/kairosdb/). It supports non-transactional, transactional, and opaque state types.

## Usage

Use static factory methods in `trident.cassandra.KairosState`.

Word count topology sample:

```java
StateFactory kairosStateFactory = KairosState.transactional("localhost");

TridentState wordCounts = topology.newStream("spout1", spout)
                                  .each(new Fields("sentence"), new Split(), new Fields("word"))
                                  .groupBy(new Fields("word"))
                                  .persistentAggregate(kairosStateFactory, new Count(), new Fields("count"))
                                  .parallelismHint(6);
```

KairosState.Options, that could be passed to factory methods:

```java
public static class Options<T> implements Serializable {
    public int localCacheSize = 5000;
    public String globalKey = "$__GLOBAL_KEY__$";
}
```

## License

Copyright (C) 2013 BrightTag, Inc.

Distributed under the Apache Public License v2.

(License is under discussions, it may be changed soon)
