# Docker compose curated example

Set up both nginx & phpfpm, with a sample page, available on [localhost:80](http://localhost:80/), for convenience (no specific port to set in firewall)

See the [original project](https://github.com/youpiwaza/docker-compose-example) for more comments on nginx + phpfpm setup.

**Curated** > Apply most docker security & best practices to the build.

[Docker compose reference](https://docs.docker.com/compose/compose-file/).

```bash
# Go to your own folder
# > cd ~/../c/Users/Patolash/Documents/_dev/docker-compose-curated-example

# Start composition
> docker-compose up

# Stop/rm composition
# Ctrl + c

# Use dc down if launched with -d
> docker-compose down
```

Sources :

- *File docker-compose.yml*
- *Folder sources /src*
- *Folder configs /config*
  - Also contains defaults config files for both images, for reference

## Original bench, on configured host

```yaml
### Docker bench security audit
#   https://github.com/docker/docker-bench-security

# /!\ Executing straight away the docker run command execute v.1.3.4
# Run the basic script to get the latest 1.3.5 (Needs sudo rights)
#   > cd
#   > git clone https://github.com/docker/docker-bench-security.git
#   > cd docker-bench-security
#   > sudo sh docker-bench-security.sh

# Prevent false positives > prune all (included all non used images)
# > docker system prune
# > docker image prune -a

#---

# Total Score > Warn Scored -1, Pass Scored +1, Not Score (INFO) -0

# Bench after having corrected steps 1 to 5
# > Checks: 107 / Score: XXXX / XXXX WARN (host partition, auth for docker clients commands, images healthcheck false positive)

# Original bench with a random container started, wo. flags
# > Checks: 107 / Score 48 / ~11 WARN

# ---

# Original bench steps 4 & 5

# [INFO] 4 - Container Images and Build File
# [WARN] 4.1  - Ensure a user for the container has been created
# [WARN]      * Running as root: docker-compose-curated-test-repo_phpfpm_1
# [WARN]      * Running as root: docker-compose-curated-test-repo_web_1
# [NOTE] 4.2  - Ensure that containers use only trusted base images
# [NOTE] 4.3  - Ensure that unnecessary packages are not installed in the container
# [NOTE] 4.4  - Ensure images are scanned and rebuilt to include security patches
# [PASS] 4.5  - Ensure Content trust for Docker is Enabled
# [WARN] 4.6  - Ensure that HEALTHCHECK instructions have been added to container images
# [WARN]      * No Healthcheck found: [alpine:3.11.5 alpine:latest]
# [WARN]      * No Healthcheck found: [alpine:3.11.5 alpine:latest]
# [WARN]      * No Healthcheck found: [nginx:alpine]
# [WARN]      * No Healthcheck found: [nginx:1.16.1-alpine]
# [WARN]      * No Healthcheck found: [php:7.4.1-fpm]
# [WARN]      * No Healthcheck found: [nginx:1.17.6-alpine]
# [INFO] 4.7  - Ensure update instructions are not use alone in the Dockerfile
# [INFO]      * Update instruction found: [php:7.4.1-fpm]
# [NOTE] 4.8  - Ensure setuid and setgid permissions are removed
# [PASS] 4.9  - Ensure that COPY is used instead of ADD in Dockerfiles
# [NOTE] 4.10  - Ensure secrets are not stored in Dockerfiles
# [NOTE] 4.11  - Ensure only verified packages are installed


# [INFO] 5 - Container Runtime
# [PASS] 5.1  - Ensure that, if applicable, an AppArmor Profile is enabled
# [WARN] 5.2  - Ensure that, if applicable, SELinux security options are set
# [WARN]      * No SecurityOptions Found: docker-compose-curated-test-repo_phpfpm_1
# [WARN]      * No SecurityOptions Found: docker-compose-curated-test-repo_web_1
# [PASS] 5.3  - Ensure Linux Kernel Capabilities are restricted within containers
# [PASS] 5.4  - Ensure that privileged containers are not used
# [PASS] 5.5  - Ensure sensitive host system directories are not mounted on containers
# [PASS] 5.6  - Ensure sshd is not run within containers
# [WARN] 5.7  - Ensure privileged ports are not mapped within containers
# [WARN]      * Privileged Port in use: 80 in docker-compose-curated-test-repo_web_1
# [NOTE] 5.8  - Ensure that only needed ports are open on the container
# [PASS] 5.9  - Ensure the host's network namespace is not shared
# [WARN] 5.10  - Ensure that the memory usage for containers is limited
# [WARN]      * Container running without memory restrictions: docker-compose-curated-test-repo_phpfpm_1
# [WARN]      * Container running without memory restrictions: docker-compose-curated-test-repo_web_1
# [WARN] 5.11  - Ensure CPU priority is set appropriately on the container
# [WARN]      * Container running without CPU restrictions: docker-compose-curated-test-repo_phpfpm_1
# [WARN]      * Container running without CPU restrictions: docker-compose-curated-test-repo_web_1
# [WARN] 5.12  - Ensure that the container's root filesystem is mounted as read only
# [WARN]      * Container running with root FS mounted R/W: docker-compose-curated-test-repo_phpfpm_1
# [WARN]      * Container running with root FS mounted R/W: docker-compose-curated-test-repo_web_1
# [WARN] 5.13  - Ensure that incoming container traffic is bound to a specific host interface
# [WARN]      * Port being bound to wildcard IP: 0.0.0.0 in docker-compose-curated-test-repo_web_1
# [WARN] 5.14  - Ensure that the 'on-failure' container restart policy is set to '5'
# [WARN]      * MaximumRetryCount is not set to 5: docker-compose-curated-test-repo_phpfpm_1
# [WARN]      * MaximumRetryCount is not set to 5: docker-compose-curated-test-repo_web_1
# [PASS] 5.15  - Ensure the host's process namespace is not shared
# [PASS] 5.16  - Ensure the host's IPC namespace is not shared
# [PASS] 5.17  - Ensure that host devices are not directly exposed to containers
# [PASS] 5.18  - Ensure that the default ulimit is overwritten at runtime if needed
# [PASS] 5.19  - Ensure mount propagation mode is not set to shared
# [PASS] 5.20  - Ensure the host's UTS namespace is not shared
# [PASS] 5.21  - Ensure the default seccomp profile is not Disabled
# [NOTE] 5.22  - Ensure docker exec commands are not used with privileged option
# [NOTE] 5.23  - Ensure that docker exec commands are not used with the user=root option
# [PASS] 5.24  - Ensure that cgroup usage is confirmed
# [WARN] 5.25  - Ensure that the container is restricted from acquiring additional privileges
# [WARN]      * Privileges not restricted: docker-compose-curated-test-repo_phpfpm_1
# [WARN]      * Privileges not restricted: docker-compose-curated-test-repo_web_1
# [WARN] 5.26  - Ensure that container health is checked at runtime
# [WARN]      * Health check not set: docker-compose-curated-test-repo_phpfpm_1
# [WARN]      * Health check not set: docker-compose-curated-test-repo_web_1
# [INFO] 5.27  - Ensure that Docker commands always make use of the latest version of their image
# [WARN] 5.28  - Ensure that the PIDs cgroup limit is used
# [WARN]      * PIDs limit not set: docker-compose-curated-test-repo_phpfpm_1
# [WARN]      * PIDs limit not set: docker-compose-curated-test-repo_web_1
# [PASS] 5.29  - Ensure that Docker's default bridge 'docker0' is not used
# [PASS] 5.30  - Ensure that the host's user namespaces are not shared
# [PASS] 5.31  - Ensure that the Docker socket is not mounted inside any containers
```
