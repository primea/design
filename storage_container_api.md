# Storage Container API
The Storage Container API expose a key/value db API and is the only way that data is persistently saved between executions of a program. 
It is designed to be extensible with the expection of adding a richer API in the future.

# CREATION
A storage instance can be created by the creation service. The creation message needs to contain a single port, which is used for command input.

# API
The Storage Container API has tree functions `set`, `get` and `delete`. All response
are sent to the response port of the corresponding command message.

## `command_code`
Messages to the creation container must start with only of the following ids
* `0` - Defines a `delete` message
* `1` - Defines a `get` message
* `2` - Defines a `set` message

## `delete`
Creates a new instance of a container. 

| Fiels | Type | Description |
|-------|------|-------------|
|code   | uint32  | The commands code | 
|key    | bytes?  | the remainder of the message is the key to delete |

## `get`
Get returns a value throught the response port for a given value

| Fiels | Type | Description |
|-------|------|-------------|
|code   | uint32  | The commands code | 
|key    | bytes?  | the remainder of the message is the key to fetch |

## `set`
Get returns a value throught the response port for a given value

| Fiels | Type | Description |
|-------|------|-------------|
|code   | uint32  | The commands code | 
|key_len| varint64| the length of the key |
|key    | bytes?  | the  key |
|value  | bytes?  | the remainder of the message is the data to save at the given key | 

## Error Code

TODO
