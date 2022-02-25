# "You are going to create your first Dockerfile to containerize Rails applications. That’s
# a special configuration: this particular Dockerfile will be generic, and called in another
# Dockerfile, that will look something like this:
#
# FROM ft-rails:on-build
# EXPOSE 3000
# CMD ["rails", "s", "-b", "0.0.0.0", "-p", "3000"]
#
# Your generic container should install, via a ruby container, all the necessary dependencies and gems, then copy your rails application in the /opt/app folder of your
# container. Docker has to install the approtiate gems when it builds, but also launch
# the migrations and the db population for your application. The child Dockerfile should
# launch the rails server (see example below). If you don’t know what commands to use,
# it’s high time to look at the Ruby on Rails documentation."

# syntax=docker/dockerfile:1
FROM ruby:2.7.5
EXPOSE 3000
RUN apt-get update && \
	apt-get update && \
	gem install bundler && \
	gem install rails && \
	gem install sqlite3-ruby && \
	apt-get install nodejs -y && \
	mkdir -p /opt/app
# Executed on child image build
ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app
ONBUILD RUN bundle update && \
			bundle install && \
			rails db:migrate && \
			rails db:seed
# 1. Build with:
# docker build -t ft-rails:on-build .
# 2. Copy app folder to ex02.
# 3. Comment out the dockerfile above and uncomment the file below.
#FROM ft-rails:on-build
#EXPOSE 3000
#CMD ["rails", "s", "-b", "0.0.0.0", "-p", "3000"]
# 4. Build with:
# docker build -t jylimaul/rails .
# 5. Run with:
# docker run -it -p 3000:3000 jylimaul/rails