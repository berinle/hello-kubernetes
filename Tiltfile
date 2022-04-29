SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='tanzu01.azurecr.io/tap-apps/hello-kubernetes-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='dev1')
allow_k8s_contexts('tap-demo')


k8s_custom_deploy(
    'hello-kubernetes',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload hello-kubernetes --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    container_selector='workload',
    live_update=[
      sync('.', '/workspaces'),
      # run('cd /app && npm install', trigger=['./package.json'),
    ]
)

k8s_resource('hello-kubernetes', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'hello-kubernetes'}])
