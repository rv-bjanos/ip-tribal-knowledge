# Voltron has defeated Megazord

But understand that running a microservice application is really tricky.

There are many improvements that Voltron needs, but here are some hacky workarounds for specific use cases.

## To get started with Voltron

Go [here](https://github.com/RedVentures/filo-util-voltron). Tyler documented how to get started really well. Make sure your `$GOBIN` is set in your bash/zsh profile.

## To run integration tests for an http service

First, run `voltron start <service-name>` for each service that is being macrified. This will allow the databases to be copied to the new service.

Next, run `voltron start <http-service-name>` to get that service up and running. 

Now run `go test ./...` and it should run! If you need to make changes, that's where it gets hacky...

## To restart an http service with changes

First, run `voltron stop <http-service-name>` to kill the container.

Next, run `docker system prune` because Voltron has trouble cleaning up after itself.

Next, run `docker images` and copy the image id of both the service AND the migration.

Run `docker rmi -f <image-id>` to delete the service and migration images.

Finally, run `voltron start <http-service-name>` and it should spin up.

## To run integration tests for a gRPC service

You're out of luck, sorry. The best way I'm aware to test a gRPC service is by running the admin BFF and then testing the endpoint there. Hopefully by the time anyone reads this, we're out of gRPC hell.

## To test the endpoints of an http service

Just run the service and hit it at whatever port is exposed via postman. You can see the port by running `docker ps`. The port will be whichever number is before the `->`. So if it says ` 0.0.0.0:7161-7162->7161-7162/tcp`, you'll reach out to `http://localhost:7161`.
