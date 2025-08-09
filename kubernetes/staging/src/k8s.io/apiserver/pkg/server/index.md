#

## genericapiserver.go

```go
type APIGroupInfo struct {
    ...
	VersionedResourcesStorageMap map[string]map[string]rest.Storage
    ...
}

type GenericAPIServer struct {
    ...
}

func (s *GenericAPIServer) installAPIResources(apiPrefix string, apiGroupInfo *APIGroupInfo, typeConverter managedfields.TypeConverter) error {
    ...
    for _, groupVersion := range apiGroupInfo.PrioritizedVersions {
        ...
        apiGroupVersion, err := s.getAPIGroupVersion(apiGroupInfo, groupVersion, apiPrefix)
        ...
        discoveryAPIResources, r, err := apiGroupVersion.InstallREST(s.Handler.GoRestfulContainer)
        ...
    }
    ...
    return nil
}

func (s *GenericAPIServer) InstallLegacyAPIGroup(apiPrefix string, apiGroupInfo *APIGroupInfo) error {
    ...
	if err := s.installAPIResources(apiPrefix, apiGroupInfo, openAPIModels); err != nil {
		return err
	}
    ...
	return nil
}

func (s *GenericAPIServer) InstallAPIGroups(apiGroupInfos ...*APIGroupInfo) error {
    ...
	for _, apiGroupInfo := range apiGroupInfos {
		if err := s.installAPIResources(APIGroupPrefix, apiGroupInfo, openAPIModels); err != nil {
			return fmt.Errorf("unable to install api resources: %v", err)
		}
        ...
	}
	return nil
}

func (s *GenericAPIServer) getAPIGroupVersion(apiGroupInfo *APIGroupInfo, groupVersion schema.GroupVersion, apiPrefix string) (*genericapi.APIGroupVersion, error) {
    ...
	version := s.newAPIGroupVersion(apiGroupInfo, groupVersion)
    ...
	return version, nil
}

func (s *GenericAPIServer) newAPIGroupVersion(apiGroupInfo *APIGroupInfo, groupVersion schema.GroupVersion) *genericapi.APIGroupVersion {

	allServedVersionsByResource := map[string][]string{}
	for version, resourcesInVersion := range apiGroupInfo.VersionedResourcesStorageMap {
		for resource := range resourcesInVersion {
			if len(groupVersion.Group) == 0 {
				allServedVersionsByResource[resource] = append(allServedVersionsByResource[resource], version)
			} else {
				allServedVersionsByResource[resource] = append(allServedVersionsByResource[resource], fmt.Sprintf("%s/%s", groupVersion.Group, version))
			}
		}
	}

	return &genericapi.APIGroupVersion{
		GroupVersion:                groupVersion,
		AllServedVersionsByResource: allServedVersionsByResource,
        ...
	}
}

func NewDefaultAPIGroupInfo(group string, scheme *runtime.Scheme, parameterCodec runtime.ParameterCodec, codecs serializer.CodecFactory) APIGroupInfo {
    ...
	return APIGroupInfo{
		...
		VersionedResourcesStorageMap: map[string]map[string]rest.Storage{},
        ...
	}
}
```