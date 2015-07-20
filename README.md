# go-bayeux-client

This package implements a client conditionally compliant with the [Bayeux Protocol](http://svn.cometd.org/trunk/bayeux/bayeux.html). It uses long-polling, not websockets. Messages are delivered on a channel provided by the user of the library, and can thus be buffered or unbuffered.

Example:

```go
import "github.com/andreas/go-bayeux-client"

func main() {
	// The second argument is a *http.Client, which defaults to http.DefaultClient.
	// This can be used to control timeouts, etc.
	client := bayeux.NewClient("https://foo.com", nil)

	// Messages will be delivered on this channel (use buffered chan if needed).
	msgs := make(bayeux.Chan)

	// Subscribe will automatically handshake with the server if not connected.
	if err := client.Subscribe("/bar/baz", msgs); err != nil {
		panic(err)
	}
	// Disconnect from server and stop background loop.
	defer client.Close()

	for msg := range msgs {
		fmt.Println("Received message: ", msg)
	}
}
```

# Limitations

- Does not support publishing.
- Only supports `long-polling` transport/connection type.
- Limited [advice](http://svn.cometd.org/trunk/bayeux/bayeux.html#toc_32) support.
