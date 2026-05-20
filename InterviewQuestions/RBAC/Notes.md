# Kubernetes RBAC Interview Questions & Answers

## 1. What is RBAC in Kubernetes?

RBAC (Role-Based Access Control) is used to control:

* Who can access Kubernetes resources
* What actions they can perform

---

## 2. What are the main RBAC components?

| Component          | Purpose                             |
| ------------------ | ----------------------------------- |
| Role               | Namespace-level permissions         |
| ClusterRole        | Cluster-level permissions           |
| RoleBinding        | Attach Role to user/service account |
| ClusterRoleBinding | Attach ClusterRole cluster-wide     |

---

## 3. Difference between Role and ClusterRole?

### Role

* Namespace scoped
* Used for:

  * pods
  * deployments
  * services
  * configmaps

### ClusterRole

* Cluster scoped
* Used for:

  * nodes
  * namespaces
  * persistentvolumes
  * storageclasses

---

## 4. Difference between RoleBinding and ClusterRoleBinding?

| Component          | Purpose                        |
| ------------------ | ------------------------------ |
| RoleBinding        | Binds Role inside namespace    |
| ClusterRoleBinding | Binds ClusterRole cluster-wide |

---

# Resource Scope

## Cluster-Level Resources

| Resource          | Purpose                       |
| ----------------- | ----------------------------- |
| nodes             | Entire cluster infrastructure |
| namespaces        | Top-level isolation           |
| persistentvolumes | Shared storage                |
| storageclasses    | Cluster storage configuration |

### Usually Uses

```yaml
ClusterRole + ClusterRoleBinding
```

---

## Namespace-Level Resources

* pods
* deployments
* services
* configmaps

### Usually Uses

```yaml
Role + RoleBinding
```

---

## 5. What is apiGroups?

`apiGroups` specifies which Kubernetes API group the resource belongs to.

Example:

```yaml
apiGroups: [""]
```

Means:

```text
Core Kubernetes API group
```

Examples:

| Resource    | API Group         |
| ----------- | ----------------- |
| pods        | ""                |
| services    | ""                |
| configmaps  | ""                |
| deployments | apps              |
| daemonsets  | apps              |
| ingresses   | networking.k8s.io |

---

## 6. What are verbs in RBAC?

| Verb   | Purpose         |
| ------ | --------------- |
| get    | Read one        |
| list   | List resources  |
| watch  | Monitor changes |
| create | Create resource |
| update | Full update     |
| patch  | Partial update  |
| delete | Remove resource |

---

## 7. Difference between update and patch?

| update               | patch                        |
| -------------------- | ---------------------------- |
| Replaces full object | Updates specific fields only |

### Example update

```bash
kubectl apply -f deployment.yaml
```

### Example patch

```bash
kubectl patch deployment nginx \
-p '{"spec":{"replicas":3}}'
```

---

## 8. Why use ClusterRole for nodes?

Because:

```text
nodes are cluster-scoped resources
```

and not tied to any namespace.

---

## 9. What is least privilege access?

Giving only minimum required permissions to users/services.

Example:

* Developer → pod read access only
* Avoid cluster-admin access for everyone

---

## 10. Authentication vs Authorization

### Authentication

Checks:

```text
Who are you?
```

Handled by:

* AWS IAM
* AWS SSO
* Token
* Certificates

### Authorization

Checks:

```text
What are you allowed to do?
```

Handled by:

* Kubernetes RBAC

---

## 11. What is ServiceAccount?

Identity used by:

* Pods
* Applications

to access Kubernetes API.

---

## 12. RBAC Access Flow

```text
User / Pod
     ↓
Authentication
     ↓
RoleBinding / ClusterRoleBinding
     ↓
Role / ClusterRole
     ↓
Permissions
     ↓
Kubernetes API Access
```

---

## 13. ServiceAccount RBAC Flow

```text
Application Pod
      ↓
ServiceAccount
      ↓
Authentication to API Server
      ↓
RoleBinding / ClusterRoleBinding
      ↓
Role / ClusterRole
      ↓
Permissions (verbs + resources)
      ↓
Kubernetes API Access
```

---

## 14. How to check permissions?

### Current User

```bash
kubectl auth can-i get pods
```

### Another User

```bash
kubectl auth can-i delete pods --as=anil
```

---

## 15. What is cluster-admin?

Built-in ClusterRole with full Kubernetes access.

Avoid giving cluster-admin to everyone.

---

## 16. How does EKS authentication and authorization work?

```text
Authentication → AWS IAM / SSO
Authorization → Kubernetes RBAC
```

---

## 17. Example Role YAML

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role

metadata:
  namespace: dev
  name: pod-reader

rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get","list"]
```

---

## 18. Example ClusterRole YAML

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole

metadata:
  name: node-reader

rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["get","list"]
```

---

## 19. Example RoleBinding YAML

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding

metadata:
  name: dev-binding
  namespace: dev

subjects:
- kind: User
  name: anil

roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

---

## 20. Example ClusterRoleBinding YAML

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding

metadata:
  name: node-binding

subjects:
- kind: User
  name: anil

roleRef:
  kind: ClusterRole
  name: node-reader
  apiGroup: rbac.authorization.k8s.io
```

---

## 21. Full EKS User Access Flow

```text
1. Create AWS profile/user
        ↓
2. Give EKS cluster access
        ↓
3. Assign RBAC permissions
        ↓
4. User can access cluster
```

---

## 22. Common kubectl Commands

### Get nodes

```bash
kubectl get nodes
```

### Get pods

```bash
kubectl get pods -A
```

### Describe pod

```bash
kubectl describe pod nginx
```

### Logs

```bash
kubectl logs nginx
```

---

## 23. Best RBAC Practices

* Use least privilege access
* Avoid cluster-admin for everyone
* Use namespace isolation
* Use groups instead of per-user RBAC
* Enable audit logging
* Separate dev/qa/prod access
* Use IAM + RBAC together in EKS
* Avoid sharing kubeconfig files

---

## 24. Important Interview One-Liners

### What is RBAC?

RBAC is Kubernetes authorization mechanism used to control access to resources using Roles, ClusterRoles, and Bindings.

### Role vs ClusterRole?

Role is namespace-scoped, while ClusterRole is cluster-scoped.

### Why ClusterRole for nodes?

Because nodes are cluster-level resources.

### Authentication vs Authorization?

Authentication verifies identity, while authorization verifies permissions.

### What does patch mean?

Partial update of a Kubernetes resource.

### What does apiGroups indicate?

It specifies which Kubernetes API group the resource belongs to.
