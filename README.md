==========================================
==========================================
**As of February 5, 2022 these steps work**

- Create namespace filebrowser on hub cluster (optional)
- Create pvc using this file on hub cluster https://github.com/Anshu-Garg/filebrowser-move/blob/main/fb-bkp/pvc.yaml
- Deploy application using ACM console-> subscription , use below parameters
  - name: file-browser
  - namespace: filebrowser
  - Repo type: Git
  - URL: https://github.com/Anshu-Garg/filebrowser-move.git
  - Branch: main
  - Path: fb-bkp
  - Reconcile option: Merge
  - Label==> site=onprem or any label of your choice for cluster matching
-  Application should be up and can be verified

Now to move application

- Create namespace filebrowser on hub cluster
- Create pvc using this file on hub cluster https://github.com/Anshu-Garg/filebrowser-move/blob/main/fb-bkp/pvc.yaml
- From Application menu on ACM console, edit application and change label according to target cluster
- This should move application


==========================================
==========================================

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
