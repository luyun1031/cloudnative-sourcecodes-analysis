#

## apis.go

```go
type RESTStorageProvider interface {
	GroupName() string
	NewRESTStorage(apiResourceConfigSource serverstorage.APIResourceConfigSource, restOptionsGetter generic.RESTOptionsGetter) (genericapiserver.APIGroupInfo, error)
}

func (s *Server) InstallAPIs(restStorageProviders ...RESTStorageProvider) error {
    nonLegacy := []*genericapiserver.APIGroupInfo{}
    ...
    for _, restStorageBuilder := range restStorageProviders {
        groupName := restStorageBuilder.GroupName()
        apiGroupInfo, err := restStorageBuilder.NewRESTStorage(s.APIResourceConfigSource, s.RESTOptionsGetter)
        ...
        if len(groupName) == 0 {
			// the legacy group for core APIs is special that it is installed into /api via this special install method.
			if err := s.GenericAPIServer.InstallLegacyAPIGroup(genericapiserver.DefaultLegacyAPIPrefix, &apiGroupInfo); err != nil {
				return fmt.Errorf("error in registering legacy API: %w", err)
			}
		} else {
			// everything else goes to /apis
			nonLegacy = append(nonLegacy, &apiGroupInfo)
		}
    }

    if err := s.GenericAPIServer.InstallAPIGroups(nonLegacy...); err != nil {
		return fmt.Errorf("error in registering group versions: %w", err)
	}
	return nil
}
```

## server.go

```go
type Server struct {
	GenericAPIServer *genericapiserver.GenericAPIServer

	APIResourceConfigSource   serverstorage.APIResourceConfigSource
	RESTOptionsGetter         genericregistry.RESTOptionsGetter
	ClusterAuthenticationInfo clusterauthenticationtrust.ClusterAuthenticationInfo
	VersionedInformers        clientgoinformers.SharedInformerFactory
}
```