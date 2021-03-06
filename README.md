<img align="right" src="https://layer5.io/assets/images/cube-sh-small.png" />

# [Meshery](https://layer5.io/meshery)

A service mesh playground to faciliate learning about functionality and performance of different service meshes. [Meshery](https://layer5.io/meshery) incorporates the collection and display of metrics from applications running in the playground.

- [Functionality](#functionality)
- [Running Meshery](#running)
- [Architecture](https://docs.google.com/presentation/d/1UbuYMpn-e-mWVYwEASy4dzyZlrSgZX6MUfNtokraT9o/edit?usp=sharing)
  - [Design document](https://docs.google.com/document/d/1nV8TunLmVC8j5cBELT42YfEXYmhG3ZqFtHxeG3-w9t0/edit?usp=sharing)
- [Contributing](CONTRIBUTING.md/#contributing)
  - [Write an adapter](CONTRIBUTING.md/#adapter)
  - [Build the project](CONTRIBUTING.md/#building)
  
In an effort to produce service mesh agnostic tooling, Meshery uses a [common performance benchmark specification](https://github.com/layer5io/service-mesh-benchmark-spec) to capture and share environment information and test configuration. 

## <a name="functionality">Functionality</a>
<img align="right" src="./public/static/img/meshery.png?raw=true" alt="Service Mesh Playground" width="50%" />

1. Multi-mesh Performannce Benchmark
Meshery is intended to be a vendor and project-neutral utility for uniformly benchmarking the performance of service meshes. Between service mesh and proxy projects, a number of different tools *and results* exist. For example, Istio's [Performance and Scalability WG](https://github.com/istio/community/blob/master/WORKING-GROUPS.md#performance-and-scalability) currently uses a couple of different tools to measure Istio performance: [BluePerf](https://ibmcloud-perf.istio.io/regpatrol/) and [Fortio](https://fortio.istio.io).

1. Multi-mesh Functionalty Playground
A service mesh playground to faciliate learning about functionality of different service meshes. Meshery incorporates a visual interface for manipulating traffic routing rules. Sample applications will be included in Meshery. 

## <a name="running">Running Meshery</a>

### General Prerequisites
1. Docker engine (e.g. Docker for Desktop).
1. Kubernetes cluster (preferably version 1.10+).

#### Istio Playground Prerequisites
1. Istio version 1.0.3+ in `istio-system` namespace along with the Istio ingress gateway.

### Running Meshery on Kubernetes
- You can deploy Meshery to an existing kubernetes cluster using the provided yaml file into any namespace of your choice. For now let us deploy it to a namespace `meshery`: 

    ```bash
    kubectl create ns meshery
    kubectl -n meshery apply -f deployment_yamls/k8s

    # additional step for running on Istio
    kubectl -n meshery apply -f deployment_yamls/istio.yaml
    ```
    If you want to use a different namespace, please change the name of the namespace in the `ClusterRoleBinding` section appropriately.
- Meshery can be deployed either on/off your existing service mesh.
- If deployed on the same Kubernetes cluster as the mesh, you dont have to provide a kubeconfig file.
- please review the yaml and make necessary changes as needed for your cluster.

### Running Meshery on Docker
- We have a docker-compose.yaml file which can be used to spin up the services quickly
- There a few requirements for running all the Meshery services on your local
  - SSO, which uses Twitter and/or Github
    - Instructions to setup Twitter for SSO can be found <a href="#twitter">here</a>
    - Instructions to setup Github for SSO can be found <a href="#github">here</a>
  - Add an entry for `meshery-saas` in your `/etc/hosts` file to point to 127.0.0.1 and save the file
  - After setting up SSO, store the respective key and secret as variables in the shell as shown below.
    - for Twitter:
    ```
    TWITTERKEY="PASTE TWITTER KEY"
    ```
    ```
    TWITTERSECRET="PASTE TWITTER SECRET"
    ```
    - for Github:
    ```
    GITHUBKEY="PASTE GITHUB KEY"
    ```
    ```
    GITHUBSECRET="PASTE GITHUB SECRET"
    ```
    __Note__: you can use Twitter and/or Github

    Now that the environment variables are setup, we can start the containers by running:
    ```
    docker-compose up
    ```
    Please add a `-d` flag to the above command if you want to run it in the background.
- Now you should be able to access Meshery in your browser at `http://localhost:8080/play`

##### <a name="twitter">Using Twitter for SSO</a>
- Create an app in the Twitter developer console: [https://developer.twitter.com/en/apps](https://developer.twitter.com/en/apps) after logging in.
- Fill appropriate details in the presented form
  - Remember to enable to `Sign in with Twitter`
  - For the callback url, please use this value: `http://meshery-saas:9876/auth/twitter/callback`
- After creating the app you will be able to grab the API key and secret from the `Keys and tokens` section of the app.

##### <a name="github">Using Github for SSO</a>
- Create an OAuth app in the Github developer settings: [https://github.com/settings/developers](https://github.com/settings/developers) after logging in.
- Fill appropriate details in the presented form
  - For the callback url, please use this value: `http://meshery-saas:9876/auth/github/callback`
- After creating the app you will be able to grab the Client ID and Secret from the app page.

## Linkerd Playground App
_coming soon for Linkerd_

#### License

This repository and site are available as open source under the terms of the [Apache 2.0 License](https://opensource.org/licenses/Apache-2.0).

#### About Layer5
[Layer5.io](https://layer5.io) is a service mesh community, serving as a repository for information pertaining to the surrounding technology ecosystem (service meshes, api gateways, edge proxies, ingress and egress controllers) of microservice management in cloud native environments.
