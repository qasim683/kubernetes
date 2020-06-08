you can create Name space with impertive way by command

	$ kubectl create namespace dev

you can also create by yaml file by

	$ kubectl create -f namespace.yaml

for listing of name space you can use

	$ kubectl -n dev get pods,svc,deploy,rc,cm,secret

you can switch you current context 
 
	$ kubectl config-context $( kubectl config-current context) dev
