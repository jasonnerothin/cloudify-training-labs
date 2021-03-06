# Lab 7: Deploying Docker Containers

**NOTE: If you currently have nodecellar installed on your manager, then uninstall it. Otherwise you will run into port conflicts.**

In this lab, we will demonstrate how to deploy and install a Docker container. We will use a Docker version of the Nodecellar application introduced in previous labs.

It is assumed that the `LAB_ROOT` environment variable points to the exercise's root directory. Otherwise, export it:

```bash
export LAB_ROOT=~/cloudify-training-labs/lab7/exercise
```

### Step 1: Edit `singlehost.yaml`

Edit the file `$LAB_ROOT/blueprint/singlehost.yaml` by replacing all strings beginning with `REPLACE_WITH` with correct values.

Some of the values you'll have to put in, are names of Docker images. Browse to [http://hub.docker.com](http://hub.docker.com) and look for those images.

### Step 2: Edit `inputs.yaml`

Edit the file `$LAB_ROOT/blueprint/inputs.yaml` to contain values applicable to your environment.

### Step 3: Upload the blueprint, create a deployment and install

```bash
cfy blueprints upload -p $LAB_ROOT/blueprint/singlehost.yaml -b nc-docker
cfy deployments create -b nc-docker -d nc-docker -i $LAB_ROOT/blueprint/inputs.yaml
cfy executions start -d nc-docker -w install
```

**NOTE**: if you get an error saying that the downloaded image could not be verified, please execute the `uninstall` workflow, delete the deployment
and try again. We are currently looking into this issue.

### Step 4: Verify installation

Navigate to port 8080 on the public IP that is associated with the machine on which Nodecellar was installed. For example:

```
http://<manager-machine-public-ip>:8080
```

You should be presented with the Nodecellar application.

### Step 5: Cleanup

```bash
cfy executions start -d nc-docker -w uninstall
cfy deployments delete -d nc-docker
cfy blueprints delete -b nc-docker
```