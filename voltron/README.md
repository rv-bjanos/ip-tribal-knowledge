# Voltron has defeated Megazord

But understand that running a microservice application is really tricky.

There are many improvements that Voltron needs, but here are some hacky workarounds for specific use cases.

## I need to run integration tests for an http service

First, run `voltron start <service-name>` for each service that is being macrified. This will allow the databases to be copied to the new service.

Next, run `voltron start <http-service-name>` to get that service up and running. 

Now run `go test ./...` and it should run! If you need to make changes, that's where it gets hacky...

## To restart an http service with changes

First, run `voltron stop <http-service-name>` to kill the container.

Next, run `docker system prune` because Voltron has trouble cleaning up after itself.

Next, run `docker images` and copy the image id of both the service AND the migration.

Run `docker rmi -f <image-id>` to delete the service and migration images.

Finally, run `voltron start <http-service-name>` and it should spin up.
