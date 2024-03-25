# OPA GATEKEEPER POLICIES

This repository contains a set of baseline policies for Gatekeeper to ensure adherence to best security practices in Kubernetes YAML files.

## Prerequisites

1. Gatekeeper
2. kubectl

## List of Policies

1. **Allowed Usage of Whitelisted Repositories**: Permits only specified container image repositories, ensuring the use of trusted images for enhanced security and compliance.

### Allowed repository list:
1. opa
2. docker

> sampleallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: opa-allowed
spec:
  containers:
    - name: opa
      image: openpolicyagent/opa:0.9.2
      args:
        - "run"
        - "--server"
        - "--addr=localhost:8080"
```
> sampledisallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-disallowed
spec:
  containers:
    - name: nginx
      image: nginx
```
2. **Block Usage of Latest Tag**: Restricts or blocks the usage of the "latest" tag for container images to ensure version control and promote predictable deployments.


> sampleallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: opa-allowed
spec:
  containers:
    - name: opa
      image: openpolicyagent/opa:0.9.2
      args:
        - "run"
        - "--server"
        - "--addr=localhost:8080"
```
> sampledisallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: opa-exempt-allowed
spec:
  containers:
    - name: opa-exp
      image: openpolicyagent/opa-exp:latest
      args:
        - "run"
        - "--server"
        - "--addr=localhost:8080"
```

3. **Allowed Usage of Whitelisted Storage Classes**: Limits storage class usage to approved ones, maintaining consistency and preventing data loss or breaches.

> sampleallowed.yaml

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ok
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 8Gi
  storageClassName: somestorageclass

```
> sampledisallowed.yaml

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nostorageclass
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 8Gi
```

4. **Block Containers with No Resources Limits/Requests**: Enforces setting resource limits and requests to prevent contention and ensure predictable performance.

> sampleallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: opa-allowed
  labels:
    owner: me.agilebank.demo
spec:
  containers:
    - name: opa
      image: openpolicyagent/opa:0.9.2
      args:
        - "run"
        - "--server"
        - "--addr=localhost:8080"
      resources:
        limits:
          cpu: "100m"
          memory: "1Gi"
        requests:
          cpu: "100m"
          memory: "1Gi"
```
> sampledisallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: opa-allowed
  labels:
    owner: me.agilebank.demo
spec:
  containers:
    - name: opa
      image: openpolicyagent/opa:0.9.2
      args:
        - "run"
        - "--server"
        - "--addr=localhost:8080"
```

5. **Block Usage of NodePort**: Prevents exposing services via NodePort, reducing the attack surface and promoting a more secure network configuration.

> sampleallowed.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: my-service-allowed
spec:
  type: ClusterIP
  ports:
    - port: 80
      targetPort: 80
```
> sampledisallowed.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: my-service-disallowed
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```

6. **Block Usage of Service Type LoadBalancer**: Disallows the use of LoadBalancer services, minimizing costs and security risks associated with external access.

> sampleallowed.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: my-service-allowed
spec:
  type: ClusterIP
  ports:
    - port: 80
      targetPort: 80
```
> sampledisallowed.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: my-service-disallowed
spec:
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```

7. **Block Usage of Wildcard Ingress Hostname**: Restricts wildcard usage in Ingress hostnames, enhancing security by preventing potential subdomain takeovers and misconfigurations.

> sampleallowed.yaml

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: non-wildcard-ingress
spec:
  rules:
  - host: 'myservice.example.com'
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: example
            port:
              number: 80
```
> sampledisallowed.yaml

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: wildcard-ingress
spec:
  rules:
  - host: ''
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: example
            port:
              number: 80
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: wildcard-ingress
spec:
  rules:
  # Omitted host field counts as a wildcard too
  - http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: example
            port:
              number: 80
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: wildcard-ingress
spec:
  rules:
  - host: '*.example.com'
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: example
            port:
              number: 80
  # Extra test to ensure the rule still detects invalid hosts in files containing valid hosts
  - host: 'valid.example.com'
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: example
            port:
              number: 80
```

8. **Block Automount Service Account Token for Pod**: Disables automatic mounting of service account tokens, reducing the risk of unauthorized access to sensitive Kubernetes resources.

> sampleallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-automountserviceaccounttoken-allowed
  labels:
    app: nginx-not-automountserviceaccounttoken
