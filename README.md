# kcache
DSL cache  instead of Spring annotation cache, back end caffeine

## compile & install
```
mvn package install
```

## add dependency
in your pom.xml
```
<dependency>
    <groupId>com.github.rwsbillyang</groupId>
    <artifactId>kcache</artifactId>
    <version>1.0.0</version>
</dependency>
```

or build.gradle:
```
 compile 'com.github.rwsbillyang:kcache:1.0.0'
```

## usage
```kotlin
private val caffeineCache: CaffeineCache by inject()

...

fun findAnyLink(id: ObjectId) = Cacheable<AnyLink> {
        cache = caffeineCache
        key ="anyLink/${id.toHexString()}"
        call { runBlocking { anyLinkCol.findOneById(id) } }
    }
    
 fun updateAnyLink(bean: AnyLinkBean) = CacheEvict<AnyLink> {
        cache = caffeineCache
        key ="anyLink/${bean.id}"
        call {
            runBlocking {
                anyLinkCol.updateOneById(ObjectId(bean.id),set(
                        SetTo(AnyLink::title, bean.title),
                        SetTo(AnyLink::brief,bean.brief),
                        SetTo(AnyLink::img,bean.img)))
            }
        }
    }   
```
