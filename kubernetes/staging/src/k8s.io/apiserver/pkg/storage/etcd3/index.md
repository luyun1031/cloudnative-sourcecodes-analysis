#

## store.go

[storage.Interface](/kubernetes/staging/src/k8s.io/apiserver/pkg/storage/index.md)

```go
type store struct {
    ...
}

func New(...) storage.Interface {
    ...
	var store storage.Interface
    store = newStore(...)
    ...
	return store
}

func newStore(...) *store {
	...
    s := &store{
        ...
	}
    ...
	return s
}
```