SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='tanzu01.azurecr.io/tap-apps/hello-k8s-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='dev1')
allow_k8s_contexts('tap-demo')


k8s_custom_deploy(
    'hello-k8s',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload hello-k8s --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    #deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('.', '/app'),
      # run('cd /app && npm install', trigger=['./package.json'),
    ]
)

k8s_resource('hello-k8s', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'hello-k8s'}])
