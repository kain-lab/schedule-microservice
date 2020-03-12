# Schedule gRPC

Manage scheduled tasks using gRPC

## Configuration

For configuration, please refer to `config/config.example.yml`

- **debug** `bool` Start debugging, ie `net/http/pprof`, access address is`http://localhost:6060`
- **listen** `string` Microservice listening address
- **log** `object` Log configuration
    - **storage** `bool` Turn on local logs
    - **storage_dir** `string` Local log storage directory
    - **socket** `bool` Enable remote log transfer
    - **socket_port** `int` Define the socket listening port

## Service

The service is based on gRPC and you can view `router/router.proto`

```
syntax = "proto3";

service Router {
    rpc Get (GetParameter) returns (GetResponse) {
    }

    rpc Lists (ListsParameter) returns (ListsResponse) {
    }

    rpc All (NoParameter) returns (AllResponse) {
    }

    rpc Put (PutParameter) returns (Response) {
    }

    rpc Delete (DeleteParameter) returns (Response) {
    }

    rpc Running (RunningParameter) returns (Response) {
    }
}

message NoParameter {
}

message Response {
    uint32 error = 1;
    string msg = 2;
}

message EntryOption {
    string cron_time = 1;
    string url = 2;
    bytes headers = 3;
    bytes body = 4;
}

message EntryOptionWithTime {
    string cron_time = 1;
    string url = 2;
    bytes headers = 3;
    bytes body = 4;
    int64 next_date = 5;
    int64 last_date = 6;
}

message Information {
    string identity = 1;
    bool start = 2;
    string time_zone = 3;
    map<string, EntryOptionWithTime> entries = 4;
}

message GetParameter {
    string identity = 1;
}

message GetResponse {
    uint32 error = 1;
    string msg = 2;
    Information data = 3;
}

message ListsParameter {
    repeated string identity = 1;
}

message ListsResponse {
    uint32 error = 1;
    string msg = 2;
    repeated Information data = 3;
}

message AllResponse {
    uint32 error = 1;
    string msg = 2;
    repeated string data = 3;
}

message PutParameter {
    string identity = 1;
    string time_zone = 2;
    bool start = 3;
    map<string, EntryOption> entries = 4;
}

message DeleteParameter {
    string identity = 1;
}

message RunningParameter {
    string identity = 1;
    bool running = 2;
}
```