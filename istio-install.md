# istioctl-install

---

[Getting Started](https://istio.io/latest/docs/setup/getting-started/)

홈페이지를 참고하여 설치 진행

[Install with Istioctl](https://istio.io/latest/docs/setup/install/istioctl/)

istioctl install

```jsx
curl -L https://istio.io/downloadIstio | sh -
cd istio-1.13.4
export PATH=$PWD/bin:$PATH

istioctl install

kubectl get all -n istio-system
NAME                                        READY   STATUS    RESTARTS   AGE
pod/istio-ingressgateway-76dcc86449-5x4s4   1/1     Running   0          62s
pod/istiod-7664dfcb67-468tr                 1/1     Running   0          68s

NAME                           TYPE           CLUSTER-IP      EXTERNAL-IP                                                              PORT(S)                                      AGE
service/istio-ingressgateway   LoadBalancer   172.20.18.81    []   15021:31966/TCP,80:32526/TCP,443:32228/TCP   62s
service/istiod                 ClusterIP      172.20.201.10   <none>                                                                   15010/TCP,15012/TCP,443/TCP,15014/TCP        68s

NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/istio-ingressgateway   1/1     1            1           62s
deployment.apps/istiod                 1/1     1            1           68s

NAME                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/istio-ingressgateway-76dcc86449   1         1         1       62s
replicaset.apps/istiod-7664dfcb67                 1         1         1       68s

NAME                                                       REFERENCE                         TARGETS         MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/istio-ingressgateway   Deployment/istio-ingressgateway   <unknown>/80%   1         5         1          62s
horizontalpodautoscaler.autoscaling/istiod                 Deployment/istiod                 <unknown>/80%   1         5         1          68s

istioctl profile dump default
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
spec:
  components:
    base:
      enabled: true
    cni:
      enabled: false
    egressGateways:
    - enabled: false
      name: istio-egressgateway
    ingressGateways:
    - enabled: true
      name: istio-ingressgateway
    istiodRemote:
      enabled: false
    pilot:
      enabled: true
  hub: docker.io/istio
  meshConfig:
    defaultConfig:
      proxyMetadata: {}
    enablePrometheusMerge: true
  profile: default
  tag: 1.13.4
  values:
    base:
      enableCRDTemplates: false
      validationURL: ""
    defaultRevision: ""
    gateways:
      istio-egressgateway:
        autoscaleEnabled: true
        env: {}
        name: istio-egressgateway
        secretVolumes:
        - mountPath: /etc/istio/egressgateway-certs
          name: egressgateway-certs
          secretName: istio-egressgateway-certs
        - mountPath: /etc/istio/egressgateway-ca-certs
          name: egressgateway-ca-certs
          secretName: istio-egressgateway-ca-certs
        type: ClusterIP
      istio-ingressgateway:
        autoscaleEnabled: true
        env: {}
        name: istio-ingressgateway
        secretVolumes:
        - mountPath: /etc/istio/ingressgateway-certs
          name: ingressgateway-certs
          secretName: istio-ingressgateway-certs
        - mountPath: /etc/istio/ingressgateway-ca-certs
          name: ingressgateway-ca-certs
          secretName: istio-ingressgateway-ca-certs
        type: LoadBalancer
    global:
      configValidation: true
      defaultNodeSelector: {}
      defaultPodDisruptionBudget:
        enabled: true
      defaultResources:
        requests:
          cpu: 10m
      imagePullPolicy: ""
      imagePullSecrets: []
      istioNamespace: istio-system
      istiod:
        enableAnalysis: false
      jwtPolicy: third-party-jwt
      logAsJson: false
      logging:
        level: default:info
      meshNetworks: {}
      mountMtlsCerts: false
      multiCluster:
        clusterName: ""
        enabled: false
      network: ""
      omitSidecarInjectorConfigMap: false
      oneNamespace: false
      operatorManageWebhooks: false
      pilotCertProvider: istiod
      priorityClassName: ""
      proxy:
        autoInject: enabled
        clusterDomain: cluster.local
        componentLogLevel: misc:error
        enableCoreDump: false
        excludeIPRanges: ""
        excludeInboundPorts: ""
        excludeOutboundPorts: ""
        image: proxyv2
        includeIPRanges: '*'
        logLevel: warning
        privileged: false
        readinessFailureThreshold: 30
        readinessInitialDelaySeconds: 1
        readinessPeriodSeconds: 2
        resources:
          limits:
            cpu: 2000m
            memory: 1024Mi
          requests:
            cpu: 100m
            memory: 128Mi
        statusPort: 15020
        tracer: zipkin
      proxy_init:
        image: proxyv2
        resources:
          limits:
            cpu: 2000m
            memory: 1024Mi
          requests:
            cpu: 10m
            memory: 10Mi
      sds:
        token:
          aud: istio-ca
      sts:
        servicePort: 0
      tracer:
        datadog: {}
        lightstep: {}
        stackdriver: {}
        zipkin: {}
      useMCP: false
    istiodRemote:
      injectionURL: ""
    pilot:
      autoscaleEnabled: true
      autoscaleMax: 5
      autoscaleMin: 1
      configMap: true
      cpu:
        targetAverageUtilization: 80
      enableProtocolSniffingForInbound: true
      enableProtocolSniffingForOutbound: true
      env: {}
      image: pilot
      keepaliveMaxServerConnectionAge: 30m
      nodeSelector: {}
      podLabels: {}
      replicaCount: 1
      traceSampling: 1
    telemetry:
      enabled: true
      v2:
        enabled: true
        metadataExchange:
          wasmEnabled: false
        prometheus:
          enabled: true
          wasmEnabled: false
        stackdriver:
          configOverride: {}
          enabled: false
          logging: false
          monitoring: false
          topology: false
```

설치가 완료된 모습.

custom해서 사용할 yaml

```jsx
istioctl manifest generate > $HOME/generated-manifest.yaml

istio가 설치된 디렉토리에 들어가서 작업
istioctl manifest generate > 1.yaml
istioctl manifest generate -f samples/operator/pilot-k8s.yaml > 2.yaml
istioctl manifest diff 1.yaml 2.yaml
```

istioctl 자동완성 기능
