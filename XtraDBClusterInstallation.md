Install Percona XtraDB Cluster on Kubernetes¶
First of all, clone the percona-xtradb-cluster-operator repository:


$ git clone -b v1.13.0 https://github.com/percona/percona-xtradb-cluster-operator
$ cd percona-xtradb-cluster-operator
Note

It is crucial to specify the right branch with -b option while cloning the code on this step. Please be careful.

Now Custom Resource Definition for Percona XtraDB Cluster should be created from the deploy/crd.yaml file. Custom Resource Definition extends the standard set of resources which Kubernetes “knows” about with the new items (in our case ones which are the core of the operator).

This step should be done only once; it does not need to be repeated with the next Operator deployments, etc.


$ kubectl apply -f deploy/crd.yaml
#The next thing to do is to add the pxc namespace to Kubernetes, not forgetting to set the correspondent context for further steps:


$ kubectl create namespace pxc
$ kubectl config set-context $(kubectl config current-context) --namespace=pxc
Now RBAC (role-based access control) for Percona XtraDB Cluster should be set up from the deploy/rbac.yaml file. Briefly speaking, role-based access is based on specifically defined roles and actions corresponding to them, allowed to be done on specific Kubernetes resources (details about users and roles can be found in Kubernetes documentation).


$ kubectl apply -f deploy/rbac.yaml
Note

Setting RBAC requires your user to have cluster-admin role privileges. For example, those using Google Kubernetes Engine can grant user needed privileges with the following command:

$ kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin --user=$(gcloud config get-value core/account)

Finally it’s time to start the operator within Kubernetes:


$ kubectl apply -f deploy/operator.yaml
Note

You can simplify the Operator installation by applying a single deploy/bundle.yaml file instead of running commands from the steps 2 and 4:


$ kubectl apply -f deploy/bundle.yaml
This will automatically create Custom Resource Definition, set up role-based access control and install the Operator as one single action.

Now that’s time to add the Percona XtraDB Cluster users Secrets with logins and passwords to Kubernetes. By default, the Operator generates users Secrets automatically, and no actions are required at this step.

Still, you can generate and apply your Secrets on your own. In this case, place logins and plaintext passwords for the user accounts in the data section of the deploy/secrets.yaml file; after editing is finished, create users Secrets with the following command:


$ kubectl create -f deploy/secrets.yaml
More details about secrets can be found in Users.

Now certificates should be generated. By default, the Operator generates certificates automatically, and no actions are required at this step. Still, you can generate and apply your own certificates as secrets according to the TLS instructions.

After the operator is started and user secrets are added, Percona XtraDB Cluster can be created at any time with the following command:


$ kubectl apply -f deploy/cr.yaml
Creation process will take some time. The process is over when both operator and replica set pod have reached their Running status:

NAME                                               READY   STATUS    RESTARTS   AGE
cluster1-haproxy-0                                 2/2     Running   0          6m17s
cluster1-haproxy-1                                 2/2     Running   0          4m59s
cluster1-haproxy-2                                 2/2     Running   0          4m36s
cluster1-pxc-0                                     3/3     Running   0          6m17s
cluster1-pxc-1                                     3/3     Running   0          5m3s
cluster1-pxc-2                                     3/3     Running   0          3m56s
percona-xtradb-cluster-operator-79966668bd-rswbk   1/1     Running   0          9m54s
Check connectivity to newly created cluster


$ kubectl run -i --rm --tty percona-client --image=percona:8.0 --restart=Never -- bash -il
percona-client:/$ mysql -h cluster1-haproxy -uroot -proot_password
This command will connect you to the MySQL monitor.

mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1976
Server version: 8.0.19-10 Percona XtraDB Cluster (GPL), Release rel10, Revision 727f180, WSREP version 26.4.3

Copyright (c) 2009-2020 Percona LLC and/or its affiliates
Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
