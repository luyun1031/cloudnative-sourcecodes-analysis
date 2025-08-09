#

## delegator.go

[storage.Interface](/kubernetes/staging/src/k8s.io/apiserver/pkg/storage/index.md)

```go
type CacheDelegator struct {
    ...
}

var _ storage.Interface = (*CacheDelegator)(nil)
```