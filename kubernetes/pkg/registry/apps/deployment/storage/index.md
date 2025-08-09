#

## storage.go

[rest.Storage](/kubernetes/staging/src/k8s.io/apiserver/pkg/registry/rest/index.md) 
[genericregistry.Store](/kubernetes/staging/src/k8s.io/apiserver/pkg/registry/generic/registry/index.md)

```go
type DeploymentStorage struct {
	Deployment *REST
	Status     *StatusREST
	Scale      *ScaleREST
	Rollback   *RollbackREST
}

type REST struct {
	*genericregistry.Store
}

type StatusREST struct {
	store *genericregistry.Store
}

type RollbackREST struct {
	store *genericregistry.Store
}

type ScaleREST struct {
	store *genericregistry.Store
}
```