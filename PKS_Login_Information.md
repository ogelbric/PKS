
With Enterprise PKS how do we get the credentials for individual K8 clusters and provide them to developers who want to operate directly with K8 clusters?
<hr>


<B>Log into PKS<\B>
 
 
root@orfpks2 [ ~ ]# pks login -a orfpksapi2.lab.local -u admin --ca-cert /tmp/admincert -p AlaBmczopIUmSL_7dIXAom7Ay7aETTxq
 
API Endpoint: orfpksapi2.lab.local
User: admin
 

#Take a look at just how many clusters do I have….

root@orfpks2 [ ~ ]# pks clusters
 
Name   Plan Name  UUID                                  Status     Action
chevy  Small      866dfb27-6444-4bf1-8fbe-e2bf91eae018  succeeded  CREATE
 
root@orfpks2 [ ~ ]# ls -ltr .kube/config
-rw------- 1 root root 11067 Jul 23 13:07 .kube/config
root@orfpks2 [ ~ ]# cp .kube/config .kube/config.bak
root@orfpks2 [ ~ ]# rm .kube/config
 

#For giggles I delete my kube config file on the machine I loogged in from…
#And get my credentials
 
root@orfpks2 [ ~ ]# pks get-credentials chevy
 

Fetching credentials for cluster chevy.
Context set for cluster chevy.
 
You can now switch between clusters by using:
$kubectl config use-context <cluster-name>

root@orfpks2 [ ~ ]# ls -ltr .kube/config
-rw------- 1 root root 2767 Jul 24 11:29 .kube/config

 
#Now I have a brand new kube config file. Which can be sent to the developer. In my case I pasted it to my mac and fixed up (Mac cant see DNS server in lab) the line

     '''server: https://chevy.lab.local:8443'''
with
     '''server: https://10.197.104.145:8443'''
 
 
 
#root@orfpks2 [ ~ ]# cat  .kube/config

apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: Ci0tLS0tQkVHSU4gQ0VSVElGSUNBVEUtLS0tLQpNSUlDK3pDQ0FlT2dBd0lCQWdJVVVmZk8reHVjMkE4V0VJM2pDWG9SWGhqMG9NMHdEUVlKS29aSWh2Y05BUUVMCkJRQXdEVEVMTUFrR0ExVUVBeE1DWTJFd0hoY05NVGt3TnpFMU1USXlPRFEzV2hjTk1qTXdOekUxTVRJeU9EUTMKV2pBTk1Rc3dDUVlEVlFRREV3SmpZVENDQVNJd0RRWUpLb1pJaHZjTkFRRUJCUUFEZ2dFUEFEQ0NBUW9DZ2dFQgpBS1RldmRuWlZEOG8zdEV1NlVsVElRb3Axb3lVMGxzNjlBbzdvYUQ4SEZ6N2RYcmlWZHZXMEM0UGVHVzcxZ1NhCmowaDNEWEU2Sk03eVB3THRKK2dhV2s2SHZpRnRJbVZESUg0M3pSTGgrMzV0SDNnMy80cFR4d3VTdXJ3cWkrNWwKT0E4UzMwYzNwMjdZQ1d1NHdyMmlEWFhYYTJhQW04cTREbmJINVRpQ3hOT1hmZkxIZW9vaXNNYzBqYnpDV3ZaKwoyZjMzOU1XalA4anFuSFNuUVQxTFlqeUFGS0FYaFBRU2lQVFlwWDBLdVlGeFRRZGs2YXBJamZVVmR1ZURRSHk1CjlFdXJjMTB6OUlSTTVVc2NvUU1HdXdETUoxNGdWV2Z0VlFid0FmM054MTVKMXFmVkJsNDRBbmtQWUNWd09LL08KcjMvNkFFNHRRMlNwdFM5NjFBZng5bmtDQXdFQUFhTlRNRkV3SFFZRFZSME9CQllFRkNBdTdRemlTQXdOQXNnUwpMazV4bGtTN2o0NXZNQjhHQTFVZEl3UVlNQmFBRkNBdTdRemlTQXdOQXNnU0xrNXhsa1M3ajQ1dk1BOEdBMVVkCkV3RUIvd1FGTUFNQkFmOHdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBRk5tS1MxNHUrMTdnUjVUakhuNUNmRXUKM1BtM2Fndi9teWRQMTZKcWtuVUxrN3ZQV0VzK1dOUVVDYjNYK0wram9rWGMyWFozanltL2VFNlNFNUdUQlJmMwpmZGE4UVlsWHo3SGhiK0dWREhheWJJa25ndTJRWG9ibXcvcGNzMmZQcElwOTh5OG9JaktNcDh5c09iajZaY1o0CmFTWmpPUm9jYmhaVXYyMkpmUWxweHhhay9QVTZtM1FIbW1nZmNodU1BQVhydWtveVRmbC8vdHoreDV5bk9IYkEKWmRwcm02OG9YeWpTWlBURWxob0cwR2JKM2Z6dHh2TFJXOGVPU1NjZkdpL0d5c1JEb1UxMGJCRzI5S1BYR2ppSwoyNFp2aVpkTkhGV0VjTVRVVEc0RXJWaVovYVArOW5GdWlqeVR5Y0t4QnZiNE1PZWRVMjh2ZkNoQ1IzSWxaVTQ9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: https://chevy.lab.local:8443
  name: chevy
