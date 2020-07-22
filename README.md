# Docker container for the Citrix Workspace app

This is a simple container to run the Citrix Workspace app using the firefox browser. Including the NetScaler Gateway plugin.

## How to build this docker image
```
docker build -t citrix .
```

## Start the container
I added the `--restart=always` flag in order to have this always running on my machine.
```
docker run -d -v /etc/timezone:/etc/timezone:ro -v /etc/localtime:/etc/localtime --restart=always --name citrix citrix
```

## Connect to the container via ssh and X-Forward
Please set the `<WEB_URL_TO_LOGIN>` to your specific login url

I created a shell script for each Citrix Server I wanted to get into. For example:
```
#!/bin/bash
ssh -f -X receiver@$(docker inspect --format '{{ .NetworkSettings.IPAddress }}' citrix) -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no /usr/bin/firefox <WEB_URL_TO_LOGIN> > /dev/null 2>&1
```
Then made it executable:
`chmod +x script.sh`
Below is the command to spawn a Firefox window with a URL that you specify by replacing `<WEB_URL_TO_LOGIN>`

```
ssh -f -X receiver@$(docker inspect --format '{{ .NetworkSettings.IPAddress }}' citrix) -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no /usr/bin/firefox <WEB_URL_TO_LOGIN> > /dev/null 2>&1
```


# Sources
[Ubuntu wiki](https://wiki.ubuntuusers.de/Citrix_Receiver_13/)
