This is an atomic application based on the nulecule specification. Kubernetes and native docker are currently the only supported providers. You'll need to run this from a workstation that has the atomic command and kubectl client that can connect to a kubernetes master.

It's a single container application based on the centos/registry image, but you can use your own.

## Prerequisite
You need to create storage directory for Docker Registry:
```bash
mkdir /var/lib/registry
```
In case you are using SELinux and kubernetes provider you shloud set SELinux context properly by:

```bash
chcon -R -t svirt_sandbox_file_t /var/lib/registry
```

## Option 1: Interactive

Run the image. It will automatically use kubernetes as the orchestration provider.

    $ [sudo] atomic run projectatomic/docker-registry-centos7-atomicapp

## Option 2: Unattended

1. Create the file `answers.conf` with these contents:

    This sets up the values for the two configurable parameters (registry_image and node_port) and indicates that kubernetes should be the orchestration provider.

        [registry]
        registry_image = centos/registry
        node_port = 30575

        [general]
        provider = kubernetes


1. Run the application from the current working directory

        $ [sudo] atomic run projectatomic/docker-registry-centos7-atomicapp

1. As an additional experiment, remove the kubernetes pod and change the provider to 'docker' and re-run the application to see it get deployed on native docker.

## Option 3: Install and Run

You may want to download the application, review the configuraton and parameters as specified in the Nulecule file, and edit the answerfile before running the application.

1. Download the application files using `atomic run IMAGE --mode fetch`

        [sudo] atomic run projectatomic/docker-registry-centos7-atomicapp --mode fetch

2. Rename `answers.conf.sample`

        mv answers.conf.sample answers.conf

3. Edit `answers.conf`, review files if desired and then run

        $ [sudo] atomic run projectatomic/docker-registry-centos7-atomicapp

## Test
You can push to this registry from docker client or try:
```bash 
        $ curl -G http://127.0.0.1:80/v2/
```
