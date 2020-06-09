				SECRETS WITH IMPERTIVE WAY

you can create secetes directly by command 

	$ kubectl create secret generic <secret-name> --from-literal=<key>=<value>

for example
	
	$ kubectl create secret generic app-secret --from-literal=db_host=mysql

2nd way is to create secret from file so create a file which containers secrets key value pairs
for example we create secret_file.txt and input some key value pairs and in command
 we will provide this secret-file.txt for example

	$ kubectl create secret generic my-db-secrets --from-file=secret_file.txt

				SECRETS WITH DECLRATIVE WAY

so for secrative way we have to create yaml file for secrtes for example secret-data.yaml

	apiVersion: v1
	kind: Secret
	metadata:
	  name: app-secret
	data:
	  DB_HOST: mysql
	  DB_USER: root
	  DB_PASSWORD: paswrd

you can encode values in data in base64 code with linux by these commands

	$echo -n "mysql" | base64
output 	bXlzcWw=

	$echo -n "root" | base64
output  cm9vdA==

	$echo -n "paswrd"| base64
output  cGFzd3Jk



so replace value of data with base64 code

	apiVersion: v1
        kind: Secret
        metadata:
          name: app-secret
        data:
          DB_HOST: bXlzcWw=
          DB_USER: cm9vdA==
          DB_PASSWORD: cGFzd3Jk




save the file and run this command

	$ kubectl create -f secret-data.yaml

for decoding these base64 code you can do with this command

        $echo -n "cGFzd3Jk" | base64 --decode

---------------------------------------------------------------------
	configure secetes in Pod

so use of these secrets are you have to provide these secrets in pod to get value of secrets
and use it so you can provide in pod defination yaml file for example

	apiVersion: v1
	kind: Pod
	metadata:
	  name: simple-webapp-color
	spec:
	  containers:
	  - name: simple-webapp-color
	    image: simple-webapp-color
	    ports:
	      - contianerPort: 8080
	    envFrom:
	      - secretRef:
		  name: app-secret

now create pod with $ kubectl create -f pod.yaml so data of secret will be available for pod.

as env 

	envFrom:
	  - secretRef:
	      name: app-secret

as single env

	env:
	  - name: DB_PASSWORD
	    valueFrom:
	      secretKeyRef:
	        name: app-secret
	        key: db_password

as volume

	volumes:
	- name: app-secret-volume
  	  secret:
	   secretName: app-secret

