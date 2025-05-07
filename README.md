# zero-trust-workload-identity-manager
The Zero Trust Workload Identity Manager is a Openshift Day-2 Operator designed to automate the setup and management of SPIFFE/SPIRE components (like `spire-server`, `spire-agent`, `spiffe-csi-driver`, and `oidc-discovery-provider`) within OpenShift clusters. It enables zero-trust security by dynamically issuing and rotating workload identities, enforcing strict identity-based authentication across workloads. This manager abstracts complex SPIRE configurations, streamlining workload onboarding with secure identities and improving the cluster's overall security posture.

## Description
Zero Trust security is rapidly becoming essential in cloud-native environments, where workloads are often ephemeral, multi-tenant, and dynamically scheduled. Traditional IP or network-based security mechanisms fall short in such setups.

This project introduces an operator that integrates SPIFFE and SPIRE with openshift to provide secure, verifiable identities to workloads. By leveraging SPIRE's identity issuing and attestation mechanisms, the operator:

- Automates deployment and lifecycle management of SPIRE components.

- Manages workload registration entries and identity policies.

- Enables integration with external services via SPIFFE JWT-SVIDs or X.509-SVIDs.

- Supports dynamic certificate issuance and rotation using the CSI driver.

The zero-trust-workload-identity-manager simplifies onboarding applications with zero-trust identities using Kubernetes CRDs and manages their security lifecycle through custom controllers.

## Getting Started

### Prerequisites
- go version v1.23.0+
- docker version 17.03+.
- kubectl version v1.11.3+.
- Access to a Openshift v4.19+ cluster.

### To Deploy on the cluster
**Build and push your image to the location specified by `IMG`:**

```sh
make docker-build docker-push IMG=<some-registry>/zero-trust-workload-identity-manager:tag
```

**NOTE:** This image ought to be published in the personal registry you specified.
And it is required to have access to pull the image from the working environment.
Make sure you have the proper permission to the registry if the above commands don’t work.

**Install the CRDs into the cluster:**

```sh
make install
```

**Deploy the Manager to the cluster with the image specified by `IMG`:**

```sh
make deploy IMG=<some-registry>/zero-trust-workload-identity-manager:tag
```

> **NOTE**: If you encounter RBAC errors, you may need to grant yourself cluster-admin
privileges or be logged in as admin.

**Create instances of your solution**
You can apply the samples (examples) from the config/sample:

```sh
kubectl apply -k config/samples/
```

>**NOTE**: Ensure that the samples has default values to test it out.

### To Uninstall
**Delete the instances (CRs) from the cluster:**

```sh
kubectl delete -k config/samples/
```

**Delete the APIs(CRDs) from the cluster:**

```sh
make uninstall
```

**UnDeploy the controller from the cluster:**

```sh
make undeploy
```

## Project Distribution

Following are the steps to build the installer and distribute this project to users.

1. Build the installer for the image built and published in the registry:

```sh
make build-installer IMG=<some-registry>/zero-trust-workload-identity-manager:tag
```

NOTE: The makefile target mentioned above generates an 'install.yaml'
file in the dist directory. This file contains all the resources built
with Kustomize, which are necessary to install this project without
its dependencies.

2. Using the installer

Users can just run kubectl apply -f <URL for YAML BUNDLE> to install the project, i.e.:

```sh
kubectl apply -f https://raw.githubusercontent.com/<org>/zero-trust-workload-identity-manager/<tag or branch>/dist/install.yaml
```

## Contributing
We welcome contributions from the community! To contribute:

- Fork this repository and create a new branch.

- Make your changes and test them thoroughly.

- Run make targets to verify the behavior.

- Submit a Pull Request describing your changes and the motivation behind them.

- Run make help to view all available development targets.

We appreciate issues, bug reports, feature requests, and feedback!