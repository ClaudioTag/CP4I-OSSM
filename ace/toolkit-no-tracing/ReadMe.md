# ACE app test: toolkit-no-tracing
- Use `Transformation_Map` test service: https://github.com/ClaudioTag/CP4I-OSSM/blob/master/ace/testAPIs/Transformation_Map.bar
- Deploy from the ACE Dashboard
- Confirm that service cannot be invoked
- Enable Istio sidecar injection to `toolkit-no-tracing` server by editing the deployment YAML manually: add line `sidecar.istio.io/inject: 'true'` to deployment in **spec > template > metadata > annotations**
- Create Istio Gateway: `toolkit-no-tracing-gateway.yaml`
- Create Virtual Service using the `istioselector` label: `toolkit-no-tracing-virtual-service.yaml`
- Create Destination Rule: `toolkit-no-tracing-destionation-rule.yaml`
- Confirm configuration in Kiali:
![toolkit-no-tracing-kiali](https://github.com/ClaudioTag/CP4I-OSSM/blob/master/images/toolkit-no-tracing-kiali.png)
- Test flow: http://toolkit-no-tracing.icp4i-istio-2-de84486a60b3b0d17097c54f4549015a-0000.eu-gb.containers.appdomain.cloud/ping_test/v1/server
