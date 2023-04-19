You got your room schema:

```yaml
type: object
required:
- number
- bookable
properties:
number:
    type: string
    description: room number
    example: "A 1.02"
    minLength: 0
    maxLength: 20
name:
    type: string
    description: name of the room
    example: Aula
    minLength: 2
    maxLength: 255
bookable:
    type: boolean
    description: wether this room can be booked or not
    example: true
```

Then you got your OpenAPI:

```yaml
openapi: "3.0.0"
# ...
paths:
  /rooms/{roomId}:
    parameters:
      - $ref: "#/components/parameters/RoomId"
    get:
      # ...
      responses:
        "200":
          $ref: "#/components/responses/Room"
components:
  # ...
  schemas:
    Room:
      $ref: "schemas.yaml#/components/schemas/Room"
```

And your AsyncAPI:

```yaml
asyncapi: "2.0.0"
# ...
channels:
  evt.rooms.created:
    subscribe:
      summary: "An room was created."
      message:
        $ref: "#/components/messages/RoomCreated"
components:
    # ...
  schemas:
    RoomCloudEvent:
      allOf:
        - $ref: "defaults.yaml#/components/schemas/DefaultCloudEvent"
        - type: object
          properties:
            data:
              $ref: "schemas.yaml#/components/schemas/Room"
```