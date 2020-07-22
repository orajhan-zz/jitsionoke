# Jitsi Meet app deployment in Kunernetes (Oracle Kubernetes Engine)
- running Jitsi Meet app on Kubernetes

I referred to https://github.com/DushmanthaBandaranayake/jitsi-kubernetes-scalable-service and have made some changes below:
Actually Dush's 10-config script is the crucial one to make HPA work since JVB must have unique port to be scaled horizontally by design. 

Prerequisite: Build costomized image with 10-config 

1. Used ingress controller to access Jitsi Meet app in web yaml. 

2. From JVB pod, I am accessing to XMPP Server via service in which 5222 so that I do not have to rely on IP address of Jitsi pod since pod ip will be changed if pod is re-created.
- XMPP_SERVER => web
- In Web yaml, add the following ports. <br />

  - name: "xmpp"   #now JVB is accessing xmpp via service name using port 5222 <br />
    port: 5222 <br />
    targetPort: 5222 <br />

* I have seen many cases that Jitsi kicked the session as soon as the second participant joins the same meeting. This issue most likely related to port issue between JVB and XMPP server. 

3. added resource reuest in jvb yaml. If not, HPA will complain. <br />

resources: <br />
  requests: <br />
    cpu: "0.5"  # any  <br />

There may be a few more changes but these are what you need to know. Will update when I remember other I changed. 
