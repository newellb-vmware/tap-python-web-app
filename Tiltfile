LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')
OUTPUT_TO_NULL_COMMAND = os.getenv("OUTPUT_TO_NULL_COMMAND", default=' > /dev/null ')

allow_k8s_contexts('taplab')

k8s_custom_deploy(
    'tap-python-web-app',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --update-strategy replace --debug --live-update" +
               " --local-path " + LOCAL_PATH +
               " --namespace " + NAMESPACE +
               " --yes " +
               OUTPUT_TO_NULL_COMMAND +
               " && kubectl get workload tap-python-web-app --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=[''],
    container_selector='workload',
    live_update=[
      sync('.', '/workspace/'),
      run(
            'pip install -r /workspace/requirements.txt',
            trigger=['./workspace/requirements.txt']
        )
    ]
)

k8s_resource('tap-python-web-app', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'carto.run/workload-name': 'tap-python-web-app', 'app.kubernetes.io/component': 'run'}])
