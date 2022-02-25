# "From an alpine image you’ll add to your container your favorite text editor, vim or
# emacs, that will launch along with your container."

# syntax=docker/dockerfile:1
FROM alpine
RUN apk update && \
	apk add vim && \
	rm -rf /var/lib/apt/lists/*
ENTRYPOINT ["vim"]

# docker build -t vims .
# docker run -it vims