spec:
  automountServiceAccountToken: false
  containers:
  - name: nginx
    image: nginx
```
> sampledisallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-automountserviceaccounttoken-disallowed
  labels:
    app: nginx-automountserviceaccounttoken
spec:
  automountServiceAccountToken: true
  containers:
  - name: nginx
    image: nginx
```

9. **Allow Usage of Whitelisted Capabilities**: Specifies permitted Linux capabilities, ensuring only necessary privileges are granted to containers, thus minimizing attack vectors.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

10. **Block Allow Privilege Escalation is True**: Prevents containers from running with privilege escalation, reducing the likelihood of attacks and maintaining cluster integrity.

> sampleallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privilege-escalation-allowed
  labels:
    app: nginx-privilege-escalation
spec:
  containers:
  - name: nginx
    image: nginx
    securityContext:
      allowPrivilegeEscalation: false
```
> sampledisallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privilege-escalation-disallowed
  labels:
    app: nginx-privilege-escalation
spec:
  containers:
  - name: nginx
    image: nginx
    securityContext:
      allowPrivilegeEscalation: true
```

11. **Block Usage of Forbidden Sysctls**: Prohibits the use of specific sysctl settings to mitigate security vulnerabilities and enforce compliance with organizational policies.

### Blocked sysctls:

1. kernel

> sampleallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-forbidden-sysctls-disallowed
  labels:
    app: nginx-forbidden-sysctls
spec:
  containers:
    - name: nginx
      image: nginx
  securityContext:
    sysctls:
      - name: net.core.somaxconn
        value: "1024"
```
> sampledisallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-forbidden-sysctls-disallowed
  labels:
    app: nginx-forbidden-sysctls
spec:
  containers:
    - name: nginx
      image: nginx
  securityContext:
    sysctls:
      - name: kernel.msgmax
        value: "65536"
      - name: net.core.somaxconn
        value: "1024"
```

12. **Block Usage of Host File System and EmptyDir**: Prevents access to the host file system and restricts usage of emptyDir volumes, enhancing security and preventing data leaks.

> sampledisallowedemptydir.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: disallowed-pod
spec:
  containers:
  - name: container1
    image: nginx
    volumeMounts:
    - name: shared-data
      mountPath: /data
  volumes:
  - name: shared-data
    emptyDir: {}
```
> sampledisallowedhostpath.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: disallowed-pod
spec:
  containers:
  - name: container1
    image: nginx
    volumeMounts:
    - name: shared-data
      mountPath: /data
  volumes:
  - name: shared-data
    hostPath:
      path: /etc
```

13. **Block Usage of Host Namespace PID/PIC**: Disallows the use of host PID and IPC namespaces to prevent potential interference with the host system, enhancing isolation and security.

> sampleallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-host-namespace-allowed
  labels:
    app: nginx-host-namespace
spec:
  hostPID: false
  hostIPC: false
  containers:
  - name: nginx
    image: nginx
```
> sampledisallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-host-namespace-disallowed
  labels:
    app: nginx-host-namespace
spec:
  hostPID: true
  hostIPC: true
  containers:
  - name: nginx
    image: nginx
```

14. **Block Usage of Privileged Ports**: Prevents containers from binding to privileged ports, reducing the risk of unauthorized network access and maintaining cluster security.

> sampleallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: disallowed-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```
> sampledisallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-host-networking-ports-disallowed
  labels:
    app: nginx-host-networking-ports
spec:
  hostNetwork: true
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 9001
      hostPort: 9001
```

15. **Block Usage of Privileged Containers**: Prohibits the use of privileged containers to prevent elevated privileges, thereby minimizing the attack surface and enhancing security.

> sampleallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged-allowed
  labels:
    app: nginx-privileged
spec:
  containers:
  - name: nginx
    image: nginx
    securityContext:
      privileged: false
```
> sampledisallowed.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged-disallowed
  labels:
    app: nginx-privileged
spec:
  containers:
  - name: nginx
    image: nginx
    securityContext:
      privileged: true
```

16. **Auto Update Allow Privilege Escalation to False**: This mutation policy will Automatically updates the `allowPrivilegeEscalation` field to `false` for enhanced security and to prevent privilege escalation attacks even if no field is declared