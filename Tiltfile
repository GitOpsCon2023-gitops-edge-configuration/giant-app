SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='source-image-location')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='local-path')
NAMESPACE = os.getenv("NAMESPACE", default='default')
OUTPUT_TO_NULL_COMMAND = os.getenv("OUTPUT_TO_NULL_COMMAND", default=' > /dev/null ')

k8s_custom_deploy(
  'giant-microservice',
  apply_cmd="tanzu apps workload apply -f config/workload.yaml --debug --live-update" +
    " --local-path " + LOCAL_PATH +
    " --source-image " + SOURCE_IMAGE +
    " --namespace " + NAMESPACE +
    " --label apps.tanzu.vmware.com/has-tests-" +
    " --yes " +
    OUTPUT_TO_NULL_COMMAND + 
    " && kubectl get workload giant-microservice --namespace " + NAMESPACE + " -o yaml",
  delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes" ,
  deps=['pom.xml', './target/classes'],
  container_selector='workload',
  live_update=[
    sync('./target/classes', '/workspace/BOOT-INF/classes')
  ]
)

k8s_resource('giant-microservice', port_forwards=["8080:8080"],
  extra_pod_selectors=[{'carto.run/workload-name': 'giant-microservice', 'app.kubernetes.io/component':'run'}])
