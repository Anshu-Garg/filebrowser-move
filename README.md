To be able to create app a deployment config had to execute below commands in cluster where app was deployed as app
oc adm policy add-scc-to-user anyuid -z default  --as system:admin
oc adm policy add-scc-to-user anyuid -z deploy  --as system:admin
oc adm policy add-scc-to-user anyuid -z  builder  --as system:admin

Application was created from below repo
oc new-app --as-deployment-config https://github.com/Anshu-Garg/filebrowser.git

Adoption happened from  https://github.com/Anshu-Garg/filebrowser.git
with branch : main
     path: fb
     name: filebrowser
     namespace: filebrowser
