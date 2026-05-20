*Kubernetes RBAC Interview Questions & Answers*

1️⃣ What is RBAC in Kubernetes?
RBAC (Role-Based Access Control) is used to control who can access Kubernetes resources and what actions they can perform.

---

2️⃣ What are the main RBAC components?
• Role
• ClusterRole
• RoleBinding
• ClusterRoleBinding

---

3️⃣ Difference between Role and ClusterRole?

🔹 Role

* Namespace scoped
* Used for pods/services/deployments

🔹 ClusterRole

* Cluster scoped
* Used for nodes/namespaces/storageclasses

---

4️⃣ Difference between RoleBinding and ClusterRoleBinding?

🔹 RoleBinding

* Attaches Role to user/service account within namespace

🔹 ClusterRoleBinding

* Attaches ClusterRole cluster-wide

---

5️⃣ Cluster-Level Resources

• nodes → Entire cluster infrastructure
• namespaces → Top-level isolation
• persistentvolumes → Shared storage
• storageclasses → Cluster storage configuration

✅ Usually use:
ClusterRole + ClusterRoleBinding

---

6️⃣ Namespace-Level Resources

• pods
• deployments
• services
• configmaps

✅ Usually use:
Role + RoleBinding

---

7️⃣ What does apiGroups mean in RBAC?

It specifies which Kubernetes API group the resource belongs to.

Example:
apiGroups: [""]

means core API group.

---

8️⃣ What are verbs in RBAC?

• get → Read one
• list → List resources
• watch → Monitor changes
• create → Create resource
• update → Full update
• patch → Partial update
• delete → Remove resource

---

9️⃣ Difference between update and patch?

🔹 update

* Replaces full object

🔹 patch

* Updates only specific fields

---

🔟 Why use ClusterRole for nodes?

Because nodes are cluster-level resources and not tied to namespaces.

---

1️⃣1️⃣ What is least privilege access?

Giving only minimum required permissions to users/services.

---

1️⃣2️⃣ How does EKS authentication and authorization work?

✅ Authentication → AWS IAM / SSO
✅ Authorization → Kubernetes RBAC

---

1️⃣3️⃣ What is RoleBinding used for?

It connects user/service account with Role.

---

1️⃣4️⃣ Can ClusterRole be used with RoleBinding?

Yes.
ClusterRole can also provide namespace-level access using RoleBinding.

---

1️⃣5️⃣ What is ServiceAccount in Kubernetes?

Identity used by pods/applications to access Kubernetes API.

---

1️⃣6️⃣ RBAC Access Flow?

User/Pod
↓
Authentication
↓
RoleBinding/ClusterRoleBinding
↓
Role/ClusterRole
↓
Permissions
↓
Kubernetes API Access

---

1️⃣7️⃣ How to check permissions?

kubectl auth can-i get pods

---

1️⃣8️⃣ How to check another user's access?

kubectl auth can-i delete pods --as=anil

---

1️⃣9️⃣ What is cluster-admin?

Built-in ClusterRole with full Kubernetes access.

---

2️⃣0️⃣ Best RBAC practices?

✅ Use least privilege
✅ Avoid cluster-admin for everyone
✅ Use namespace isolation
✅ Use groups instead of per-user RBAC
✅ Enable audit logging
✅ Separate dev/qa/prod access
