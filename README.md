# APISIX Standalone Deployment and Declarative Configuration Example

This repo demonstrates how you can run [Apache APISIX](https://apisix.apache.org/) in the **standalone mode** (Without etcd).

## How does it work?

Traditionally, APISIX Gateway has always etcd. However, APISIX can be run without an etcd too using only in-memory storage for APISIX objects. 

For this mode, you start with an [apisix.yml](https://github.com/Boburmirzo/apisix-standalone-deployment-mode/blob/main/apisix/apisix.yml) file (which includes **declarative configuration** for APISIX objects like *upstreams, plugins, and routes*). Using the standalone and declarative configuration mode is one of the quickest and easiest ways to get started with the APISIX API Gateway.

We can start APISIX by providing this declarative configuration. One note is that the APISIX Admin API will not be available. But if you want to make any changes to the configuration, you’ll need to change the config file. APISIX checks if this file has any change every second. If the file is changed & it ends with #END, APISIX loads the rules from this file and applies changes immediately. Read more about standalone mode [here](https://apisix.apache.org/docs/apisix/deployment-modes/#standalone).

## How to run this demo?

**Prerequisites**

• [Docker](https://docs.docker.com/get-docker/) is used to install the containerized example backend service(You can use your own API backend service) and APISIX.

To start the project run simply the following command from the project root directory:

```bash
docker compose up
```

For example, in this `apisix.yml` file we defined different settings related to upstreams, routes, plugins, and consumers in APISIX.

Let's break down each part:

1. **`upstreams`**: This section defines the list of backend servers (upstream) to which the API Gateway will forward incoming requests. In this example, there's a single upstream defined:
    - **`name`**: The name of the upstream, here it's "example upstream".
    - **`id`**: The unique identifier for the upstream, in this case, 1.
    - **`type`**: The load balancing algorithm used for this upstream. Here, it's "roundrobin", which distributes requests in a round-robin manner among the nodes.
    - **`nodes`**: Defines the backend servers' details. Here, "backend:80" is the backend server and 1 is the weight of the node. Weight could be used in weighted round robin or consistent hashing scenarios.
2. **`routes`**: This section defines the rules that the gateway uses to match incoming requests and select the appropriate upstream. The example defines one route:
    - **`name`**: The name of the route, here it's "example-route".
    - **`uri`**: The incoming request path that this route will match. Here, it's "/" meaning it matches all requests.
    - **`method`**: The HTTP method of the incoming request that this route will match, here it's "GET".
    - **`upstream_id`**: This associates the route with the previously defined upstream. Here it's set to 1, which means this route will forward the matched requests to the upstream defined with id 1.
    - **`plugins`**: This part defines the plugins to be used on this route. Here, it's using the "key-auth" plugin but without any specific configurations (hence the empty brackets).
3. **`consumers`**: This section is used for specifying consumer-level information and plugins. In this configuration, a consumer with the username "exampleuser" is defined.
    - **`username`**: The name of the consumer.
    - **`plugins`**: This part defines the plugins to be used by this consumer. Here, the "key-auth" plugin is defined with the key "example-key".

The **`key-auth`** plugin is a simple key authentication plugin for APIsIX, it checks if a correct **`apikey`** is in the request header. In the configuration above, the **`key-auth`** plugin is activated for the defined route and a key (**`example-key`**) is defined for the consumer **`exampleuser`**. Therefore, when a GET request is made to the root path ("/"), the consumer must include this key in their request for the request to be allowed by the gateway.

The configuration file ends with a **`#END`** comment, indicating the end of the file.

### Community

🙋 [Join the Apache APISIX Community](https://apisix.apache.org/docs/general/join/)

🐦 [Follow us on Twitter](https://twitter.com/ApacheAPISIX)

📝 [Find us on Slack](https://join.slack.com/t/the-asf/shared_invite/zt-vlfbf7ch-HkbNHiU_uDlcH_RvaHv9gQ)

💁 [How to contribute page](https://apisix.apache.org/docs/general/how-to-contribute/)

### About the author

Follow me on Twitter: [@BoburUmurzokov](https://twitter.com/BoburUmurzokov)

Visit my blog: [www.iambobur.com](https://www.iambobur.com/)
