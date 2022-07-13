### What is AsyncAPI?

  - specification for describing application's API
  - machine-readable YAML and JSON -> Check in [the Studio](https://studio.asyncapi.com/)
  - very [similar to OpenAPI](https://www.asyncapi.com/docs/getting-started/coming-from-openapi)

### What is AsyncAPI Initiative?

  - Linux Foundation project -> [Charter](https://github.com/asyncapi/community/blob/master/CHARTER.md)
  - Open governance -> Anyone can join [Technical Steering Committee](https://www.asyncapi.com/community/tsc)
  - [Specification](https://www.asyncapi.com/docs/specifications/v2.4.0) and [tools](https://www.asyncapi.com/docs/community/tooling)

### What are specification key concepts?

#### Basic

  - AsyncAPI is for describing the application interface, not the broker
  - EDA is not broker only, thus AsyncAPI is not only for broker-centric architectures
  - Channels are topics
  - [Publish/subscribe](https://www.asyncapi.com/blog/publish-subscribe-semantics) operations

#### Flexibility

  - Protocol agnostic aka [`bindings` feature](https://github.com/asyncapi/bindings). Like with [MQTT](https://github.com/asyncapi/bindings/tree/master/mqtt) when you connect to it, [`clientId`](https://github.com/asyncapi/spec/blob/v2.4.0/examples/social-media/comments-service/asyncapi.yaml#L14) must be provieded
  - Multiple message schema formats (`schemaFormat`) aka `"Just reuse what you already have defined in other systems"` -> [AsyncAPI Schema](https://www.asyncapi.com/docs/specifications/v2.4.0#schemaObject), [JSON Schema](https://json-schema.org/), [OpenAPI](https://github.com/OAI/OpenAPI-Specification/blob/3.0.0/versions/3.0.0.md#data-types), [RAML DT](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md#raml-data-types), [Avro](https://avro.apache.org/docs/current/spec.html)
      ```
      //example message data - GOOD
      {
        "displayName": "Shrek from the swamp",
        "email": "shrek@swamp.com",
        "age": 39
      }
      //example message data - BAD
      {
        "displayName": "Shrek from the swamp",
        "email": "shrek@swamp.com",
        "age": "39y"
      }
      ```
      ```
      //json schema
      {
        "description" : "User information",
        "type" : "object",
        "additionalProperties": false,
        "properties" : {
          "displayName" : {
            "type" : "string"
          },
          "email" : {
            "type" : "string"
          },
          "age" : {
            "type" : "integer",
          }
       }
      }   
      ```
      ```
      //asyncapi schema
      {
        "description" : "**User** information",
        "type" : "object",
        "additionalProperties": false,
        "properties" : {
          "displayName" : {
            "type" : "string"
          },
          "email" : {
            "type" : "string"
          },
          "age" : {
            "type" : "integer",
          }
       }
      }   
      ```
      ```
      //avro
      {
        "type": "record",
        "name": "User",
        "namespace": "com.company",
        "doc": "User information",
        "fields": [
          {
            "name": "displayName",
            "type": "string"
          },
          {
            "name": "email",
            "type": "string"
          },
          {
            "name": "age",
            "type": "int"
          }
        ]
      }
      ```
  - Extensibility through `x-` properties. Useful when there is something not available in the spec, like [specifying a response](https://www.asyncapi.com/blog/websocket-part2#describe-responses---specification-extensions)
  
#### Info structure optimization

  - Traits for common info inside AsyncAPI document. Like with [this Kafka example](https://github.com/asyncapi/spec/blob/v2.4.0/examples/streetlights-kafka.yml#L159)
  - Sharing of information between components and files with [JSON References](https://datatracker.ietf.org/doc/html/draft-pbryan-zyp-json-ref-03)(`$ref`) as in [this social media example](https://github.com/asyncapi/spec/blob/v2.4.0/examples/social-media/comments-service/asyncapi.yaml)