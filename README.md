# peatio-workbench

Peatio workbench is an easy way to start Peatio development environment.

## Prerequisites

- Docker [installed](https://docs.docker.com/engine/installation/)
- Docker Compose [installed](https://docs.docker.com/compose/install/)

## Usage

### Prepare the workbench

1. Run `make init`. It will fetch peatio and barong repositories, pull all needed
   services images, along with `rubykube/peatio` and `rubykube/barong` images that
   will be used as cache for build to reduce the build time.

   Later you can run `make init` again to get latest images and repositories versions.
2. Build the images: `make build`
3. Start all services need for peatio: `make prepare`. Make sure mysql server
   is running before the next step (check `docker logs workbench_db_1`)
4. Prepare the database and configs: `make setup-apps`
5. To have barong login working with peatio you will need to add this to your `/etc/hosts`:

```
0.0.0.0 peatio
0.0.0.0 barong
```

### Run Barong and Peatio

#### Barong

1. Start barong: `docker-compose up -d barong`
2. Create admin user for barong: `docker-compose run --rm barong bin/rake db:seed`
   It will output password for **admin@barong.io**
3. Sign in at [barong:8001](http://barong:8001), then go to [/admin](http://barong:8001/admin)
   and navigate to [Applications](http://barong:8001/oauth/applications)
4. Create new application with the following callback url `http://peatio:8000/auth/barong/callback`

#### Peatio

1. In `docker-compose.yaml`, set the newly created application credentials:

```yaml
- BARONG_CLIENT_ID=xxxxx
- BARONG_CLIENT_SECRET=xxxxx
```

2. Start peatio server: `docker-compose up -d peatio`

#### Frontend

Simply start your local server. Now you're able to log in with your local Barong and Peatio.

## Running Tests

>**TODO**
