To be able to create app a deployment config had to execute below commands in cluster where app was deployed as app
- oc adm policy add-scc-to-user anyuid -z default  --as system:admin
- oc adm policy add-scc-to-user anyuid -z deploy  --as system:admin
- oc adm policy add-scc-to-user anyuid -z  builder  --as system:admin

Application was created from below repo:
- oc new-app --as-deployment-config https://github.com/Anshu-Garg/filebrowser.git
- oc expose service/filebrowser

Following was used to create application manifest:
- kubectl get dc filebrowser -o yaml> deployment.yaml
- kubectl get route filebrowser -o yaml> route.yaml
- kubectl get service/filebrowser -o yaml > service.yaml

**Deployment via ACM console**
Above were then moved to new repo for adoption https://github.com/Anshu-Garg/filebrowser-move.git

Ensure you create a namespace : filebrowser in target cluster
Ensure you create a pvc named afm-pvc in namespace filebrowser

I used below yaml to create pvc

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: afm-pvc
spec:
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 10Gi
```

Adoption happened from  https://github.com/Anshu-Garg/filebrowser-move.git:

- with branch : main
     - path: fb
     - name: filebrowser
     - namespace: filebrowser
