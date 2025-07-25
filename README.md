原仓库主不维护了，只能自己来了，以后欢迎大家来这里提交PR。

目前已经合并了原仓库#27和#18的PR，相比原版：
修复了 Mismatched response length. Expected 1 receipts but only received 3 报错
添加了 useFcmV1=true 的请求param参数

# expo-server-sdk-go
Send push notifications to Expo apps using Go

## Installation
```
go get github.com/cola-93/exponent-server-sdk-golang/sdk
```

## Usage
```go
package main

import (
    "fmt"
    expo "github.com/cola-93/exponent-server-sdk-golang/sdk"
)

func main() {
    // To check the token is valid
    pushToken, err := expo.NewExponentPushToken("ExponentPushToken[xxxxxxxxxxxxxxxxxxxxxx]")
    if err != nil {
        panic(err)
    }

    // Create a new Expo SDK client
    client := expo.NewPushClient(nil)

    // Publish message
    responses, err := client.Publish(
        &expo.PushMessage{
            To: []expo.ExponentPushToken{pushToken},
            Body: "This is a test notification",
            Data: map[string]string{"withSome": "data"},
            Sound: "default",
            Title: "Notification Title",
            Priority: expo.DefaultPriority,
        },
    )
    
    // Check errors
    if err != nil {
        panic(err)
    }
    
    // Validate responses
    for _, res := range responses {
        if res.ValidateResponse() != nil {
            fmt.Println(res.PushMessage.To, "failed")
        }
    }
}
```

## License
MIT
