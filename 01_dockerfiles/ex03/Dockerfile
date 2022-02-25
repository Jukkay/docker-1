# "Docker can be useful to test an application that’s still being developed without polluting your libraries. You will have to design a Dockerfile that gets the development
# version of Gitlab - Community Edition installs it with all the dependencies and the necessary configurations, and launches the application, all as it builds. The container will be
# deemed valid if you can access the web client, create users and interact via GIT with this
# container (HTTPS and SSH). Obviously, you are not allowed to use the official container
# from Gitlab, it would be a shame..."

FROM ubuntu:focal
RUN apt-get update -y && \
	apt-get upgrade -y && \
	DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
	curl \
	openssh-server \
	ca-certificates \
	postfix && \
	mkdir -p /etc/gitlab/trusted-certs && \
	openssl req -x509 -nodes -days 365 -newkey rsa:2048 -subj '/CN=localhost' -keyout /etc/gitlab/trusted-certs/localhost.key -out /etc/gitlab/trusted-certs/localhost.crt && \
	curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash && \
	apt-get install -y gitlab-ce && \
	rm -rf /var/lib/apt/lists/* && \
	echo "external_url \"https://localhost\"" >> /etc/gitlab/gitlab.rb && \
	echo "nginx['ssl_certificate'] = \"/etc/gitlab/trusted-certs/localhost.crt\"" >> /etc/gitlab/gitlab.rb && \
	echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/trusted-certs/localhost.key\"" >> /etc/gitlab/gitlab.rb && \
	echo "gitlab_rails['gitlab_shell_ssh_port'] = 4242" >> /etc/gitlab/gitlab.rb
EXPOSE 22 443 80
CMD (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && service ssh restart && gitlab-ctl tail

# Build:
# docker build -t jylimaul/gitlab .
# Run:
# docker run -it --rm -p 80:80 -p 4242:22 -p 443:443 --privileged -e GITLAB_ROOT_EMAIL="jukka.ylimaula@gmail.com" -e GITLAB_ROOT_PASSWORD="12345678" jylimaul/gitlab

# Test:
# 1. Register user "jylimaul"
# 2. Log in as root and go to admin panel to approve the user
# 3. Log in as jylimaul, create empty repo called "test", and add ssh key in user preferences.
# 4. Disable ssl authentication to avoid error message about self signed certificate:
#		git config --global http.sslVerify false
# 5. Clone the test repo with ssh and https addresses:
# 		git clone ssh://git@localhost:4242/jylimaul/test.git ssh_test
# 		git clone https://localhost/jylimaul/test.git https_test