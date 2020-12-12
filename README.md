# Habilitando o Dashboard no OKE

Este procedimento permite habilitar o dashboard ao cluster de OKE (Kubernetes). https://docs.cloud.oracle.com/pt-br/iaas/Content/ContEng/Concepts/contengoverview.htm

Após criação do cluster e configuração do kubeconfig, digite o seguinte via terminal<br>

```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.3/aio/deploy/recommended.yaml
```
<br>

Crie localmente o arquivo abaixo e nomeie de autorizacao.yaml. Ele descreverá o service account e clusterrolebinding que criaremos no cluster..<br>

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: oke-admin
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: oke-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: oke-admin
    namespace: kube-system
```
<br>
Efetue a criação digitando<br>

```
$ kubectl apply -f autorizacao.yaml
```
Copie o token de acesso efetuando a consulta no secrets do cluster via terminal:<br>
```
$ kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep oke-admin | awk '{print $1}')
```
Ainda no terminal, habilite o proxy ao cluster:<br>
```
$ kubectl proxy
```
Abra seu navegador de preferência e ingresse na URL<br>
```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#!/login
```
Cole o token copiado da consulta ao secret realizada acima <br>

<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/p/Yux6rAJ5nHXL9IF9OWNMp4ZGfXaCEphHa7WdF89bGj_bcpvdMxkYo5pwnkQKTh_e/n/idsvh8rxij5e/b/imagens_git/o/Captura%20de%20tela%20de%202020-12-12%2001-56-25.png"/></th>
        </tr>
    </tbody>
</table>

Clique em Sign-in e o dashboard será apresentado com os detalhes dos worker nodes

<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/p/_RLR2HsWFBjghanFMFf6jp0l9By9gcgjldiZq6yGWXYlJIa2uPWtp7fVA0WopI7l/n/idsvh8rxij5e/b/imagens_git/o/Captura%20de%20tela%20de%202020-12-12%2002-01-07.png"/></th>
        </tr>
    </tbody>
</table>





