SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='harbor.dorn.tkg-aws-e2-lab.winterfell.katirap.xyz/tap/tanzu-java-web-app-default-bundle')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='dev')
allow_k8s_contexts('gke_pa-sincandela_europe-west3-a_tap')

k8s_custom_deploy(
    'tanzu-java-web-app',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload tanzu-java-web-app --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('tanzu-java-web-app', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'tanzu-java-web-app'}])
