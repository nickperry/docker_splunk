# dockerfile_splunk
This is a Splunk install with proper process control.

The default command for the container is /bin/splunk.sh. This wraps /opt/splunk/bin/splunk to start all Splunk processes and then gracefully shuts them down upon receiving a SIGTERM from a 'docker stop'.

Splunk is configured to run as user splunk.

Run it with 'docker run -d -p 8000:8000 -p 8089:8089 -p 7999:7999 nickperry/splunk' to make the web interface available at http://your_docker_host:8000 and the REST API available at https://your_docker_host:8089. 7999 is the port for sending data to from a forwarder.

Optionally map a volume to /opt/splunk/var to persist your indexed data. /data can be mapped as a path to load data into Splunk from files.

Splunk can take a little while to shut down, so I highly recommend specifying a longer timeout to the docker stop command (docker stop -t 60), otherwise it is likely that docker's default 10s stop timeout will be breached and the splunk processes will get SIGKILLed.
