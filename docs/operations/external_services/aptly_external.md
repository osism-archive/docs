# Aptly (external)

The setup for OSISM nodes relies on upstream deb mirrors, e.g. the ubuntu archive.
To allow installation in air-gapped environments, you can setup an own mirror with
aptly using OSISM pre-configured setup.

.. contents::
   :depth: 2

## Setup

At the moment, there is no official OSISM upstream mirror for all required apt packages.
In the future, you'll be able to consume <https://aptly.airgap.services.osism.tech/> as an
official OSISM upstream mirror. But you can also setup an aptly mirror on your own
infrastructure. Follow the steps below to get started.

### Requirements

- Some kubernetes cluster with internet access
- At least 4TB of storage (we still do no have an estimate on how much is really required, feel free to contribute your findings here)
- Helm
- Access from your OSISM nodes to this kubernetes cluster
- Optional but recommended: k9s

### Helm deployment

The helm chart deploys the following components:

- A modified nginx server to host the deb mirror
- A setup container inside the nginx pod to configure gpg
- A initial setup job to fill all mirrors
- A periodic cronjob to keep the mirrors updated

```sh
helm repo add osism https://osism.github.io/helm-charts
helm repo update osism
helm show values osism/aptly > aptly_values.yaml
# now edit the values file according to your needs
helm install --create-namespace --namespace aptly aptly osism/aptly --values aptly_values.yaml
# do NOT wait for the deployment to finish but rather jump to the section "Initial configuration after deplyoment"
```

### Helm configuration

Beside the obvious configuration parameters, here is a list of the most interesting ones:

- persistence.*.accessMode: has to be "ReadWriteMany"
- interval: When and how often the mirrors are updated. Only triggers if the initial job pod is finished.
- wrapper_env_vars:
  - sources_url: Config file which is consumed to create the upstream mirrors. An example is <https://raw.githubusercontent.com/osism/sbom/main/mirrors.yaml>, but you only have to have the "deb_mirrors" key.
  - merged_mirror_name: Name under which the mirror is accessible from the clients
  - release: Version of the operating system (NOT the mirrors itself)
  - password: The password you choose for the GPG key (see )

### Initial configuration after deployment

The deployment will get stuck on purpose.
There is an init container waiting for you to manually open a shell and perform the following commands:

```sh
gpg1 --gen-key
gpg1 --export --armor --output /opt/aptly/public/gpgkey OSISM
```

During the first command, you'll be prompted for some input. Use these steps to answer them:

- `<RETURN>`
- `<RETURN>`
- `<RETURN>`
- y
- OSISM
- `<RETURN>`
- `<RETURN>`
- O
- Super_s3cret
- Super_s3cret

Of course you can use a different name (and secret) than "OSISM".
Once you exportet the key, the init pod will finish and kick you out automatically.
The helm deployment will finish and start the init job to fill the mirrors.
Keep in mind, that you cannot change this key any more!

### Usage / Consumption

The gpg key for your repository will be available on https://your_aptly_cluster/gpgkey.
After importing this key, you can add https://your_aptly_cluster/mirror to your apt sources.
Keep in mind, that the name "mirror" can be changed in the helm chart.

## Adding and deleting mirrors

Addition or deletion of mirrors involves manual interaction as this cannot be automated easily via the wrapper script.

### Addition

1. Alter your config file (<https://raw.githubusercontent.com/osism/sbom/main/mirrors.yaml>) accordingly
2. Spawn a new pod of the `quay.io/osism/aptly-aptly` container image with a bash shell
3. Import all missing gpg keys: `curl -sL '{gpg_url}' | gpg1 --no-default-keyring --keyring trustedkeys.gpg --import`
4. Create a mirror: `aptly mirror create mirror_00000X "https://mirror/ focal stable"`
5. Update the mirror: `aptly mirror update mirror_00000X`
6. Create a snapshot of the mirror: `aptly snapshot create snapshot_00000X from mirror mirror_00000X`
7. Merge all mirrors into a new mirror: `aptly snapshot merge mirror-new mirror_000001 mirror_000002 (...) mirror_00000X`
8. Publish it `aptly publish switch -passphrase='PASSWORD' jammy mirror mirror-new`
9. Cleanup: `aptly snapshot drop mirror; aptly snapshot rename mirror-new mirror`

### Deletion

1. Alter your config file (<https://raw.githubusercontent.com/osism/sbom/main/mirrors.yaml>) accordingly
2. Wait for the next automatic update
3. Spawn a new pod of the `quay.io/osism/aptly-aptly` container image with a bash shell
4. Drop the snapshot: `aptly snapshot drop snapshot_00000X`
5. Drop the mirror `aptly mirror drop mirror_00000X`

## Maintenance

This is not needed at the moment.

## Additional notes

Aplty has been wrapped by a python script. Due to the huge amount of log messages aptly produces,
we do not recommend to store them in the container itself. This is also the reason, why
they are only streamed to stdout.

It is currently not possible to provide initial gpg keys. Though, you can create the PersistentVolumeClaime
upfront and create the gpg keys there.

Aptly does not work very well with gnupg2, but gpg1 is still fine and maintained. We use gpg1.

On deletion of the helm chart, the data is NOT deleted. You have to do this manually.
