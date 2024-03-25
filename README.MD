# OPA GATEKEEPER POLICIES

This repository contains a set of baseline policies for Gatekeeper to ensure adherence to best security practices in Kubernetes YAML files.

## Prerequisites

1. Gatekeeper
2. kubectl

## List of Policies

1. **Allowed Usage of Whitelisted Repositories**: Permits only specified container image repositories, ensuring the use of trusted images for enhanced security and compliance.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

2. **Allowed Usage of Whitelisted Storage Classes**: Limits storage class usage to approved ones, maintaining consistency and preventing data loss or breaches.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

3. **Block Containers with No Resources Limits/Requests**: Enforces setting resource limits and requests to prevent contention and ensure predictable performance.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

4. **Block Usage of NodePort**: Prevents exposing services via NodePort, reducing the attack surface and promoting a more secure network configuration.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

5. **Block Usage of Service Type LoadBalancer**: Disallows the use of LoadBalancer services, minimizing costs and security risks associated with external access.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

6. **Block Usage of Wildcard Ingress Hostname**: Restricts wildcard usage in Ingress hostnames, enhancing security by preventing potential subdomain takeovers and misconfigurations.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

7. **Block Automount Service Account Token for Pod**: Disables automatic mounting of service account tokens, reducing the risk of unauthorized access to sensitive Kubernetes resources.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

8. **Allow Usage of Whitelisted Capabilities**: Specifies permitted Linux capabilities, ensuring only necessary privileges are granted to containers, thus minimizing attack vectors.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

9. **Block Allow Privilege Escalation is True**: Prevents containers from running with privilege escalation, reducing the likelihood of attacks and maintaining cluster integrity.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

10. **Block Usage of Forbidden Sysctls**: Prohibits the use of specific sysctl settings to mitigate security vulnerabilities and enforce compliance with organizational policies.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

11. **Block Usage of Host File System and EmptyDir**: Prevents access to the host file system and restricts usage of emptyDir volumes, enhancing security and preventing data leaks.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

12. **Block Usage of Host Namespace PID/PIC**: Disallows the use of host PID and IPC namespaces to prevent potential interference with the host system, enhancing isolation and security.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

13. **Block Usage of Privileged Ports**: Prevents containers from binding to privileged ports, reducing the risk of unauthorized network access and maintaining cluster security.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```

14. **Block Usage of Privileged Containers**: Prohibits the use of privileged containers to prevent elevated privileges, thereby minimizing the attack surface and enhancing security.

> sampleallowed.yaml

```
```
> sampledisallowed.yaml

```
```
