AWS CLI 설치

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"   // 다운로드
yum install unzip  // unzip install
unzip awscliv2.zip  // install
sudo ./aws/install  && /usr/local/bin/aws --version  //  실행 및 정상 설치 확인


AWS configure 명령을 통한 사용자의 자격 증명(IAM에서 access key 정보 확인)
# aws configure
AWS Access Key ID [****************SSDO]: 
AWS Secret Access Key [****************1ZIa]: 
Default region name [ap-northeast-2]:
Default output format [json]:
확인 명령어 : aws sts get-caller-identity



KUBECTL설치
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"  // 다운로드
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"  // 정상 다운로드 확인
echo "$(cat kubectl.sha256) kubectl" | sha256sum --check  // 정상 다운로드 확인(kubectl: OK)
chmod +x kubectl  // 권한 변경
sudo mv kubectl /usr/local/bin/kubectl  // 환경변수
kubectl version --client  // 정상 설치 확인



EKSCTL 설치
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname  -s)_amd64.tar.gz" | tar xz -C /tmp  // 다운로드
sudo mv /tmp/eksctl /usr/local/bin  //  환경변수
eksctl version  // 정상 설치 확인



cloudshell에서 진행 시, EKSCTL만 설치하면 됨

 

yaml 파일을 이용한 cluster 구성(Node : EC2)
개인키(x.pem)를 Bastion에 다운로드 후  chmod 400 a.pem (권한 변경) → ssh-keygen -y -f a.pem > ~/.ssh/id_rsa.pub 명령을 통해 pub으로 변경
ssh-keygen -y -f /home/cloudshell-user/roy.pem > /home/cloudshell-user/roy.pu
cluster.yaml(하기 내용) 후 실행(eksctl create cluster -f cluster.yaml)

eks-ec2-cluster.yaml 

eks-fargate-cluster.yaml 

EKS cluster 내 보안 설정

container security 내 EKS - Add cluster
설정에 따라 overrides.yaml 수정됨

Bastion에서 helm 챠트를 이용하여 overrides.yaml 파일 참조 및 실행
helm 설치 : curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash


일정 시간 후 아래와 같이 연동 되었음을 확인 가능
확인 명령어 : kubectl cluster-info, kubectl get pods --all-namespaces





EKS CLUSTER 제거 명령어
eksctl delete cluster -f cluster.yaml  // 설치 시 사용했던 yaml
약 10여분 소요되며, 생성되었던 Node, VPC, eks-cluster 등 관련 리소스 모두 삭제됨
cloudwatch 내 log-group, IAM role은 수동 삭제 필요 
