# Kubernetes: Deployment vs StatefulSet

This repo helps you understand the difference between **Deployment** and **StatefulSet** in Kubernetes using:
- 🧠 Simple real-world examples
- 🔁 YAML configurations
- 📊 Visual diagram
- 📝 Use-case comparison table

---

## 📖 Introduction

### What is a Deployment?
A **Deployment** in Kubernetes is a resource object designed to manage stateless applications. It ensures that a specified number of identical Pods are running at all times. If a Pod fails, the Deployment automatically replaces it with a new one, making it ideal for applications where Pods are interchangeable and don’t need to maintain individual state or identity—like web servers or API services.

### What is a StatefulSet?
A **StatefulSet** is a Kubernetes resource tailored for stateful applications. Unlike Deployments, it assigns a unique, stable identity to each Pod (e.g., `mysql-0`, `mysql-1`) and ensures they are created and terminated in a specific order. It’s perfect for apps that require persistent storage or need to maintain state, such as databases or distributed systems like Kafka.

---

## 🔍 Basic Difference

| Feature            | Deployment                     | StatefulSet                               |
|--------------------|----------------------------------|--------------------------------------------|
| Pod Identity        | Random & interchangeable         | Fixed (sticky) identity per Pod            |
| Pod Names           | Random (e.g., nginx-xyz-123)     | Ordered (e.g., mysql-0, mysql-1...)        |
| Use Cases           | Stateless apps (e.g., Nginx)     | Stateful apps (e.g., MySQL, Kafka)         |
| Pod Storage         | No persistent volume by default  | Each Pod gets its own stable volume        |

---

## 🏫 Real-Life Analogy

### 🪑 Deployment → Classroom Benches
- All benches are same.
- Any student can sit on any bench.
- If one breaks, replace with same kind.

### 🏥 StatefulSet → Hospital Beds
- Each bed has a number (bed-0, bed-1).
- Each patient returns to same bed.
- Personal reports (storage) stay with the bed.

---

## 🧪 YAML Examples

### ✅ Deployment YAML (stateless)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

### ✅ StatefulSet YAML (stateful)
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql-statefulset
spec:
  serviceName: "mysql"
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:latest
          ports:
            - containerPort: 3306
          volumeMounts:
            - name: mysql-storage
              mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: mysql-storage
      spec:
        accessModes: [ "ReadWriteOnce" ]
        resources:
          requests:
            storage: 1Gi
```

---

## 📊 When to Use What?

| Use Case               | Use Deployment? | Use StatefulSet? |
|------------------------|-----------------|------------------|
| Nginx Web Server       | ✅ Yes           | ❌ No            |
| MySQL/PostgreSQL DB    | ❌ No            | ✅ Yes           |
| Redis (non-clustered)  | ✅ Yes           | ❌ No            |
| Kafka, Zookeeper       | ❌ No            | ✅ Yes           |

---

---

## ✅ Summary

- Use **Deployment** when your app doesn’t care which pod handles the request.
- Use **StatefulSet** when your app needs to **remember who it is**, and **store data** persistently.
- Know the YAML structure for both – it's a key DevOps skill!

---
