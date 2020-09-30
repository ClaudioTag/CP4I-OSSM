# ACE app test: designerflows-tracing
- Using `SimplAPI` test API: https://github.com/ClaudioTag/CP4I-OSSM/blob/master/ace/testAPIs/simpleAPI.bar
- Successfully deployed from the ACE Dashboard
- Confirmed that API cannot be invoked
- Successfully tested Istio sidecar injection to `designerflows-tracing` server by editing the deployment YAML manually: added line `sidecar.istio.io/inject: 'true'` to deployment in **spec > template > metadata > annotations**
- Created Istio Gateway: `designerflows-tracing-gateway.yaml`
- Created Virtual Service using the `istioselector` label: `designerflows-tracing-virtual-service.yaml`
- Created 2 Destination Rules: `designerflows-tracing-destionation-rule.yaml`
- Successfully tested the API using a Postman test passing the `X-ABTEST:TEST` header: https://github.com/ClaudioTag/CP4I-OSSM/blob/master/ace/testAPIs/Istio.postman_collection.json
- Confirmed that tracing is working in the Operations Dashboard:
![simpleAPI-tracing](https://github.com/ClaudioTag/CP4I-OSSM/blob/master/images/simpleAPI-tracing.png)
- Kiali picks traffic flowing out of the ACE server (to the tracing sidecars)
![simpleAPI-kiali-tracing](https://github.com/ClaudioTag/CP4I-OSSM/blob/master/images/simpleAPI-kiali-tracing.png)
