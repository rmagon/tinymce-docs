### Deploy the TinyMCE spelling service server-side component using Docker
The TinyMCE spelling service server-side component can be deployed using Docker or an container orchestration application such as Kubernetes. {{site.companyname}} provides a pre-configured package for creating a Spelling Service Docker image.

> **Warning**: Do not push this docker image to a publicly accessible  container registry. Doing so will constitute as a breach of the [{{site.companyname}} Self-Hosted Software License Agreement](https://about.tiny.cloud/legal/tiny-self-hosted-software-license-agreement-enterprise/).

#### Prerequisites
* The [Docker Engine](https://docs.docker.com/engine/docker-overview/) is installed and running.
* The user has Administrative or Root user access.
* The user is either:
  * Using a Linux, Unix or macOS operating system.
  * Using Windows and has access to unix command line tools using [Git for Windows](https://gitforwindows.org/), [Cygwin](https://www.cygwin.com/), or the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

The following procedure covers downloading, configuring, building and testing the {{site.productname}} Spelling Service Docker image.

1. Download `<filename>.zip` from `<location>` by `<process>`.
2. Open a command line and navigate to the directory containing `<filename>.zip`. Windows Users should open a Bash command line as the Administrator User.
3. Extract the contents of `<filename>.zip`, such as:

    ```sh
    unzip <filename>.zip
    ```
    The structure of the extracted files will be:
    ```sh
    tinymce/
    ├── config/
    │   └── ephox-spelling-docker-env.conf
    ├── docker-entrypoint.sh
    ├── Dockerfile
    ├── ephox-spelling.war
    └── generate-jetty-start.sh
    ```
4. Navigate into the extracted folder.

    ```sh
    cd <filename>
    ```
5. Set the permissions on the extracted files to executable.

    ```sh
    chmod +x *.sh
    ```
6. _Optional_: Edit the `http` configuration settings in `config/ephox-spelling-docker-env.conf`. The configurable settings are in the `http` section of the file. For example:

    ```
    http {
      max-connections = 100
      max-connections-per-host = 10
      max-redirects = 10
      max-retries = 3
      request-timeout-seconds = 10
      trust-all-cert = false
      websphere = {
        use-ssl-config = true
      }
    }
    ```
    For information on the `http` configuration setting, see: [Configure server-side components - `http`]({{site.baseurl}}/enterprise/server/configure/#httpoptional).
1. _Optional_: Configure the service to use a HTTP proxy by updating `config/ephox-spelling-docker-env.conf`. See:
[Configure server-side components]({{site.baseurl}}/enterprise/server/configure/).
1. _Optional_: Add a custom dictionary, as described in [Adding custom dictionaries]({{site.baseurl}}/enterprise/check-spelling/custom/).
7. Create an `origins.env` file and specify the Hypertext Transfer Protocol (HTTP) and domain name of sites hosting the TinyMCE editor (`allowed-origins`). Up to 99 origins can be added without editing `config/ephox-spelling-docker-env.conf`.

    For example:

    ```conf
    ORIGIN0=[example.net](<http://example.net/>)
    ORIGIN1=example
    ORIGIN2=http://[example.org](<http://example.org//>)
    ```
    For information on `allowed-origins`, see: [Configure server-side components - allowed-origins]({{site.baseurl}}/enterprise/server/configure/#allowed-originsrequired).
8. As the root user or Administrator, build the {{site.productname}} Spelling Service Docker image using the following command:

    ```sh
    docker build -t tinymce-spelling-service .
    ```
9. As the root user or Administrator, deploy the service using the following command:

    ```sh
    docker run -d -p 8082:8080 --env-file origins.env tinymce-spelling-service
    ```
    Where:
    * `-p 8082:8080` maps the container port `8080` to local port `8082`.
    * `--env-file origins.env` adds the allowed origins to the container.
10. To verify that the Docker container is deployed and the spelling service is running, execute:

    ```sh
    curl http://localhost:8082/ephox-spelling/
    ```
    The response from the `curl` command should be:
    ```
    Spelling service is running.
    ```

The {{site.productname}} Spelling Service Docker image can now be pushed to a private container registry for deployment on [Kubernetes](https://kubernetes.io/), [Docker Swarm](https://docs.docker.com/engine/swarm/) or [OpenShift](https://www.openshift.com/).