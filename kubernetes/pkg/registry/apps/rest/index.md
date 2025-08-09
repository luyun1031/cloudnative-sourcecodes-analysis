#

## storage_apps.go

[rest.Storage](/kubernetes/staging/src/k8s.io/apiserver/pkg/registry/rest/index.md)
[apiserver.RESTStorageProvider](/kubernetes/pkg/controlplane/apiserver/index.md)
```go
// 实现了apiserver.RESTStorageProvider
type StorageProvider struct{}

func (p StorageProvider) NewRESTStorage(...) (genericapiserver.APIGroupInfo, error) {
	apiGroupInfo := genericapiserver.NewDefaultAPIGroupInfo(apps.GroupName, legacyscheme.Scheme, legacyscheme.ParameterCodec, legacyscheme.Codecs)
	// If you add a version here, be sure to add an entry in `k8s.io/kubernetes/cmd/kube-apiserver/app/aggregator.go with specific priorities.
	// TODO refactor the plumbing to provide the information in the APIGroupInfo

	if storageMap, err := p.v1Storage(apiResourceConfigSource, restOptionsGetter); err != nil {
		return genericapiserver.APIGroupInfo{}, err
	} else if len(storageMap) > 0 {
		apiGroupInfo.VersionedResourcesStorageMap[appsapiv1.SchemeGroupVersion.Version] = storageMap
	}

	return apiGroupInfo, nil
}

func (p StorageProvider) v1Storage(apiResourceConfigSource serverstorage.APIResourceConfigSource, restOptionsGetter generic.RESTOptionsGetter) (map[string]rest.Storage, error) {
	storage := map[string]rest.Storage{}
    ...
	return storage, nil
}

// GroupName returns name of the group
func (p StorageProvider) GroupName() string {
	return apps.GroupName
}
```