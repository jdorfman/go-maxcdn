# go-maxcdn

MaxCDN Golang API.

[![GoDoc](https://godoc.org/github.com/MaxCDN/go-maxcdn?status.png)](https://godoc.org/github.com/MaxCDN/go-maxcdn) [![Build Status](https://travis-ci.org/MaxCDN/go-maxcdn.svg)](https://travis-ci.org/MaxCDN/go-maxcdn)

## [API Documentation](http://godoc.org/github.com/MaxCDN/go-maxcdn)

```go
import "github.com/MaxCDN/go-maxcdn"
```

Package maxcdn is the golang bindings for MaxCDN's REST API.

Developer Notes:

- Custom types can be (somewhat) easily generated by using `maxcurl` (see:
https://github.com/MaxCDN/maxcdn-tools/tree/master/maxcurl) to fetch the
raw json output and the `json2struct` tool at http://mervine.net/json2struct to
generate a sample struct. In the resulting struct, I recommend changing a
`float64` types to `int` types and replacing any resulting `interface{}` types
with `string` types.

## [Documentation](http://godoc.org/github.com/MaxCDN/go-maxcdn)

```go
	// Basic Get
	max := maxcdn.NewMaxCDN(alias, token, secret)
	var got maxcdn.Generic
	res, err := max.Get(&got, "/account.json", nil)
	if err != nil {
		panic(err)
	}

	fmt.Printf("code: %d\n", res.Code)
	fmt.Printf("name: %s\n", got["name"].(string))

	// Basic Put
	form := url.Values{}
	form.Set("name", "new name")

	var put maxcdn.Generic
	if _, err = max.Put(&put, "/account.json", form); err == nil &&
		put["name"].(string) == "new name" {
		fmt.Println("name successfully updated")
	}

	// Basic Delete
	if _, err = max.Delete("/zones/pull.json/123456", nil); err == nil {
		fmt.Println("zone successfully deleted")
	}

    // Logs
    if logs, err := max.GetLogs(nil); err == nil {
        for _, line := range logs.Records {
            fmt.Println("%+v\n", line)
        }
    }
```
