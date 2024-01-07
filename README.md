deploy:
    stage:deploy
	script:
		- 'which ssh-agent || (apt-get update -y && apt-get install openssh-client wget curl -y)'
		- eval $(ssh-agent -s)
		- echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add - > /dev/null
		- mkdir -p ~/.ssh
		- chmod 700 ~/.ssh
		- '[[ -f /.dockerenv ]] && echo -e "Host *\n\tStrictHostKeyChecking no\n\n" > ~\.ssh/config'
		- cd ./dist && tar --warning='no-file-changed'
		- scp -r checkout.tar root@$HOST_IP:$WORK_DIR
		- ssh root@$HOST_IP "cd $WORK_DIR &&
					tar -xf checkout.tar &&
					rm checkout.tar"


#Deployment PipeLine
#Prodasd
