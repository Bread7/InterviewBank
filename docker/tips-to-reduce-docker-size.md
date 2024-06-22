# Tips to reduce docker size

A Docker layer represents a set of files or changes that are stacked on top of each other to form a complete Docker image. Each layer is built from instructions in the Dockerfile, such as `RUN`, `COPY`, and `ADD`. Each of these instructions **creates a new layer in the Docker image.**

## 1) Reduce the number of layers

In a Dockerfile, each `FROM`, `RUN`, and `COPY` command creates a separate layer, increasing the overall size and build time of the image.

To reduce the size of a Docker image, execute multiple commands in a single `RUN` or `COPY` instruction to minimize the number of layers in the Dockerfile.

```docker
FROM ubuntu:latest
RUN apt update -y
RUN apt install unzip -y
RUN apt install curl -y
RUN apt install python3 -y
```

Instead of using separate instructions for each command, combine them together:

```docker
FROM ubuntu:latest
RUN apt update -y && \
apt install unzip -y && \
apt install curl -y && \
apt install python3 -y
```

It manage to reduce by 2MB

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

## 2) Use smaller base image

The most obvious way to reduce the size of a Docker image is by using a smaller base image.

If you want to create an image for a Python application, consider using the `python:3.9-slim` image instead of `python:3.9`.

The size of `python:3.9` is about 1.3 GB, while `python:3.9-slim` is only about 1 GB.

You can further reduce the image size by using the Alpine version. Alpine images are specifically designed to run as containers and are very small. The `python:3.9-alpine` image is only 49 MB.

## 3) use no install recommends flag

When we run the `apt install` command to install some packages, it installs some unnecessary recommended packages. Using the `--no-install-recommends` flag can significantly reduce the image size.

```docker
FROM ubuntu:latest
RUN apt update -y && \
apt install unzip -y --no-install-recommends && \
apt install curl --no-install-recommends -y && \
apt install python3 -y --no-install-recommends
```

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption><p>Reduce by 3MB</p></figcaption></figure>

## 4) Remove the package after installed

After apt install, we can use this commands (`rm -rf /var/lib/apt/lists/*`)to remove the package left in storage to reduce the image size&#x20;

```docker
FROM ubuntu:latest
RUN apt update -y && \
apt install unzip -y --no-install-recommends && \
apt install curl --no-install-recommends -y && \
apt install python3 -y --no-install-recommends && \
rm -rf /var/lib/apt/lists/*
```

It reduce alot more this time.

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

## 5) Use .dockerignore

If you do not want to copy certain files into the Docker image, then using a `.dockerignore` file can save you some space.

There are some hidden files/folders in the build context that you can transfer to the image using the `ADD` or `COPY` commands, such as `.git`, etc. Including a `.dockerignore` file to reduce the size of the Docker image is a good practice.

Example of a `.dockerignore` file.

```
ignorethisfile.txt
logs/
ignorethisfolder/
.git
.cache
*.md
```

## 6) Copy after RUN

In some cases, you make minor changes to your code and need to repeatedly build the image from the Dockerfile. In such cases, placing the `COPY` command after the `RUN` command will help reduce the image size, as Docker will be able to make better use of its caching capabilities in this scenario.

It will create a cache for the image with installed dependencies, and each time the code changes, Docker will use this cache to create the image. This will also reduce the Docker build time.

```docker
FROM ubuntu:latest
RUN apt update -y && \
apt install unzip -y --no-install-recommends && \
apt install curl --no-install-recommends -y && \
apt install python3 -y --no-install-recommends && \
rm -rf /var/lib/apt/lists/*
COPY file /home/ubuntu
```

## 7) Delete the software package

If you need to install some packages in a Docker image and you are downloading them from external sources, it is best to delete those packages after installation.

For example, if you want to install AWS CLI V2 from a zip file, remember to also delete the zip file after successful installation.

```docker
FROM ubuntu:latest
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
unzip awscliv2.zip && \
sudo ./aws/install && \
rm awscliv2.zip
```

## Author

[Ik0nw](https://github.com/Ik0nw)

## Interview question

Can you compress this dockerfiles to make it smaller?

For python project what might be the files that might add it into `.dockerignore` ?

craft a sample .`dockerignore` file.

```docker
# Python images
FROM python:3.9

# Copying all current files to /app
COPY . /app

# update and install package
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install -y git

# Install python flask
RUN pip install flask
RUN pip install requests

# Install the aws cli
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
RUN unzip awscliv2.zip
RUN sudo ./aws/install

# Setting the working directory
WORKDIR /app

# Running the application
CMD ["python", "app.py"]
```

