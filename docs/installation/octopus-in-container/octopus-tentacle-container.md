---
title: Octopus as a Container
description: Running the Octopus Server or Tentacle from inside the official Docker container
position: 8
---

## Octopus Tentacle
Running a Tentacle inside a container may be preferable in some environments where installing one directly on the host is not an option. The currently available Octopus Tentacle Docker container is available to be run in either [polling](docs/infrastructure/windows-targets/polling-tentacles/index.md) or [listening](/docs/infrastructure/windows-targets/listening-tentacles/index.md) mode.

:::info
 Keep in mind that because the Tentacle will be running _inside a container_, script execution wont be happening on the host itself and so Octopus Tentacles inside a container may not be appropriate for many deployment tasks.
:::

When an Octopus Tentacle container starts up, it will attempt to invoke the [`register-with`](/docs/api-and-integration/tentacle.exe-command-line/register-with.md) command to connect and add itself as a machine to that server with the provided roles and environments. Note that due to some limitations in Windows Containers that have only recently been fixed and made available in the 1709 Windows release, this registration will occur on every startup and you may end up with multiple instances if you stop/start a container. Our goal is to update this image to de-register the Tentacle when the container `SIGKILL` signal is passed in.

```
docker run --name OctopusTentacle -p 10931:10933 -i --env "ListeningPort=10931" --env "ServerApiKey=API-MZKUUUMK3EYX7TBJP6FAKIFHIEO" --env "TargetEnvironment=Development" --env "TargetRole=container-server" --env "ServerUrl=http://172.23.191.1:8065" octopusdeploy/tentacle:3.19.2
```

### Configuration
When running an Octopus Tentacle Image, the following values can be provided to configure the running Octopus Tentacle instance.

#### Environment Variables
Read Docker [docs](https://docs.docker.com/engine/reference/commandline/run/#set-environment-variables--e---env---env-file) about setting environment variables.
|||
|---| --- |
|**ServerApiKey**|The API Key of the Octopus Server the Tentacle should register with|
|**ServerUsername**|If not using an api key, the user to use when registering the Tentacle with the Octopus Server|
|**ServerPassword**|If not using an api key, the password to use when registering the Tentacle|
|**ServerUrl**|The Url of the Octopus Server the Tentacle should register with|
|**TargetEnvironment**|Comma delimited list of environments to add this target to|
|**TargetRole**|Comma delimited list of roles to add to this target|
|**TargetName**|Optional Target name, defaults to host|
|**ServerPort**|The port on the Octopus Server that the Tentacle will poll for work. Implies a polling Tentacle|
|**ListeningPort**|The port that the Octopus Server will connect back to the Tentacle with. Defaults to `10933`. Implies a listening Tentacle|
|**PublicHostNameConfiguration**|How the url that the Octopus server will use to communicate with the Tentacle is determined. Can be `PublicIp`, `FQDN`, `ComputerName` or `Custom`. Defaults to `PublicIp`|
|**CustomPublicHostName**|If PublicHostNameConfiguration is set to `Custom`, the host name that the Octopus Server should use to communicate with the Tentacle|

#### Exposed Ports
Read Docker [docs](https://docs.docker.com/engine/reference/commandline/run/#publish-or-expose-port--p---expose) about exposing ports.
|||
|--|--|
|**10933**|Port tentacle will be listening on (if in listening mode)|

_The Octopus Server container does not currently support HTTPS however this should be available sometime in the future_

#### Volume Mounts
Read Docker [docs](https://docs.docker.com/engine/reference/commandline/run/#mount-volume--v---read-only) about mounting volume.
|||
|--|--|
|**C:\Applications**|Default directory to deploy applications to|