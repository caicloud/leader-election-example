<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Leader Election Example](#leader-election-example)
  - [Running](#running)
  - [Health Check](#health-check)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Leader Election Example

This example demonstrates how to use the leader election package.

## Running

Run the following three commands in separate terminals.

```bash
# first terminal
go run main.go -kubeconfig=/path/to/kubeconfig -logtostderr=true -lease-lock-name=example -lease-lock-namespace=default -id=0 -port=8080

# second terminal
go run main.go -kubeconfig=/path/to/kubeconfig -logtostderr=true -lease-lock-name=example -lease-lock-namespace=default -id=1 -port=8081

# third terminal
go run main.go -kubeconfig=/path/to/kubeconfig -logtostderr=true -lease-lock-name=example -lease-lock-namespace=default -id=2 -port=8082
```

> You can ignore the `-kubeconfig` flag if you are running these commands in the Kubernetes cluster.

Now kill the existing leader. You will see from the terminal outputs that one of the remaining two processes will be elected as the new leader.

## Health Check

```bash
$ curl -i localhost:8080/healthz
HTTP/1.1 200 OK
Content-Type: text/plain; charset=utf-8
X-Content-Type-Options: nosniff
Date: Thu, 19 Sep 2019 10:44:39 GMT
Content-Length: 2

ok%

$ curl -i localhost:8080/healthz/leaderElection
HTTP/1.1 200 OK
Date: Thu, 19 Sep 2019 10:44:31 GMT
Content-Length: 2
Content-Type: text/plain; charset=utf-8

ok%
```
