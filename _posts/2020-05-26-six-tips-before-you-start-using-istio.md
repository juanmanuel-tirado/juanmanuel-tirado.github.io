---
title: "Six tips before you start using Istio"
date: "2020-05-26 11:26:29.000000000 +02:00"
categories:
- K8s
- Systems
tags:
- istio
- kubernetes
- services
permalink: "/six-tips-before-you-start-using-istio/"
excerpt: "Istio can help you to simplify the management of your K8s services. However,
  it can be a bit intimidating. Check out my six tips before you decide to give it
  a try!"
layout: single
clases:
    - wide
    - dark-theme
---

A forgotten aspect of Kubernetes deployments is access control. Kubernetes assumes services are isolated depending on the namespace they belong to. This is a simple rule of thumb that may be good enough for several solutions. However, not all the services may be accessed from any service in the same namespace.

Access control is particularly suitable to avoid confusion between development/production environments and to difficult malicious accesses. A good practice is to define what services can be accessed by whom in order to preserve the coherence of a solution built on top of multiple services.

[Istio](istio.io) offers a very handy solution to define services access control using Authorization Policies. These policies can be used for both HTTP and TCP traffic with access control based on service accounts, namespaces or HTTP methods. For example:

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
 name: myserver
 namespace: foo
spec:
 action: ALLOW
 rules:
 - from:
   - source:
       principals: ["cluster.local/ns/default/sa/myapp"]
   to:
	 - operation:
	    ports:
		- 9200
 - from:
   - source:
       namespaces: ["foo"]
   to:
	 - operation:
	    ports:
		- 9200
		- 9300
 selector:
  matchLabels:
    app: myserver
    version: v1
```
The authorization policy defined above affects to any deployment in the same namespace that matches the selector. This affects to any service accessing a pod matching the selector. This is useful when we have several services for a single endpoint. In particular, this policy authorizes traffic coming from the same namespace `foo` to ports 9200 and 9300. However, only port 9200 is accessible for applications from the service account `myapp`. Accesses from any other source is denied.

Fort HTTP REST APIs it is possible to define a finer granularity access control.
```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
 name: myserver
 namespace: foo
spec:
 action: ALLOW
 rules:
 - from:
   - source:
       principals: ["cluster.local/ns/default/sa/myapp"]
   to:
	 - operation:
	    methods: ["GET"]
		paths: ["/add*"]
 - from:
   - source:
       namespaces: ["foo"]
   to:
	 - operation:
	   methods: ["GET"]
	   paths: ["/add*","/delete*"]
 selector:
  matchLabels:
    app: myserver
    version: v1
```
In this case we grant access to `add` and `delete` methods in the API if and only if traffic comes from namespace `foo`. Other traffic accesses are denied.

I do strongly suggest you to define authorization policies if you have installed Istio. In case you have not done it yet, visit my [previous post](https://jmtirado.net/six-tips-before-you-start-using-istio/) with some tips for Istio. Check out the official Istio [documentation](https://istio.io/docs/reference/config/security/authorization-policy/) for more details about Authorization Policies.