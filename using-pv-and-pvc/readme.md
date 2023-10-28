To create a new manifest called `pv-pvc.yaml` for a PersistentVolume (PV) and PersistentVolumeClaim (PVC) and modify your existing `apache.yaml` to reference the new PVC, 
you can follow these steps. \
In this example, we'll create a basic PV and PVC for demonstration purposes. \
You can adjust the specifications according to your specific requirements.

Here's the `pv-pvc.yaml` manifest:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: apache-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /data/apache   # Change this path to the desired host path
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: apache-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

This manifest defines a PV (`apache-pv`) and a corresponding PVC (`apache-pvc`) with a 5Gi storage capacity and a host path of `/data/apache`. \
Make sure to adjust the `hostPath` to your actual storage location.

Next, you need to modify your `apache.yaml` manifest to reference this new PVC. You can add a `volumes` and `volumeMounts` section to your container definition. \
Here's the modified `apache.yaml` with the changes:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test-deployment
  labels:
    app: apache
spec:
  selector:
    matchLabels:
      app: test
  replicas: 1
  template:
    metadata:
      labels:
        app: test
    spec:
      containers:
      - name: test
        image: ubuntu:20.04
        command: ["/bin/sh", "-c", "apt update -y && apt install -y apache2 && service apache2 start && tail -f /dev/null"]
        ports:
        - name: http
          containerPort: 80
        env:
        - name: DEBIAN_FRONTEND
          value: noninteractive
        volumeMounts:  # Add this section
        - name: apache-data
          mountPath: /var/www/html   # Adjust the mountPath as needed
      volumes:  # Add this section
      - name: apache-data
        persistentVolumeClaim:
          claimName: apache-pvc  # Reference the PVC defined in pv-pvc.yaml
---
apiVersion: v1
kind: Service
metadata:
  name: apache-service
spec:
  selector:
    app: test
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
    targetPort: http
```

Now, your `apache.yaml` includes a PVC named `apache-pvc`, and a volume is mounted to the container at `/var/www/html`. \
Make sure the `claimName` in the `persistentVolumeClaim` matches the PVC name defined in the `pv-pvc.yaml` manifest. \
Adjust the `mountPath` and other specifications as needed for your specific use case.

