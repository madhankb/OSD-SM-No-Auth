# Guide to setup self-managed dashboards in EC2 hosted container

## Prerequisite
An AWS managed OpenSearch domain without any authentication method enabled and is accompanied by the following domain access policy.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "es:*",
      "Resource": "arn:aws:es:ap-south-1:765423874566:domain/no-security/*"
    }
  ]
}
```
> [!WARNING]
> To establish a connection with the managed OpenSearch domain, it's necessary to uninstall the security plugin from OpenSearch Dashboards. Otherwise, the Dashboards' security plugin will anticipate a secured domain and will fail to make a connection

## Steps to remove the security plugin and spinup a self managed dashboards
1. Remove all Security plugin configuration settings from opensearch_dashboards.yml or place the below example file in the same folder as the Dockerfile
```yml
server.name: opensearch-dashboards
server.host: "0.0.0.0"
opensearch.hosts: http://localhost:9200
```
2. Create a new Dockerfile like below
```
FROM opensearchproject/opensearch-dashboards:2.5.0
RUN /usr/share/opensearch-dashboards/bin/opensearch-dashboards-plugin remove securityDashboards
COPY --chown=opensearch-dashboards:opensearch-dashboards opensearch_dashboards.yml /usr/share/opensearch-dashboards/config/
```
3. Run this command `docker build --tag=opensearch-dashboards-no-security .` to build a new Docker image with security plugin removed.
4. Validate if the new image is created by running the `docker images` command
5. In the attached sample `docker-compose.yml`, change the dashboards' image name from `opensearchproject/opensearch-dashboards:2.5.0` to `opensearch-dashboards-no-security`.
6. The new `docker-compose-no-security.yml` file is now created. Now run the `docker-compose up` command to run the containers with new image.
