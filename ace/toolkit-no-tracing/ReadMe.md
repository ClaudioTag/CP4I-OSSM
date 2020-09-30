# ACE app test: toolkit-no-tracing
- Using `Transformation_Map` test service: https://github.com/ClaudioTag/CP4I-OSSM/blob/master/ace/testAPIs/Transformation_Map.bar
- Successfully deployed from the ACE Dashboard
- Confirmed that service cannot be invoked
- Successfully tested Istio sidecar injection to `toolkit-no-tracing` server by editing the deployment YAML manually: added line `sidecar.istio.io/inject: 'true'` to deployment in **spec > template > metadata > annotations**
- Created Istio Gateway: `toolkit-no-tracing-gateway.yaml`
- Created Virtual Service using the `istioselector` label: `toolkit-no-tracing-virtual-service.yaml`
- Created Destination Rule: `toolkit-no-tracing-destionation-rule.yaml`
- Confirmed succesfull configuration in Kiali:
![toolkit-no-tracing-kiali](https://github.com/ClaudioTag/CP4I-OSSM/blob/master/images/toolkit-no-tracing-kiali.png)
- Successfully tested flow: http://toolkit-no-tracing.icp4i-istio-2-de84486a60b3b0d17097c54f4549015a-0000.eu-gb.containers.appdomain.cloud/ping_test/v1/server