contexts:
- context:
    cluster: chevy
    user: 7253e224-c535-4146-9524-288516089da7
  name: chevy
current-context: chevy
kind: Config
preferences: {}
users:
- name: 7253e224-c535-4146-9524-288516089da7
  user:
    token: eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6IjcyNTNlMjI0LWM1MzUtNDE0Ni05NTI0LTI4ODUxNjA4OWRhNy10b2tlbi1zazl0aiIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiI3MjUzZTIyNC1jNTM1LTQxNDYtOTUyNC0yODg1MTYwODlkYTciLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiIzZjUxMzU0OS1hZTA2LTExZTktOGI1OS0wMDUwNTY4ZDliZjYiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6ZGVmYXVsdDo3MjUzZTIyNC1jNTM1LTQxNDYtOTUyNC0yODg1MTYwODlkYTcifQ.psSdRbjHrKiKCjKCrNeya176XSL3MaloNsv_J1JHCOCpyk7xg6T300xnuSAtDmiNtgZeu6ORx5ee9zD-nGB7E0bgVx8aMYKr5I2CkAkR9VKbyEciDLHZwJ8U6CHSOy62bnAFVjT10uu-ch3AsX_4VDXb9Q_6vShfaTfmi95NLxHgUvWHciq1-ZQFMMoYctvTsWj_jHBuf_jh_v1lyo73tDK2HGqwiY0lwt_goFBA3jJWkGnWWpZxW2MRWCCzY0zzl2BDnOsnMAbvaT194PmHjqc3vjxzpMqvptnRRdI4s1YLa5gfauTOYWWlWMgiX2cQFCMta8ALcNZgWFXNOgSR8g
root@orfpks2 [ ~ ]# 
 
 
#From my MAC this works now:
 
ogelbrich-a01:~ ogelbrich$ kubectl get nodes
NAME                                   STATUS    ROLES     AGE       VERSION
50b71f51-5bc6-4078-866a-3a1249b953fc   Ready     <none>    8d        v1.13.5
58a8b454-2a84-4f39-8538-f0540ed987f6   Ready     <none>    6d        v1.13.5
bfb5660e-51e7-454a-ae8f-6a949cf2e76b   Ready     <none>    8d        v1.13.5
ogelbrich-a01:~ ogelbrich$ 
 
 
#Then I can get the Kubernetes Load balancer (if set up on system)
 
ogelbrich-a01:~ ogelbrich$ kubectl config set-context chevy --namespace kube-system
Context "chevy" modified.
ogelbrich-a01:~ ogelbrich$ kubectl get services 
NAME                   TYPE           CLUSTER-IP       EXTERNAL-IP        PORT(S)         AGE
kube-dashboard-lb      LoadBalancer   10.100.200.47    10.197.104.14...   443:31447/TCP   23h
kube-dns               ClusterIP      10.100.200.10    <none>             53/UDP,53/TCP   8d
kubernetes-dashboard   NodePort       10.100.200.102   <none>             443:32753/TCP   8d
metrics-server         ClusterIP      10.100.200.55    <none>             443/TCP         8d
ogelbrich-a01:~ ogelbrich$ kubectl describe svc | grep -i load
Type:                     LoadBalancer
LoadBalancer Ingress:     10.197.104.148, 100.64.64.15
ogelbrich-a01:~ ogelbrich$ 
 
 
#In a browser go to
 
https: //10.197.104.148
 
and use the token from the .kube/config file  (Last line)
 
eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6IjcyNTNlMjI0LWM1MzUtNDE0Ni05NTI0LTI4ODUxNjA4OWRhNy10b2tlbi1zazl0aiIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiI3MjUzZTIyNC1jNTM1LTQxNDYtOTUyNC0yODg1MTYwODlkYTciLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiIzZjUxMzU0OS1hZTA2LTExZTktOGI1OS0wMDUwNTY4ZDliZjYiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6ZGVmYXVsdDo3MjUzZTIyNC1jNTM1LTQxNDYtOTUyNC0yODg1MTYwODlkYTcifQ.psSdRbjHrKiKCjKCrNeya176XSL3MaloNsv_J1JHCOCpyk7xg6T300xnuSAtDmiNtgZeu6ORx5ee9zD-nGB7E0bgVx8aMYKr5I2CkAkR9VKbyEciDLHZwJ8U6CHSOy62bnAFVjT10uu-ch3AsX_4VDXb9Q_6vShfaTfmi95NLxHgUvWHciq1-ZQFMMoYctvTsWj_jHBuf_jh_v1lyo73tDK2HGqwiY0lwt_goFBA3jJWkGnWWpZxW2MRWCCzY0zzl2BDnOsnMAbvaT194PmHjqc3vjxzpMqvptnRRdI4s1YLa5gfauTOYWWlWMgiX2cQFCMta8ALcNZgWFXNOgSR8g
 
#You end up here…
 
Login screen with token:

