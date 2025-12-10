# Kubernetes-Cluster-EC2-Ubuntu
# Kubernetes Cluster on AWS EC2 Instance - Ubuntu OS
# STEP1: Create one instance for Master Node and atleast one instance for Worker Nodes: Make sure instance have atleate 2vcpu and 4GB RAM
# STEP2: Execute 3 Security group script in Cloud Shell and attach to all your cluster instances

VPC_ID="vpc-xxxxxxxxxx"

aws ec2 create-security-group \
    --group-name k8s-cluster-sg \
    --description "Kubernetes cluster security group" \
    --vpc-id $VPC_ID

SG_ID=$(aws ec2 describe-security-groups --filters "Name=group-name,Values=k8s-cluster-sg" --query 'SecurityGroups[0].GroupId' --output text)

aws ec2 authorize-security-group-ingress --group-id $SG_ID --ip-permissions '[{"IpProtocol":"tcp","FromPort":22,"ToPort":22,"IpRanges":[{"CidrIp":"0.0.0.0/0"}]},{"IpProtocol":"tcp","FromPort":6443,"ToPort":6443,"IpRanges":[{"CidrIp":"0.0.0.0/0"}]},{"IpProtocol":"tcp","FromPort":2379,"ToPort":2380,"UserIdGroupPairs":[{"GroupId":"'$SG_ID'"}]},{"IpProtocol":"tcp","FromPort":10250,"ToPort":10250,"UserIdGroupPairs":[{"GroupId":"'$SG_ID'"}]},{"IpProtocol":"tcp","FromPort":10257,"ToPort":10257,"UserIdGroupPairs":[{"GroupId":"'$SG_ID'"}]},{"IpProtocol":"tcp","FromPort":10259,"ToPort":10259,"UserIdGroupPairs":[{"GroupId":"'$SG_ID'"}]},{"IpProtocol":"tcp","FromPort":30000,"ToPort":32767,"IpRanges":[{"CidrIp":"0.0.0.0/0"}]},{"IpProtocol":"udp","FromPort":8472,"ToPort":8472,"UserIdGroupPairs":[{"GroupId":"'$SG_ID'"}]}]'

# STEP3: Paste Master Node Script in Master Node and Worker Node Script in Worker Node + chmod +x scriptname.sh
# STEP4: Run this commands one by one in all your cluster instances: sudo apt-get update and sudo apt-get install -y conntrack
# STEP5: Execute the ./master.sh in Master Node Instance and /worker.sh script in Worker Node Instance
# STEP6: Generate Token in master node and join token in worker node 

# Run this command in Master Node and Copy Paste the join command with below command:   sudo kubeadm token create --print-join-command
# After that run this command in Worker Node:                                                 sudo <paste-the-join-command> --cri-socket unix:///var/run/cri-dockerd.sock 

# Use ChatGPT to resolve the issue if any. 
# Refer to the AWS Cloud Controller Notes to configure it. 
