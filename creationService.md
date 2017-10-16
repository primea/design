# Creation Service

The Creation service allows for the creation of new container instances and 
is a system service (i.e. it is implemented as a privalged contract).

# API
The Creation service has two functions `create_instance` and `get_channel`. 

## `command_code`
Messages to the creation container must start with only of the following ids
* `0` - Defines a `create_instance` message
* `1` - Defines a `get_channel` message

## `create_instance`
Creates a new instance of a container. 

| Fiels | Type | Description |
|-------|------|-------------|
|code   | uint32  | The commands code | 
|type   | uint32  | The type of container to create |
|code   | bytes   | The code for the container to run |

## `get_channel`
Creates a new channel to the creation service. The resulting port will be 
returned on the response port.

| Fiels | Type | Description |
|-------|------|-------------|
| code  | uint32  | The commands code | 
