## Atlassian bitbucket

This is the source for the automated build of the trusted `frootdock/atlassian-bitbucket`
image. For more Informations on the webapp please refere to the offical Atlassian bitbucket
[website][1].

### Prerequisites

In order to use this image you need at least [docker 1.6.0][2] and [docker-compose 1.2.0][3].

### Run the application

    # rebuild the docker images
    $ docker-compose build

    # restart the docker images
    $ docker-compose up -d

    # inspect the logs
    $ docker-compose logs

If you deploy the app for the first time you may need to restore the database from a backup!

### Debug (aka. go inside) an image

    # execute a bash shell
    $ docker exec -it atlassianstash_bitbucket_1 bash

### Backup

    # backup the home folder
    $ tar czf backup/bitbucket-home_$(date +%F).tgz home

    # backup the stash database
    $ docker run -it --rm --link atlassianbitbucket_database_1:db \
      -v $(pwd)/tmp:/tmp postgres sh -c 'pg_dump -U bitbucket \
        -h "$DB_PORT_5432_TCP_ADDR" -w stash > /tmp/bitbucket.dump'

### Restore

    # unpack a homefolder backup
    $ tar xzvf backup/home_2015-05-02.tgz --strip=1 -C home

    # restore the database backup
    $ docker run -it --rm --link atlassianbitbucket_database_1:db \
        -v $(pwd)/tmp:/tmp postgres sh -c 'pg_restore -U bitbucket \
        -h "$DB_PORT_5432_TCP_ADDR" -n public -w -d bitbucket \
        /tmp/bitbucket.dump'

---
[1]: https://www.atlassian.com/software/bitbucket
[2]: https://docs.docker.com/installation
[3]: https://docs.docker.com/compose
