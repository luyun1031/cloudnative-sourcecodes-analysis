#

## dryrun.go

[storage.Interface](/kubernetes/staging/src/k8s.io/apiserver/pkg/storage/index.md)

```go
type DryRunnableStorage struct {
	Storage storage.Interface
	Codec   runtime.Codec
}
```

## store.go

```go
type Store struct {
    ...
    Storage DryRunnableStorage
    ...
}

func (e *Store) CompleteWithOptions(options *generic.StoreOptions) error {
    ...
    opts, err := options.RESTOptions.GetRESTOptions(e.DefaultQualifiedResource, e.NewFunc())
    ...
    if e.Storage.Storage == nil {
        ...
		e.Storage.Storage, e.DestroyFunc, err = opts.Decorator(
            ...
		)
		...
	}
    ...
}
```
