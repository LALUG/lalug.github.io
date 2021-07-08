# LaLUG website

This repository contains the LaLUG website based on jekyll and the jekyll-klise template. 


## Development environment using podman

This part describes how to setup a develpoment environment using podman to keep your main system clean from ruby dependencies. 
Install podman for your distribution: 

Debian 11: 
```bash
apt install podman
```
CentOS/Fedora: 
```bash
dnf install podman
```
or
```bash
yum install podman
```

Create a working directory:
```bash
mkdir -p lalug/bundle lalug/lalug-pod 
cd lalug 
git clone https://github.com/LALUG/lalug.github.io.git
```
create a file pod.sh with your favorite editor and safe the following script:
```bash
# Gemfile lock has to be owned by apache:apache
touch ./lalug-pod/Gemfile.lock
chmod a+w ./lalug-pod/Gemfile.lock

# Jekyll runs as User with id 1000
podman run -it --rm --name jekyll \
  -v ./lalug.github.io:/srv/jekyll:rw,slave,Z \
  -v ./bundle:/usr/local/bundle:rw,slave,Z \
  --publish 4000:4000 \
  -e JEKYLL_UID=1000 \
  -e JEKYLL_GID=1000 \
  docker.io/jekyll/jekyll:3.8.5 \
  jekyll serve --drafts
```
Make script executable:
```bash
chmod +x pod.sh 
```

Now you can start the pod using the ./pod.sh script and your system keeps clean from all the needed jekyll dependencies. 

Have fun.

## Installation

Run local server:

```bash
$ git clone https://github.com/piharpi/jekyll-klise.git
$ cd jekyll-klise
$ bundle install
$ bundle exec jekyll serve
```

Open `localhost:4000` in your browser. 


