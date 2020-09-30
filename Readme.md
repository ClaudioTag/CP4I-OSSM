# Red Hat OpenShift Service Mesh and Cloud Pak for Integration

- An installation of Red Hat OpenShift Service Mesh differs from upstream Istio community installations in multiple ways: https://docs.openshift.com/container-platform/4.3/service_mesh/service_mesh_arch/ossm-vs-community.html#ossm-vs-community
- OCP uses an opinionated version of Istio called Maistra: https://maistra.io/

## Cluster set-up

Provision an OpenShift 4.3 cluster with Cloud Pak for Integration 2020.1 installed.

## Service Mesh set-up

### Service Mesh Installation
- Refer to instructions at: https://docs.openshift.com/container-platform/4.3/service_mesh/service_mesh_install/installing-ossm.html
- Versions:
  - Red Hat Service Mesh 1.1.3
  - Istio 1.4.8
  - Jaeger 1.17.3
  - Kiali 1.12.13
  - 3scale Istio adapter 1.0.0
  - Elasticsearch 4.3.23
- Starting with Red Hat OpenShift Service Mesh 1.1.3, you must install the Elasticsearch Operator, the Jaeger Operator, and the Kiali Operator before the Red Hat OpenShift Service Mesh Operator can install the control plane.
- Installed operators from OperatorHub
  - Install Elasticsearch successfully
  - Install the Red Hat version of Jaeger (not community) successfully
  - Install the Red Hat version of Kiali (not community) successfully
  - Install Red Hat OpenShift Service Mesh
- Create a service mesh control plane in the `istio-system` namespace from the Installed Operators tab, and using the basic yaml install without customisation: `servicemeshcontrolplane-install-01`
- Create a Service Mesh Member Roll called `default`, with an empty project (`istio-test-ns-01`) added to the members:
```
apiVersion: maistra.io/v1
kind: ServiceMeshMemberRoll
metadata:
  name: default
  namespace: istio-system
spec:
  members:
    - istio-test-ns-01
```
- Once the Service mesh is successfully deployed, it generates by default network policies which prevent accessing pods via the OpenShift Routes

### Service Mesh Configuration
- Enable automatic route creation (IOR): https://docs.openshift.com/container-platform/4.3/service_mesh/service_mesh_day_two/ossm-auto-route.html
  - In the Service Mesh Control plane change `ior_enabled` from `false` to `true`:
  ```
  ior_enabled: true
  ```
- Follow instructions at: https://docs.openshift.com/container-platform/4.3/service_mesh/service_mesh_day_two/prepare-to-deploy-applications-ossm.html
- Using the `default` template: no ConfigMap created
- Add line: `sidecar.istio.io/inject: 'true'` to test deployment in **spec > template > metadata > annotations**
- Chang mixer policy enforcement from `disablePolicyChecks: true`  to `disablePolicyChecks: false`.
- Test that Istio injection works for selected deployments

## Applications

### Bookinfo
- Refer to: https://github.com/ClaudioTag/CP4I-OSSM/tree/master/bookinfo

### ACE dashboard
The ACE Dashboard doesn't have a dedicated network policy, like the Asset Repository and ACE Designer. This means that when the `ace` namespace gets onboarded into the mesh, the `istio-mesh` network policy will block access to the ACE Dashboard.
- To enable access to the ACE Dashboard, deploy the policy `ace-dashboard-network-policy-icp4i` **before** onboarding the `ace` namespace in the mesh: https://github.com/ClaudioTag/CP4I-OSSM/blob/master/ace/ace-dashboard/ace-dashboard-network-policy-icp4i.yaml
- Policies take precedence based on creation time. If the `ace-dashboard-network-policy-icp4i` network policy is deployed **after** the `ace` namespace in onboarded in the mesh, it won't take effect until a change is made to the `istio-mesh` network policy: any change would recreate `istio-mesh` and give precedence to `ace-dashboard-network-policy-icp4i`.

### ACE Designer
- No changes needed for ACE Designer, which will still be accessible because the chart deploys a Network policy which overrides the ones created by the service mesh.

### ACE Server
- Add the `ace` namespace to the ServiceMeshMemberRoll, which creates two new network policies which prevent access to anything in that namespace:
![Istio netowrk policies](https://github.com/ClaudioTag/CP4I-OSSM/blob/master/images/Istio-network-policies.png)
- Use 4 ACE servers types in the `ace` namespace, for testing purposes:
  - `toolkit-no-tracing` is a 1-replica deployment, with no OD and Designer sidecars
  - `toolkit-tracing` is a 1-replica deployment, with OD sidecars, but no Designer sidecars
  - `designerflows-no-tracing` is a 1-replica deployment, with Designer sidecars, but no OD sidecars
  - `designerflows-tracing` is a 1-replica deployment, with both OD and Designer sidecars
- Detailed instructions available here: https://github.ibm.com/claudio-tag/Istio-PoC/tree/master/ace

![working-configuration](https://github.com/ClaudioTag/CP4I-OSSM/blob/master/images/working-configuration-kiali.png)
