#

## client.go

```go
type Client struct {
	*clientv3.Client
	Kubernetes Interface
}

var _ Interface = (*Client)(nil)
```

## interface.go

```go
type Interface interface {

}
```