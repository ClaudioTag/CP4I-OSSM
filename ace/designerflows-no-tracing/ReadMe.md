# ACE app test: designerflows-no-tracing
- Using `SimplAPI` test API: https://github.com/ClaudioTag/CP4I-OSSM/blob/master/ace/testAPIs/simpleAPI.bar
- Successfully deployed from the ACE Dashboard
- Confirmed that API cannot be invoked
- Successfully tested Istio sidecar injection to `designerflows-no-tracing` server by editing the deployment YAML manually: added line `sidecar.istio.io/inject: 'true'` to deployment in **spec > template > metadata > annotations**
- Created Istio Gateway: `designerflows-no-tracing-gateway.yaml`
- Created Virtual Service using the `istioselector` label: `designerflows-no-tracing-virtual-service.yaml`
- Created Destination Rule: `designerflows-no-tracing-destionation-rule.yaml`
- Confirmed succesfull configuration in Kiali:
![designerflows-no-tracing-kiali](https://github.com/ClaudioTag/CP4I-OSSM/blob/master/images/designerflows-no-tracing-kiali.png)
- Successfully pinged API on http://designerflows-no-tracing.icp4i-istio-2-de84486a60b3b0d17097c54f4549015a-0000.eu-gb.containers.appdomain.cloud/simpleAPI/developer/John
