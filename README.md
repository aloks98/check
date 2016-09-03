# Checkdesk 3

- Install `docker-compose`
- `git clone --recursive git@github.com:meedan/checkdesk-app.git && cd checkdesk-app`
- Configuration:
  - Copy `checkdesk-api/config/config.yml.example` to `checkdesk-api/config/config.yml` and edit the Twitter/Facebook keys to enable social login
  - Copy `checkdesk-api/config/database.yml.example` to `checkdesk-api/config/database.yml`
  - Copy `pender/config/config.yml.example` to `pender/config/config.yml` and edit the Twitter/Facebook keys to enable better parsing of their respective media
  - Copy `pender/config/database.yml.example` to `pender/config/database.yml`
  - Copy `checkdesk-client/config.js.example` to `checkdesk-client/config.js`
- `docker-compose build`
- `docker-compose up`
  - Databases (Postgres, Elasticsearch, etc.) will persist across runs - to clean, invoke `./docker-clean.sh`
  - Container names (as per `docker-compose.yml`):
    - `web` = Checkdesk client
    - `api` = Checkdesk API
    - `pender` = Pender API
    - `elasticsearch` = Elasticsearch
    - `postgres` = Postgres

## Additional steps
- Build the client bundle:
  - `docker-compose run web bash`
  - `npm run build` (for `node` legacy reasons this doesn't work when invoked from `docker-compose` directly)
- Update submodules to their latest commit
  - `./git-update.sh`
- Cleanup docker images and volumes
  - `./docker-clean.sh`

## Troubleshooting

If you're using VirtualBox, and can reach the servers via the ngrok.io URL in stdout, but not locally (e.g. localhost:3333 fails), you might need to set up port forwards (e.g. host 3333 to guest 3333) on the VirtualBox VM:

0. Run `docker ps` to see the necessary ports
0. Open VirtualBox.app (can also do on command line)
0. Select the running VM and click "Settings" in the main window toolbar
0. Click "Network" and "Port Forwarding"

If you're having difficulties building images try..
   - `docker-compose build --pull`
This will download the base image specified by the `FROM` instuction in each Dockerfile (usually `meedan/ruby`)


## Available services

- Checkdesk client at [http://localhost:3333](http://localhost:3333)
- Checkdesk API at [http://localhost:3000/api](http://localhost:3000/api) - use `dev` as API key
- Checkdesk GraphQL at [http://localhost:3000/graphiql](http://localhost:3000/graphiql)
- Pender API at [http://localhost:3200/api](http://localhost:3200/api) - use `dev` as API key
- Elasticsearch at [http://localhost:9200/_plugin/gui](http://localhost:9200/_plugin/gui)
- Postgres at port 5432 (you can use a standard Pg admin tool to connect)

## Testing

- Checkdesk Client: `docker-compose run web npm run test`
- Checkdesk API: `docker-compose run api-test bundle exec rake test`
- Pender API: `docker-compose run pender-test bundle exec rake test`
