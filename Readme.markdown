# Cache

A generic caching library for Swift. Cache depends on Foundation.


## Usage

Cache provides a simple `Cache` protocol:

```swift
protocol Cache {
  associatedtype Element

  func get(key key: String, completion: (Element? -> Void))
  func set(key key: String, value: Element, completion: (() -> Void)?)
  func remove(key key: String, completion: (() -> Void)?)
  func removeAll(completion completion: (() -> Void)?)
}
```

There are two (well actually three, but we'll get there) provided caches:

* `MemoryCache` — Backed by an `NSCache`
* `DiskCache` — Backed by files on disk using `NSFileManager`

Using both of these is exactly the same interface.

## MultiCache

There is a third cache provided called `MultiCache`. This combines caches.

```swift
let cache = MultiCache(caches: [memoryCache, diskCache])
cache.set(key: "foo", value: "bar")
```

This will write to all caches in parallel. When accessing the multi cache, it will go in order. In this example, it will try the memory cache and if there's a miss it will try the disk cache. If it were to find it in the disk cache, it will write it to all previous caches for faster future reads.


## Thanks

Thanks to [Caleb Davenport](https://twitter.com/calebd) for unintentioanlly inspiring this and lots of help along the way.