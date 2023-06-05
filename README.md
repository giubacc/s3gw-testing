# s3gw-testing

![License](https://img.shields.io/github/license/giubacc/s3gw-system-tests)
![Lint](https://github.com/giubacc/s3gw-system-tests/actions/workflows/lint.yaml/badge.svg)
![Nightly Tests](https://github.com/giubacc/s3gw-system-tests/actions/workflows/nightly-tests.yaml/badge.svg)

## Local setup

### Bootstrap

> **Before doing anything else**: ensure to execute the following command
> after the clone:

```shell
git submodule update --init --recursive
```

### Requirements

- Docker, Docker compose
- Helm
- k3d
- kubectl
- Go (1.20+)
- Ginkgo

### Build the s3gw's images

Before creating the `k3d-s3gw-acceptance` cluster,
you have to build the images:

```shell
make build-images
```

> **Be patient**: this will take long.

After the command completes successfully,
you will see the following images:

```shell
docker images
```

- `quay.io/s3gw/s3gw:{@TAG}`
- `quay.io/s3gw/s3gw-ui:{@TAG}`
- `quay.io/s3gw/s3gw-cosi-driver:{@TAG}`
- `quay.io/s3gw/s3gw-cosi-sidecar:{@TAG}`

Where `{@TAG}` is the evaluation of the following expression:

```bash
$(git describe --tags --always)
```

### Create the acceptance cluster

You create the `k3d-s3gw-acceptance` cluster with:

```shell
make acceptance-cluster-create
```

> **WARNING**: the command updates your `.kube/config` with the credentials of
> the just created `k3d-s3gw-acceptance` cluster and sets its context as default.

### Delete the acceptance cluster

```shell
make acceptance-cluster-delete
```

### Prepare the acceptance cluster

You prepare the acceptance cluster with:

```shell
make acceptance-cluster-prepare
```

This imports the pre-built s3gw's images into k3d and triggers
a deployment of needed resources.

### Deploy the s3gw-acceptance-0/s3gw-0 instance on the acceptance cluster

You deploy the `s3gw-acceptance-0/s3gw-0` instance in the acceptance
cluster with:

```shell
make acceptance-cluster-s3gw-deploy
```

Acceptance tests are **NOT** relying on the `s3gw-acceptance-0/s3gw-0` instance.

### Trigger tests on the acceptance cluster

```shell
make acceptance-test-install
```

## Release tests

When releasing a new s3gw version it is appropriate to run a series of
specialized tests dedicated to ensure installation and
upgrade correctness of s3gw the in the Kubernetes cluster.

## License

Copyright (c) 2022-2023 [SUSE, LLC](http://suse.com)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
