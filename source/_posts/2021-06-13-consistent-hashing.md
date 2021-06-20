---
layout: post
title:  "Design distributed Cache"
tags:
 - system design
---

## Requirements
Functional:
- put(key, value)
- get(key)

Non-Functional:
- Scalable
- highly available(survives hardware/network failures)
- highly performant(fast put/get)

## LRU Cache
The basic data structure for cache would be **hash table** since it provides O(1) time complexity for put/get operation. So far so good. 

One machine has finite memory capacity so we can't put keys endlessely. If the cache is full, we need to delete existing data so that we can add new one. Then which key should be evicted(deleted)? This is called [cache eviction(replacement) policy](https://en.wikipedia.org/wiki/Cache_replacement_policies). The most popular one is Least recently Used(LRU).

LRU discards the least recently used items first. So the algorithm needs to keep track of access time of the items. 

Order can be easily expressed by linked list and cache is basically hash table. Then we can combine these two into one data structure, called **Linked Hash Table.**. basic idea is whenever the key is accessed, we add the item into head of the list. Then the tail of the list will be always the one last accessed, which is the target item to be removed when capacity is full.

```java
class Node{
  private final String key;
  private String value;
  private Node prev, next;

  public Node(String key, String value){
    this.key=key;
    this.value=value;
  }
}

class LRUCache{
  Map<String, Node> map;
  int capacity;
  NOde head = null;
  Node tail = null;

  private deleteFromList(Node node){

  }

  private setListHead(Node node){

  }

  public put(String key, String value) {
    if(map.containsKey(key)){
      map.get(key).setValue(value);
      deleteFromList(node);
      setListHead(node);
    } else{
      if(map.size()>=capacity){
        map.remove(tail.getKey());
        deleteFromList(tail);
      }
      Node node = new Node(key,value);
      map.put(key, node);
      setListHead(node);
    }
  }

  public String get(String key){
    if(!map.containsKey(key)){
      return null;
    }
    Node node = map.get(key);
    deleteFromList(node);
    setListHead(node);
    return node.getValue();
  }
}
```
## 2 possible approaches 

### dedicated cache cluster
- isolation of resources between service and cache
- can be used by multiple services
- flexibility in choosing hardware
### co-located cache
cache service is co-located with the service host.
- No extra hardware and operation cost
- scales together with the service

## choosing a cache host
Definitely one cache host can't server all the items. They need to be split across multiple hosts by hash of key of the item. How keys can be distributed? 

### MOD
Cache host number = hash_function(key) MOD #hosts

mod operator would be the most easy one to implement but it has obvious weakness. entire cache need to be rewritten whenever a new host is added or deleted. Generally this kind of overhead is not acceptable in production environment.

### Consistent hashing
consistent hashing is basically placing host on a circle. 12 oclock will be value 0 or 2^32. the point will be mapped to the point on the circle boundary based on the hash value. for example 2^32/4 will be mapped to 3 o'clock. we will place cache host along the circle with even distance. the keys between the two hosts belong to the first clockwise host. 

```
before:
H1 --- H2 --- H3 --- H1

after:
H1 --- H2 --- H2.5 --- H3 --- H1
```

Let's say we added H2.5 between H2 and H3. then the affected hosts are H2 and only. very limited blast radius compared to the MOD approach.

## Cache client
cache client knows about all cache servers. all cache clients should have the same list of servers. It stores list of servers in sorted order. just like TreeMap in java. binary search is used to identify server, which is O(logn). It uses TCP or UDP protocol to talke to servers. If cache server is unavailable, client proceeds as if it was a cache miss. 

### maintaining a list of cache servers

- use configuration file in local host
- use external file like S3
- use configuration service like ZooKeeper
  - config service will check heartbeat with all the cache hosts. if it fails it will be deleted from the cache list


## high availability
hot shard problem can be solved by replication.

there are two categories of data replication protocols.
- a set of probablistic protocol -> eventual consistency
  - gossip
  - epdemic broadcast
  - trees
  - bimodal multicast
- consensus protocol -> strong consistency
  - 2 or 3 phase commit
  - paxos
  - raft
  - chain replication

master-slave replication

## what else?
- consistency. The consistency issues can happen. consistency can be achieved by performing synchronous replication. but this will introduce additional latency and overall complexity of the system. so it's heavily depends on the service requireemnts.
- data expiration. The data can be expired after some time later depends on business requiremetns. If that's the case we can schedule a batch job to remove expired keys. Or passively delete expired cache on a regular basis.

- Local cache can be used on the clinet library side. LRU cache or Guava cache can be used.
- Security.
  - firewall
  - encrypt the data
- monitoring and loggin
  - number of errors, latency, cache hit/miss, cpu, memory
- cache client
  - maintain a list of cache servers
  - pick a shard
  - remote call
  - delegate many responsibilities to proxy, ref twemproxy project
  or make cache servers responsible for picking a shard
- consistent hasing has 2 flaws. domino effect and not split the circle evenly-> add server on the circle multiple times


## Reference
- https://youtu.be/iuqZvajTOyA
- TCP vs UDP, https://www.geeksforgeeks.org/differences-between-tcp-and-udp/




