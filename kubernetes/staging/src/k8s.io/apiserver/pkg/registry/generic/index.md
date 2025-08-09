# 

## options.go

```go
type RESTOptionsGetter interface {
	GetRESTOptions(resource schema.GroupResource, example runtime.Object) (RESTOptions, error)
}

type StoreOptions struct {
	RESTOptions RESTOptionsGetter
	TriggerFunc storage.IndexerFuncs
	AttrFunc    storage.AttrFunc
	Indexers    *cache.Indexers
}
```

## storage_decorator.go

[storage.Interface](/kubernetes/staging/src/k8s.io/apiserver/pkg/storage/index.md)  
[factory.Create](/kubernetes/staging/src/k8s.io/apiserver/pkg/storage/storagebackend/factory/index.md)
```go
// 这种签名的函数返回一个storage.Interface实现等
type StorageDecorator func(
    config *storagebackend.ConfigForResource,
	resourcePrefix string,
	keyFunc func(obj runtime.Object) (string, error),
	newFunc func() runtime.Object,
	newListFunc func() runtime.Object,
	getAttrsFunc storage.AttrFunc,
	trigger storage.IndexerFuncs,
	indexers *cache.Indexers
) (storage.Interface, factory.DestroyFunc, error)

// 返回不带缓存的storage.Interface实现
/* 符合上面的StorageDecorator函数签名 */
func UndecoratedStorage(
    config *storagebackend.ConfigForResource,
	resourcePrefix string,
	keyFunc func(obj runtime.Object) (string, error),
	newFunc func() runtime.Object,
	newListFunc func() runtime.Object,
	getAttrsFunc storage.AttrFunc,
	trigger storage.IndexerFuncs,
	indexers *cache.Indexers
) (storage.Interface, factory.DestroyFunc, error) {
    return NewRawStorage(config, newFunc, newListFunc, resourcePrefix)
}

// TODO: 当cacher适用于所有registry（事件除外），这个方法会被移除
/* 没想通为什么，先放放 */
func NewRawStorage(
    config *storagebackend.ConfigForResource,
	newFunc func() runtime.Object,
	newListFunc func() runtime.Object,
    resourcePrefix string
) (storage.Interface, factory.DestroyFunc, error) {
    return factory.Create(*config, newFunc, newListFunc, resourcePrefix)
}
```
