---
layout: image-right
image: https://photo.pku.edu.cn/Uploads/Bdtpw/Picture/2021/11/08/s6188daf747437.jpg
---

Section 4
# Application Runtime

- Containers
- CI/CD
- Best Practice

---

# Containers
Application Runtime

To run applications, we have the following approaches:

- Directly run it
  - Dangerous!
- Create a service account, and run the app use that account
  - Complicated!
- **Wrap the app inside a container**

---

# Containers
Application Runtime

As for Linux, there are many container solutions, including Docker, LXC, LXD, Podman, kubernetes, etc.

In this presentation, we will focus on **Docker**, which is the most widely used solution.

---

# Containers
Application Runtime

Example 1: Running a 3rd part application (cloudflared)

```yaml
# docker-compose.yml
version: '3.4'
services:
  cloudflared:
    restart: always
    image: cloudflare/cloudflared
    container_name: cloudflared
    volumes:
      - /opt/volumes/cloudflared:/etc/cloudflared
    command: --config /etc/cloudflared/config.yml tunnel run
    restart: unless-stopped
    network_mode: host
```

To run the application, simple put your credentials and config in `/opt/volumes/cloudflared`, then run

```bash
docker compose up -d
```

---

# Containers
Application Runtime

Example 2: Running a 1st part application (thezzisu/telehole)

```docker
FROM node:gallium-alpine as builder
WORKDIR /app
COPY . .
RUN yarn install
RUN yarn build

FROM node:gallium-alpine
WORKDIR /app
COPY --from=builder /app/lib ./lib
COPY --from=builder /app/package.json ./
COPY --from=builder /app/yarn.lock ./
RUN yarn install --production
CMD [ "yarn", "start" ]
```

To build and run:

```sh
docker build -t thezzisu/telehole .
docker run --env-file .env --restart always -d ghcr.io/thezzisu/telehole
```

---

# CI/CD
Application Runtime

Also, we can automate the test and deployment process using CI/CD.

Example: Build and push docker image to GitHub Container Registry after each commit

---

```yaml
# .github/workflows/publish.yml
name: Publish to GHCR
on:
  push:
    branches:
      - 'master'
jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: docker/setup-qemu-action@v1
      - uses: docker/setup-buildx-action@v1
      - uses: docker/login-action@v1
        with:
          registry: ghcr.io
          username: ${{ github.repository_owner }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - uses: docker/build-push-action@v2
        with:
          context: .
          platforms: linux/amd64
          push: true
          tags: |
            ghcr.io/thezzisu/telehole:latest
```

---

# CI/CD
Application Runtime

By using CI/CD, we can save a lot of time and effort.

---

# Best Practice
Application Runtime

- Use **stateless** containers
  - Configuration via **environment variables**
  - Data via **volumes**
  - The rest part of container should be replacable at any time
- Use **docker compose** to manage services
  - Define a set of related containers (as a service) in a single file
- COW filesystem like btrfs is recommended
  - Built-in overlay support
  - Volume management & Snapshot