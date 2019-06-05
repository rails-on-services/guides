
Right now this is just a dumping area for stuff
### 5. Boot all servers using docker-compose

<!---
[x]Dockerfile/compose in gems which mounts the current directory
[x]no puma, just rails s -b 0.0.0.0; puma is in the enclosing app's Gemfile
[x]nginx is used to so that multiple services are available at the same port number
[x]Files: Dockerfile, docker-compose.yml, nginx.conf, settings.yml?
[x]dependent gems in dockerfile: use git instead of path, commit code to git, use bundle local for development, but dockerfile pulls from GH
[]get this thing deployed into k8s using skaffold
[]add a .env file for the version number of the image, etc. push to ros docker hub
--->
# aws eks update-kubeconfig --name production --role-arn arn:aws:iam::814127428874:role/eks-admin
  3 # name-db.subdomain.domain
  4 # api-db.whistler.perxtech.net
  5 # pru-api-db.whistler.perxtech.net
  6 # pru-api-redis.whistler.perxtech.net
