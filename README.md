sudo mkdir -p /var/lib/jenkins/.kube
sudo cp ~/.kube/config /var/lib/jenkins/.kube/config
sudo chown -R jenkins:jenkins /var/lib/jenkins/.kube



# for image with {{build}}
--set frontend.image.repository=${FRONTEND_IMAGE} \
--set frontend.image.tag=${BUILD_NUMBER} \
--set backend.image.repository=${BACKEND_IMAGE} \
--set backend.image.tag=${BUILD_NUMBER}
  # delete
  export KOPS_STATE_STORE=s3://kop-deployment-bymaji
   kubectl get nodes -o wide
   42  helm uninstall todo-app
   43  kubectl get all
   44  kubectl delete pods --all
   45  kubectl delete svc --all
   46  kops delete cluster --name majitha.k8s.local --yes
   47  aws s3 ls s3://kop-deployment-bymaji#####
   48  aws s3 rm s3://kop-deployment-bymaji --recursive
   49  kops delete cluster --name majitha.k8s.local --yes
   50  export KOPS_STATE_STORE=s3://kop-deployment-bymaji
   51  kops delete cluster --name majitha.k8s.local --yes
   52  kops get cluster
   53  history