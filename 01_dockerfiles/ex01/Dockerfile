# "From a debian image you will add the appropriate sources to create a TeamSpeak
# server, that will launch along with your container. It will be deemed valid if at least
# one user can connect to it and engage in a normal discussion (no far-fetched setup), so
# be sure to create your Dockerfile with the right options. Your program should get the
# sources when it builds, they cannot be in your repository."

# syntax=docker/dockerfile:1

# Base image is debian
FROM debian

# EXPOSE defines which ports are intended to be used and opens them to container's internal processes. It doesn't actually publish them.
# Ports are published by operator with docker run -p.
EXPOSE 30033/tcp
EXPOSE 9987/udp
EXPOSE 10011/tcp

# Set environmental variable so server can start without reading license agreement
ENV TS3SERVER_LICENSE=accept

# Install server and clean up
RUN apt-get update && \
	apt-get install -y curl tar bzip2 && \
	rm -rf /var/lib/apt/lists/* && \
	curl -kLJO https://files.teamspeak-services.com/releases/server/3.13.6/teamspeak3-server_linux_amd64-3.13.6.tar.bz2 && \
	tar -xvf teamspeak3-server_linux_amd64-3.13.6.tar.bz2 && \
	rm -rf teamspeak3-server_linux_amd64-3.13.6.tar.bz2 && \
	apt-get autoremove -y && \
	apt-get clean

# Set working directory
WORKDIR /teamspeak3-server_linux_amd64

# Command to execute when container starts
ENTRYPOINT ["./ts3server"]

# Build docker image
# docker build -t jylimaul/teamspeak .

# Create and run the container
# docker run --name teamspeak-server -p 9987:9987/udp -p 30033:30033 -p 10011:10011 -d jylimaul/teamspeak

# Get password for the server
# docker logs teamspeak-server | grep password
#
# Use the client to connect to localhost with username serveradmin and add the token from the logs
# (You don't need the password to log in if you're the first user to the server. Token is enough.)