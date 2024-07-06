---
title: "Project Giggs"
updated: 2024-07-06
---

# Project: Giggs

## Backlog
- Get secrets working for BulletTrain & Fly
- Giggs is running on Fly (custom domain)
- As a user, I can see a list of shows
- As a user, I can see associated comics for a show

## Icebox
- We have a domain name
- Production domain name connected to Fly Cname

## Models
- Show
- Comic

## Build Notes

Show credentials:
```
bin/rails credentials:show

```

Set the key:
```
flyctl secrets set RAILS_MASTER_KEY=your_master_key
```

Open `config/master.key`

Edit `fly.toml`:
```
[env]
  RAILS_MASTER_KEY = "your_master_key"
```

Get encryption key for `devise`
```
openssl rand -hex 16
```

```
fly secrets set DEVISE_PEPPER=<your_generated_pepper>

```

Verify environment variables are setup
```
echo $DEVISE_PEPPER
fly secrets list

```

## Running the app
```
source /config/credentials/set_environment_secrets.zsh
```

### Deploy the app to FLY
Deploy the app
```
fly deploy

```

Our Last State:

   ~/Code/giggs  on   main ⇣13⇡4 !3 ?2  fly deploy                                                                                       1 ✘  3.3.2   at 01:26:48 PM  
==> Verifying app config
Validating /Users/jonahprice/Code/giggs/fly.toml
✓ Configuration is valid
--> Verified app config
WARN RAILS_MASTER_KEY may be a potentially sensitive environment variable. Consider setting it as a secret, and removing it from the [env] section: https://fly.io/docs/reference/secrets/

