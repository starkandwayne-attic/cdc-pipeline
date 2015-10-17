# CDC Pipeline

In our environment we have a Concourse pipeline that we are deploying via another Concourse pipeline. These two Concourses must be identical in every way other than their IP ranges and should be self-updating.

Note that the example files in this repository are for a basic Concourse configuration that uses a deployment key and is in a vSphere environment.

### Setup

To play around with this example you will need:
* `spruce`: A replacement for `spiff`, the latest release can be downloaded [here](https://github.com/geofffranks/spruce/releases).
* `fly`: Downloaded from a deployed Concourse
* A project repo with a deployment key

#### How to Create and Add a Deployment Key (in Github)

1. Generate the deployment key using `ssh-keygen`, e.g. `ssh-keygen -t rsa -b 4096`. Name your key (e.g. `deploy-key`), do not specify a password.
1. Add the public deployment key to Github by going to the deployment repository -> Settings -> Deploy keys.
 * If you have a Mac, you can copy the key using `pbcopy`: `cat deploy-key.pub | pbcopy`
1. Add the private deployment key to the `credentials.yml` file under `pipeline-private-key`.
 * `cat deploy-key | pbcopy`
 * Keep the `|` as this allows you to supply a multiline value for `pipeline-private-key`.

### How to Make the Manifests

Before creating/updating the manifests, please ensure that you:

* Update the information in `meta` for both `alpha-concourse.yml` and `beta-concourse.yml`
* Update the `network` information for both `alpha-concourse.yml` and `beta-concourse.yml`

In order to make the manifests run the following from the `cdc-pipeline` directory:

```
./bin/make-manifests
```

**Please be mindful that this was done for vSphere**

Currently this example is using vSphere manifests in `environment`. Please be mindful that you may need to make additional changes to your concourse manifests if you are using a different environment, e.g. AWS. We are planning to add templates for other environments at a later time.

### How to Deploy the Pipeline

Before creating/deploying the pipelines, please ensure that you:

* Update `credentials.yml`

No other changes should be needed.

In order to deploy the pipelines run the following from the `cdc-pipeline` directory:

```
./bin/deploy-pipelines
```
