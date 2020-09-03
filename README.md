你好！
很冒昧用这样的方式来和你沟通，如有打扰请忽略我的提交哈。我是光年实验室（gnlab.com）的HR，在招Golang开发工程师，我们是一个技术型团队，技术氛围非常好。全职和兼职都可以，不过最好是全职，工作地点杭州。
我们公司是做流量增长的，Golang负责开发SAAS平台的应用，我们做的很多应用是全新的，工作非常有挑战也很有意思，是国内很多大厂的顾问。
如果有兴趣的话加我微信：13515810775  ，也可以访问 https://gnlab.com/，联系客服转发给HR。
# graphql [![Build Status](https://travis-ci.org/graphql-go/graphql.svg)](https://travis-ci.org/graphql-go/graphql) [![GoDoc](https://godoc.org/graphql.co/graphql?status.svg)](https://godoc.org/github.com/graphql-go/graphql) [![Coverage Status](https://coveralls.io/repos/graphql-go/graphql/badge.svg?branch=master&service=github)](https://coveralls.io/github/graphql-go/graphql?branch=master) [![Join the chat at https://gitter.im/graphql-go/graphql](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/graphql-go/graphql?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

A *work-in-progress* implementation of GraphQL in Go. Its currently a port of `graphql-js` [v0.6.0](https://github.com/graphql/graphql-js/releases/tag/v0.6.0) which is based on the [April 2016](https://github.com/facebook/graphql/releases/tag/April2016) GraphQL specification. Future efforts will be guided directly by the [latest formal GraphQL specification](https://github.com/facebook/graphql/releases) (currently: [October 2016](https://github.com/facebook/graphql/releases/tag/October2016)).

### Documentation

godoc: https://godoc.org/github.com/graphql-go/graphql

### Getting Started

To install the library, run:
```bash
go get github.com/graphql-go/graphql
```

The following is a simple example which defines a schema with a single `hello` string-type field and a `Resolve` method which returns the string `world`. A GraphQL query is performed against this schema with the resulting output printed in JSON format.

```go
package main

import (
	"encoding/json"
	"fmt"
	"log"

	"github.com/graphql-go/graphql"
)

func main() {
	// Schema
	fields := graphql.Fields{
		"hello": &graphql.Field{
			Type: graphql.String,
			Resolve: func(p graphql.ResolveParams) (interface{}, error) {
				return "world", nil
			},
		},
	}
	rootQuery := graphql.ObjectConfig{Name: "RootQuery", Fields: fields}
	schemaConfig := graphql.SchemaConfig{Query: graphql.NewObject(rootQuery)}
	schema, err := graphql.NewSchema(schemaConfig)
	if err != nil {
		log.Fatalf("failed to create new schema, error: %v", err)
	}

	// Query
	query := `
		{
			hello
		}
	`
	params := graphql.Params{Schema: schema, RequestString: query}
	r := graphql.Do(params)
	if len(r.Errors) > 0 {
		log.Fatalf("failed to execute graphql operation, errors: %+v", r.Errors)
	}
	rJSON, _ := json.Marshal(r)
	fmt.Printf("%s \n", rJSON) // {“data”:{“hello”:”world”}}
}
```
For more complex examples, refer to the [examples/](https://github.com/graphql-go/graphql/tree/master/examples/) directory and [graphql_test.go](https://github.com/graphql-go/graphql/blob/master/graphql_test.go).

### Third Party Libraries
| Name          | Author        | Description  |
|:-------------:|:-------------:|:------------:|
| [graphql-go-handler](https://github.com/graphql-go/graphql-go-handler) | [Hafiz Ismail](https://github.com/sogko) | Middleware to handle GraphQL queries through HTTP requests. |
| [graphql-relay-go](https://github.com/graphql-go/graphql-relay-go) | [Hafiz Ismail](https://github.com/sogko) | Lib to construct a graphql-go server supporting react-relay. |
| [golang-relay-starter-kit](https://github.com/sogko/golang-relay-starter-kit) | [Hafiz Ismail](https://github.com/sogko) | Barebones starting point for a Relay application with Golang GraphQL server. |
| [dataloader](https://github.com/nicksrandall/dataloader) | [Nick Randall](https://github.com/nicksrandall) | [DataLoader](https://github.com/facebook/dataloader) implementation in Go. |

### Blog Posts
- [Golang + GraphQL + Relay](http://wehavefaces.net/)

