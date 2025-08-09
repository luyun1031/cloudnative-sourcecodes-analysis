#

## get.go

这里直接使用go原生的net/http，可能也是为了和go-restful解耦，以后可能换其他以net/http为底座的web框架

```go
type getterFunc func(ctx context.Context, name string, req *http.Request) (runtime.Object, error)

func getResourceHandler(scope *RequestScope, getter getterFunc) http.HandlerFunc {
	return func(w http.ResponseWriter, req *http.Request) {
        ...
	}
}

// GetResource returns a function that handles retrieving a single resource from a rest.Storage object.
func GetResource(r rest.Getter, scope *RequestScope) http.HandlerFunc {
	return getResourceHandler(scope,
		func(ctx context.Context, name string, req *http.Request) (runtime.Object, error) {
            ...
			return r.Get(ctx, name, &options)
		})
}

// GetResourceWithOptions returns a function that handles retrieving a single resource from a rest.Storage object.
func GetResourceWithOptions(r rest.GetterWithOptions, scope *RequestScope, isSubresource bool) http.HandlerFunc {
	return getResourceHandler(scope,
		func(ctx context.Context, name string, req *http.Request) (runtime.Object, error) {
            ...
			return r.Get(ctx, name, opts)
		})
}
```