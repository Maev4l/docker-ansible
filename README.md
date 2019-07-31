docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v \$PWD:/ansible/playbooks -v /Users/isnan/.ssh:/root/.ssh isnan/ansible:latest ansible-playbook --inventory hosts.yaml test.yaml
