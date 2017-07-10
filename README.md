1. Install kubectl

> $ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl

2. Install minikube

> $ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.20.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

3.
> $ minikube start

> $ kubectl config use-context minikube

> $ eval $(minikube docker-env)

> $ cd drkiq

> $ docker build -t drkiq:v1 .

4. Install kompose
> $ curl -L https://github.com/kubernetes-incubator/kompose/releases/download/v0.7.0/kompose-linux-amd64 -o kompose

> $ chmod +x kompose

> $ sudo mv ./kompose /usr/local/bin/kompose

5. Convert docker-compose.yml to yaml files
> $ kompose convert -f docker-compose.yaml

6.

	> $ kubectl run drkiq --image=drkiq --port=8000

	> $ kubectl get pods
	NAME                     READY     STATUS             RESTARTS   AGE
	drkiq-2256726683-bt874   0/1       CrashLoopBackOff   1          6m
	
	> $ kubectl logs drkiq-2256726683-bt874
	/usr/local/bundle/gems/unicorn-4.9.0/lib/unicorn/configurator.rb:597:in `set_int': too low (< 1): worker_processes=0 (ArgumentError)
		from /usr/local/bundle/gems/unicorn-4.9.0/lib/unicorn/configurator.rb:212:in `worker_processes'
		from config/unicorn.rb:6:in `reload'
		from /usr/local/bundle/gems/unicorn-4.9.0/lib/unicorn/configurator.rb:75:in `instance_eval'
		from /usr/local/bundle/gems/unicorn-4.9.0/lib/unicorn/configurator.rb:75:in `reload'
		from /usr/local/bundle/gems/unicorn-4.9.0/lib/unicorn/configurator.rb:68:in `initialize'
		from /usr/local/bundle/gems/unicorn-4.9.0/lib/unicorn/http_server.rb:100:in `new'
		from /usr/local/bundle/gems/unicorn-4.9.0/lib/unicorn/http_server.rb:100:in `initialize'
		from /usr/local/bundle/gems/unicorn-4.9.0/bin/unicorn:126:in `new'
		from /usr/local/bundle/gems/unicorn-4.9.0/bin/unicorn:126:in `<top (required)>'
		from /usr/local/bundle/bin/unicorn:16:in `load'
		from /usr/local/bundle/bin/unicorn:16:in `<main>'

7. Notes:
	- It appears that environment variables aren't passed, it reads worker_processes=0
	- Followed were many trials to pass the variables.

		-- Defined a Pod.

		-- Defined a secret.

		-- Tried using configMaps.
	- Both trials failed with the same error.
	- Tried $ kubectl create -f file.yaml (for each yaml file created in step 5) since I doubted they aren't read automatically by a simple run.
	- New errors appeared indicating syntax errors with some .yaml files.
