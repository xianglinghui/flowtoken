# flowtoken
flowtoken is a request flow controller, it can adjust flow threshold automatically based on success rate per second.

#usage
```
conf := &flowtoken.Config{
	InitCwnd: 10000,
	MinCwnd:  10,
},

// New flowtoken
ft := flowtoken.NewFlowToken("192.168.1.10:10000", conf)

// Get token from flowtoken
token, err := ft.GetToken()
_, ok := IsTokenExhaustedError(err)
if ok {
	// token exhausted, it may reach the traffic threshold.
	...
	return
}

// request service
err = call(...)

if err == nil{
	//Report succ to flowtoken
	token.Succ()
} else{
	//Report fail to flowtoken
	token.Fail()
}

```
