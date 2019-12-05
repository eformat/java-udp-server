# java-udp
code from:
https://systembash.com/a-simple-java-udp-server-and-udp-client/


```
oc new-project java-udp-server
oc new-app --strategy=source redhat-openjdk18-openshift:1.5~https://github.com/eformat/java-udp-server --context-dir=/
oc delete svc java-udp-server

echo "hey" | nc -4u -w1 localhost 30600

--
NodePort (non cloud), LoadBalancer (cloud)

cat <<EOF | oc create -f -
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  name: java-udp-server
spec:
  ports:
    - name: broker-amq-tcp
      nodePort: 30600
      port: 30600
      protocol: UDP
      targetPort: 30600
  selector:
    deploymentConfig: java-udp-server
  sessionAffinity: None
  type: NodePort
status:
  loadBalancer: {}
EOF

```