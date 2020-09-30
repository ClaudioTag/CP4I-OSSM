# ACE app test: toolkit-tracing
- Using `Transformation_Map` test service: https://github.com/ClaudioTag/CP4I-OSSM/blob/master/ace/testAPIs/Transformation_Map.bar
- Successfully deployed from the ACE Dashboard
- Confirmed that service cannot be invoked
- Successfully tested Istio sidecar injection to `toolkit-tracing` server by editing the deployment YAML manually: added line `sidecar.istio.io/inject: 'true'` to deployment in **spec > template > metadata > annotations**
- Created Istio Gateway: `toolkit-tracing-gateway.yaml`
- Created Virtual Service using the `istioselector` label: `toolkit-tracing-virtual-service.yaml`
- Created Destination Rule: `toolkit-tracing-destionation-rule.yaml`
- Successfully tested the API using a Postman test passing the `X-ABTEST:TEST` header: https://github.com/ClaudioTag/CP4I-OSSM/blob/master/ace/testAPIs/Istio.postman_collection.json
- Confirmed that tracing is working in the Operations Dashboard:
![pingtest-tracing](https://github.com/ClaudioTag/CP4I-OSSM/blob/master/images/pingtest-tracing.png)