==> Building image
Remote builder fly-builder-bitter-dust-1305 ready
Remote builder fly-builder-bitter-dust-1305 ready
==> Building image with Docker
--> docker host: 24.0.7 linux x86_64
[+] Building 1.7s (25/25) FINISHED                                                                                                                                                  
 => [internal] load .dockerignore                                                                                                                                              0.1s
 => => transferring context: 745B                                                                                                                                              0.1s
 => [internal] load build definition from Dockerfile                                                                                                                           0.1s
 => => transferring dockerfile: 2.90kB                                                                                                                                         0.1s
 => resolve image config for docker.io/docker/dockerfile:1                                                                                                                     0.5s
 => CACHED docker-image://docker.io/docker/dockerfile:1@sha256:e87caa74dcb7d46cd820352bfea12591f3dba3ddc4285e19c7dcd13359f7cefd                                                0.0s
 => [internal] load metadata for docker.io/library/ruby:3.3.2-slim                                                                                                             0.2s
 => [internal] load build context                                                                                                                                              0.7s
 => => transferring context: 34.62kB                                                                                                                                           0.7s
 => [base 1/5] FROM docker.io/library/ruby:3.3.2-slim@sha256:4d611590cb3dc3211dc2e42c87347970c0ae9f7ad9c3db17a121d5996296f8ff                                                  0.0s
 => CACHED [base 2/5] WORKDIR /rails                                                                                                                                           0.0s
 => CACHED [base 3/5] RUN gem update --system --no-document &&     gem install -N bundler                                                                                      0.0s
 => CACHED [base 4/5] RUN apt-get update -qq &&     apt-get install --no-install-recommends -y curl libicu-dev &&     rm -rf /var/lib/apt/lists /var/cache/apt/archives        0.0s
 => CACHED [base 5/5] RUN curl -sL https://github.com/nodenv/node-build/archive/master.tar.gz | tar xz -C /tmp/ &&     /tmp/node-build-master/bin/node-build "20.14.0" /usr/l  0.0s
 => CACHED [stage-2 1/4] RUN apt-get update -qq &&     apt-get install --no-install-recommends -y curl imagemagick libvips postgresql-client &&     rm -rf /var/lib/apt/lists  0.0s
 => CACHED [build 1/9] RUN apt-get update -qq &&     apt-get install --no-install-recommends -y build-essential libpq-dev libvips node-gyp pkg-config python-is-python3        0.0s
 => CACHED [build 2/9] RUN corepack enable &&     corepack prepare yarn@4.2.2 --activate                                                                                       0.0s
 => CACHED [build 3/9] COPY --link Gemfile Gemfile.lock .ruby-version ./                                                                                                       0.0s
 => CACHED [build 4/9] RUN bundle install &&     bundle exec bootsnap precompile --gemfile &&     rm -rf ~/.bundle/ "/usr/local/bundle"/ruby/*/cache "/usr/local/bundle"/ruby  0.0s
 => CACHED [build 5/9] COPY --link package.json yarn.lock ./                                                                                                                   0.0s
 => CACHED [build 6/9] RUN yarn install --immutable                                                                                                                            0.0s
 => CACHED [build 7/9] COPY --link . .                                                                                                                                         0.0s
 => CACHED [build 8/9] RUN bundle exec bootsnap precompile app/ lib/                                                                                                           0.0s
 => CACHED [build 9/9] RUN SECRET_KEY_BASE_DUMMY=1 ./bin/rails assets:precompile                                                                                               0.0s
 => CACHED [stage-2 2/4] COPY --from=build /usr/local/bundle /usr/local/bundle                                                                                                 0.0s
 => CACHED [stage-2 3/4] COPY --from=build /rails /rails                                                                                                                       0.0s
 => CACHED [stage-2 4/4] RUN groupadd --system --gid 1000 rails &&     useradd rails --uid 1000 --gid 1000 --create-home --shell /bin/bash &&     chown -R 1000:1000 db log t  0.0s
 => exporting to image                                                                                                                                                         0.0s
 => => exporting layers                                                                                                                                                        0.0s
 => => writing image sha256:f171ab865610e145318d13b206ed4e53f4ea646a06dcc65db24b5b164c07a079                                                                                   0.0s
 => => naming to registry.fly.io/giggs:deployment-01J24VNWJRP19X3EKSTXWWTZ4Q                                                                                                   0.0s
--> Building image done
==> Pushing image to fly
The push refers to repository [registry.fly.io/giggs]
d9b9a3899e27: Layer already exists 
98f33c8c6ca6: Layer already exists 
ec2be5289970: Layer already exists 
9a15f0c1fc29: Layer already exists 
a420f7788dfa: Layer already exists 
ad9d68edc086: Layer already exists 
7fc118d20e97: Layer already exists 
e3cda5a92b5c: Layer already exists 
12a3edda1a09: Layer already exists 
4f73cd38a2c8: Layer already exists 
f999a5876eea: Layer already exists 
df253112c3e8: Layer already exists 
5d4427064ecc: Layer already exists 
deployment-01J24VNWJRP19X3EKSTXWWTZ4Q: digest: sha256:1b1b7635b8ea12274293bbb3fc78c3d4e1eebc8542ebc4249da0867f5fcefc7a size: 3057
--> Pushing image done
image: registry.fly.io/giggs:deployment-01J24VNWJRP19X3EKSTXWWTZ4Q
image size: 1.1 GB

Watch your deployment at https://fly.io/apps/giggs/monitoring

Running giggs release_command: ./bin/rails db:prepare

-------
 ✖ release_command failed
-------
Error release_command failed running on machine 328733df060968 with exit code 1.
Check its logs: here's the last 100 lines below, or run 'fly logs -i 328733df060968':
  Pulling container image registry.fly.io/giggs:deployment-01J24VNWJRP19X3EKSTXWWTZ4Q
  Successfully prepared image registry.fly.io/giggs:deployment-01J24VNWJRP19X3EKSTXWWTZ4Q (462.280726ms)
  Configuring firecracker
  2024-07-06T20:27:39.176686087 [01J24VP4YC36ET71FDBT186DQQ:main] Running Firecracker v1.7.0
  [    0.263675] PCI: Fatal: No config space access function found
   INFO Starting init (commit: ad092ccf)...
   INFO Preparing to run: `/rails/bin/docker-entrypoint ./bin/rails db:prepare` as 1000
   INFO [fly api proxy] listening at /.fly/api
  2024/07/06 20:27:39 INFO SSH listening listen_address=[fdaa:9:7587:a7b:b2e2:961a:405f:2]:22 dns_server=[fdaa::3]:53
  Machine created and started in 2.478s
  bin/rails aborted!
  ArgumentError: key must be 16 bytes (ArgumentError)
          cipher.key = @secret
                       ^^^^^^^
  /rails/config/initializers/devise.rb:10:in `block in <main>'
  /rails/config/initializers/devise.rb:3:in `<main>'
  /rails/config/environment.rb:5:in `<main>'
  /rails/bin/rails:4:in `<main>'
  Tasks: TOP => db:prepare => db:load_config => environment
  (See full trace by running task with --trace)
   INFO Main child exited normally with code: 1
   INFO Starting clean up.
   WARN could not unmount /rootfs: EINVAL: Invalid argument
  [    5.592708] reboot: Restarting system
-------
Error: release command failed - aborting deployment. error release_command machine 328733df060968 exited with non-zero status of 1
