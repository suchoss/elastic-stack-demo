Download the dependencies for the application:

    go mod vendor

Launch the service with Docker Compose:

    docker-compose --project-name demo up

Wait until Kibana is running:

    until docker inspect kibana_1 > /dev/null 2>&1 && [[ $(docker inspect -f '{{ .State.Health.Status }}' kibana_1) == "healthy" ]]; do echo -n '.'; sleep 5; done;

Open Kibana at <http://localhost:5601> and drive some traffic to the application:

    for i in `seq 1 100`; do curl -s http://localhost:8080; sleep 0.1; done

Alternatively, use a tool like `siege`, `wrk` or `hey`:

    siege --time=15s --internet http://localhost:8080
    wrk --duration 15s  --latency http://localhost:8080
    hey -z 15s http://localhost:8080