  A couple of weeks ago, I saw a demo of Waypoint, a new tool Hashicorp announced that aims to provide an easy, intuitive and customizable “build, deploy and release” workflow for an application especially to on top of Kubernetes.


Before jumping right into what the tool is and how it works, I want to set a little bit of context for why developers need this tool particulary when then. 

Steps involved and things to care in the process of software Development:

1. Developers write code in the IDE of their choice

2. How do I test this application( commit code to git, run automated  tests in a CI system)

3. Now things start to get c
   hallenging, how do I deploy this app  and when we talk about deploy, there is really some sub-steps involved

4. How do I build my application (if it is a containerized app, how do I create a docker image and push it to the registry)?  I need to transform from source code into something that is runnable, I need to put that runnable artifact into a registry and keep track of it Before we jump right into what the tool is and how it works, I want to set a little bit of context for why developers need this.

5. I need to map the artifact that's been created to a runtime like Kubernetes, VM, serverless, etc…

6. How do I manage a release process for application to serve the user traffic (blue-green  or canary )

7. How do I measure what's happening (logs, metrics, tracing)

**Hashicorp Waypoint aims to be the interface for Devs to build, deploy, and manage applications.**


Getting Started with Hashicorp Waypoint to deploy apps to Kubernetes

I have already installed Waypoint Server on Kubernetes. 

Waypoint  Server HTTPS UI Address: https://<Waypoint IP address >
Waypoint Token: bM152PWkXxfoy4vA51JFhR7LnxkTG5FX1cNsRMr2wPiHUpAKweHEL8y8atzqFWwsott5tYPt1jb2gAWTGopxEdqHfHVUA84k1UCFa

Install Waypoint client to your pc by following this link https://learn.hashicorp.com/tutorials/waypoint/get-started-install

On Ubuntu linux
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install waypoint

Setting Waypoint context
waypoint context create -server-addr=<waypoint IP address>:9701  -server-auth-token=bM152PWkXxfoy4vA51JFhR7LnxkTG5FX1cNsRMr2wPiHUpAKweHEL8y8atzqFWwsott5tYPt1jb2gAWTGopxEdqHfHVUA84k1UCFa -server-tls-skip-verify=true  -server-require-auth=true  -set-default waypoint-snow-vmware-k8s-cluster
Context "waypoint-vmware-k8s-cluster" created.

Let’s deploy a sample nodejs application hosted in this repo.
git clone git@github.com:contentful/the-example-app.nodejs.git
Cloning into 'the-example-app.nodejs'...
remote: Enumerating objects: 339, done.
remote: Counting objects: 100% (339/339), done.
remote: Compressing objects: 100% (248/248), done.
remote: Total 23295 (delta 150), reused 191 (delta 88), pack-reused 22956
Receiving objects: 100% (23295/23295), 7.89 MiB | 2.82 MiB/s, done.
Resolving deltas: 100% (12695/12695), done.

Create a file named waypoint.hcl

project = "the-example-app.nodejs"
app "the-example-app.nodejs" {
# Build specifies how an application should be deployed. In this case,
# we'll build using a Dockerfile and keeping it in a local registry.
build {
use "docker" {}

        # Uncomment below to use a remote docker registry to push your built images.
        #
        registry {
           use "docker" {
             image = "proget.euse.local/proget/library/snowdesignsystem"
             tag   = "latest"
           }
         }

    }
deploy {
use "kubernetes" {
probe_path = "/"
}
}

release {
use "kubernetes" {
load_balancer = true
port          = 80
}
}
}


Initialize waypoint using the command


dilip@atlas:~$ waypoint init
✓ Configuration file appears valid
✓ Connection to Waypoint server was successful
✓ Project "the-example-nodejs.app" and all apps are registered with the server.
✓ Plugins loaded and configured successfully
✓ Authentication requirements appear satisfied.

Project initialized!

You may now call 'waypoint up' to deploy your project or
commands such as 'waypoint build' to perform steps individually.

Build, Deploy and Release this sample nodejs application With a Single command

dilip@snow:~$ waypoint up
✓ Tagging Docker image: waypoint.local/the-example-nodejs.app:latest =>
proget.euse.local/proget/library/the-example-nodejs.app:latest
✓ Pushing Docker image...
│ 27086a907872: Layer already exists
│ c41cda2429b7: Layer already exists
│ 174e334f3f46: Layer already exists
│ cbe6bbd0c86f: Layer already exists
│ ef5de533cb53: Layer already exists
│ a4c504f73441: Layer already exists
│ e8847c2734e1: Layer already exists
│ b323b70996e4: Layer already exists
│ latest: digest: sha256:94bec2f13b8205aa5de04e80b4a358a22bcda739899f4240d0c034016
│ 0a19655 size: 2968

» Deploying...
✓ Kubernetes client connected to https://10.20.0.44:6443 with namespace default
✓ Creating deployment...
-> Waiting for deployment...
-> Waiting on deployment to become available: 1/0/1
-> Deployment successfully rolled out!
» Releasing...
-> Initializing Kubernetes client...
-> Kubernetes client connected to https://10.20.0.44:6443 with namespace default
-> Preparing service...
-> Creating service...
-> Waiting for service to become ready...
-> Service is ready!
The deploy was successful! A Waypoint deployment URL is shown below. This
can be used internally to check your deployment and is not meant for external
traffic. You can manage this hostname using "waypoint hostname."
Release URL: http://10.20.1.4



You can notice that in the above steps, there is no YAML being created by you. Everything from pushing docker image to creating Kubernetes resources(deployment, services) are taken care of by  Waypoint Itself.














