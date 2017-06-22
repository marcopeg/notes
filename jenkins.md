Jenkins
==========================

[Digital Ocean Tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-16-04)

	# start / stop Jenkins daemon
	sudo systemctl start jenkins
	sudo systemctl stop jenkins

	# Check which user is jenkins using
	ps axufwwww | grep 'jenkins\|java' -


[Setup Jenkins in Ubuntu](https://gist.github.com/ostinelli/b77c20d91e4e33507813)  
the bottom line is that jenkins runs on the "jenkins" user.

## Run Remote SSH

https://wiki.jenkins-ci.org/display/JENKINS/SSH+plugin
(this shit does not work)

http://wiki.jenkins-ci.org/display/JENKINS/Publish+Over+SSH+Plugin
with this one it worked but i can't understand how to add more than one certificate

I managed to git pull and remote publish a set of assets:

```
EC2KEY=/var/lib/jenkins/ssh-keys/aws.pem
EC2DNS=ec2-34-253-137-82.eu-west-1.compute.amazonaws.com
EC2USR=ubuntu
EC2CWD=/home/ubuntu/hello-world-static
EC2OPT=-oStrictHostKeyChecking=no

ssh $EC2OPT -i $EC2KEY "$EC2USR"@"$EC2DNS" "rm -rf $EC2CWD/*; bash"
scp $EC2OPT -i $EC2KEY -r ./src/. "$EC2USR"@"$EC2DNS":$EC2CWD
```

## Docker in Jenkins

You have to add the	`jenkins` user to the `docker` group and then **restart jenkins** (or reboot the machine) for this setting to take effect.

```
sudo usermod -a -G docker jenkins
```


## Development build

```
# Build and push the image
VERSION_NUMBER=$(head -n 1 "./VERSION")
VERSION_NUMBER=${VERSION_NUMBER:-"latest"}
VERSION_TAG="registry.24hr.se/hello-world-static:dev"

docker build -t "$VERSION_TAG" ./services/web
docker push "$VERSION_TAG"

# Deploy to production
EC2KEY=/var/lib/jenkins/ssh-keys/aws.pem
EC2DNS=ec2-34-253-137-82.eu-west-1.compute.amazonaws.com
EC2USR=ubuntu
EC2STR="-oStrictHostKeyChecking=no -i $EC2KEY $EC2USR""@""$EC2DNS"
EC2CNAME="hello-world-static-dev"

ssh $EC2STR "docker pull $VERSION_TAG; bash"
ssh $EC2STR "docker stop $EC2CNAME; bash"
ssh $EC2STR "docker run --rm -d --name $EC2CNAME -p 1081:80 $VERSION_TAG; bash"
```

## Production build

```
# Build and push the image
#VERSION_NUMBER=$(head -n 1 "./VERSION")
#VERSION_NUMBER=${VERSION_NUMBER:-"latest"}
VERSION_NUMBER=$(git symbolic-ref -q --short HEAD || git describe --tags --exact-match)
VERSION_TAG="registry.24hr.se/hello-world-static:$VERSION_NUMBER""-prod"

docker build -t "$VERSION_TAG" ./services/web
docker push "$VERSION_TAG"

# Deploy to production
EC2KEY=/var/lib/jenkins/ssh-keys/aws.pem
EC2DNS=ec2-34-253-137-82.eu-west-1.compute.amazonaws.com
EC2USR=ubuntu
EC2STR="-oStrictHostKeyChecking=no -i $EC2KEY $EC2USR""@""$EC2DNS"
EC2CNAME="hello-world-static-prod"

ssh $EC2STR "docker stop $EC2CNAME; bash"
ssh $EC2STR "docker run --rm -d --name $EC2CNAME -p 1080:80 $VERSION_TAG; bash"
```
