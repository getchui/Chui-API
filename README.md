# Chui API Documentation

Hi everyone, and welcome on the **Chui REST API documentation**.

![Your Face is the Key](https://chui-web-app.appspot.com/static/img/positive.png)

You will find a `postman.json` attached, to compose your requests and test the endpoints out with [Postman](https://www.getpostman.com/).

You are invited to [report any problem you may encounter](https://github.com/getchui/Chui-API/issues).

### Response

The API responds with the following formatted object, under an appropriate [HTTP response code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

```json
RESPONSE
{
    "success": 0,
    "message": "An error message if any.",
    "data": ???
}
```

There will probably be no `data` property on error.

### Authentication

Send your API key as an `api-key` header for each of your requests.
For each request except **[POST] /api/token** (login) provide a valid `token` ([JWT](http://jwt.io)) header.

```json
REQUEST HEADER
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJyZWZyZXNoIjoicGhPTVd6dWFJYUFMUE5xenNVeHJ3VCIsInVzZXIiOnsiZW1haWwiOiJ1c2VyQGVtYWlsLmNvbSIsInVzZXJfaWQiOjU2Mjk0OTk1MzQyMTMxMjB9fQ.hjJoDcImvnxlSue9ITC101JnjHa_ivVIyzo9s1SJUrU",
    "api-key": "l0qo3d:x0SM89_8^8vgm!61_68kz2Z0Q",
    ...
}
```

Tokens will expire after 15 minutes. Use the endpoint **[PUT] /api/token** to [refresh them](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/).

## Token

* **[POST] /api/token**

    **Log In** – Authenticates a user from `email` and `password`. Returns the owner (user) `id` and `email`, a valid access `token` ([JWT](http://jwt.io)), and a [refresh token](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/) to keep safely.
    ```json
    REQUEST
    {
        "email": "user@email.com",
        "password": "password"
    }
    RESPONSE
    {
        "email": "user@email.com",
        "expire_time": 1486058659,
        "refresh": "Q9P9viaF3LGvN5jhlnDWib",
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJyZWZyZXNoIjoiUTlQOXZpYUYzTEd2TjVqaGxuRFdpYiIsInVzZXIiOnsiZW1haWwiOiJ1c2VyQGVtYWlsLmNvbSIsInVzZXJfaWQiOjU2Mjk0OTk1MzQyMTMxMjB9LCJleHAiOjE0ODYwNTg2NTkuMH0.hpvmBDEamagM6Np0YlqBETsmt4bhq4Qto7aErG7veso",
        "user_id": 5629499534213120
    }
    ```

* **[GET] /api/token**

    Returns the logged user info...
    ```json
    RESPONSE
    {
        "email": "user@email.com",
        "user_id": 5629499534213120
    }
    ```

* **[PUT] /api/token**

    **Refresh Token** – Returns a valid access `token` ([JWT](http://jwt.io)) and a [refresh token](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/) if token is indeed expired.
    ```json
    REQUEST
    {
        "refresh": "Q9P9viaF3LGvN5jhlnDWib"
    }
    RESPONSE
    {
        "email": "user@email.com",
        "expire_time": 1486059121,
        "refresh": "5Qe1HbjNgxXDtSP3b3bjpE",
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7ImVtYWlsIjoidXNlckBlbWFpbC5jb20iLCJ1c2VyX2lkIjo1NjI5NDk5NTM0MjEzMTIwfSwicmVmcmVzaCI6IjVRZTFIYmpOZ3hYRHRTUDNiM2JqcEUifQ.fH--hP3ltZcbYc56inenLFQ75qR6w1vdQbwE3jKinQU",
        "user_id": 5629499534213120
    }
    ```

## Device

* **[GET] /api/device**

    Returns configuration of all devices – as a list – for the user specified in the authentication `token`.
    ```json
    RESPONSE
    [
        {
            "date": "2017-02-02T19:42:58.831819",
            "fail_safe": 1,
            "id": "08b31508-967b-420c-b4bf-d190e306d1e2",
            "name": "08b31508-967b-420c-b4bf-d190e306d1e2",
            "nfc": 0,
            "notifications": 1,
            "opentok": {
                "expire_time": "2017-03-04T19:42:54.345389",
                "session_id": "1_MX40NTY4ODk1Mn5-MTQ4NjA2NDU3ODczNH54ejZzUDkzWU9XUmZJRGIycFlUWjhCK0t-UH4",
                "token_app": "T1==cGFydG5lcl9pZD00NTY4ODk1MiZzaWc9MDEzZDAwNWZmY2EyNDkwZTJhN2UzYzM5MTc3MDExYzUzN2I1YWY3NDpjb25uZWN0aW9uX2RhdGE9YXBwJnNlc3Npb25faWQ9MV9NWDQwTlRZNE9EazFNbjUtTVRRNE5qQTJORFUzT0Rjek5INTRlalp6VURreldVOVhVbVpKUkdJeWNGbFVXamhDSzB0LVVINCZub25jZT02MzMyMzAmY3JlYXRlX3RpbWU9MTQ4NjA2NDU3OCZyb2xlPXB1Ymxpc2hlciZleHBpcmVfdGltZT0xNDg4NjU2NTc0",
                "token_device": "T1==cGFydG5lcl9pZD00NTY4ODk1MiZzaWc9YzU4ODExYzVjNGYzOTg5OGM3MWVkNGU3MmZhNTA4MDZjOTJlNTNjOTpjb25uZWN0aW9uX2RhdGE9ZGV2aWNlJnNlc3Npb25faWQ9MV9NWDQwTlRZNE9EazFNbjUtTVRRNE5qQTJORFUzT0Rjek5INTRlalp6VURreldVOVhVbVpKUkdJeWNGbFVXamhDSzB0LVVINCZub25jZT0zOTY2OTQmY3JlYXRlX3RpbWU9MTQ4NjA2NDU3OCZyb2xlPXB1Ymxpc2hlciZleHBpcmVfdGltZT0xNDg4NjU2NTc0"
            },
            "relay_ip": null,
            "unlock_duration": 8,
            "video": 1
        },
        ...
    ]
    ```

* **[GET] /api/device?id={device_id}**

    Returns a single device (by `id`).
    ```json
    RESPONSE
    {
        "date": "2017-02-02T19:42:58.831819",
        "fail_safe": 1,
        "id": "08b31508-967b-420c-b4bf-d190e306d1e2",
        "name": "08b31508-967b-420c-b4bf-d190e306d1e2",
        "nfc": 0,
        "notifications": 1,
        "opentok": {
            "expire_time": "2017-03-04T19:42:54.345389",
            "session_id": "1_MX40NTY4ODk1Mn5-MTQ4NjA2NDU3ODczNH54ejZzUDkzWU9XUmZJRGIycFlUWjhCK0t-UH4",
            "token_app": "T1==cGFydG5lcl9pZD00NTY4ODk1MiZzaWc9MDEzZDAwNWZmY2EyNDkwZTJhN2UzYzM5MTc3MDExYzUzN2I1YWY3NDpjb25uZWN0aW9uX2RhdGE9YXBwJnNlc3Npb25faWQ9MV9NWDQwTlRZNE9EazFNbjUtTVRRNE5qQTJORFUzT0Rjek5INTRlalp6VURreldVOVhVbVpKUkdJeWNGbFVXamhDSzB0LVVINCZub25jZT02MzMyMzAmY3JlYXRlX3RpbWU9MTQ4NjA2NDU3OCZyb2xlPXB1Ymxpc2hlciZleHBpcmVfdGltZT0xNDg4NjU2NTc0",
            "token_device": "T1==cGFydG5lcl9pZD00NTY4ODk1MiZzaWc9YzU4ODExYzVjNGYzOTg5OGM3MWVkNGU3MmZhNTA4MDZjOTJlNTNjOTpjb25uZWN0aW9uX2RhdGE9ZGV2aWNlJnNlc3Npb25faWQ9MV9NWDQwTlRZNE9EazFNbjUtTVRRNE5qQTJORFUzT0Rjek5INTRlalp6VURreldVOVhVbVpKUkdJeWNGbFVXamhDSzB0LVVINCZub25jZT0zOTY2OTQmY3JlYXRlX3RpbWU9MTQ4NjA2NDU3OCZyb2xlPXB1Ymxpc2hlciZleHBpcmVfdGltZT0xNDg4NjU2NTc0"
        },
        "relay_ip": null,
        "unlock_duration": 8,
        "video": 1
    }
    ```

* **[PUT] /api/device**

    Updates an existing device. Returns full modified entity. The `id ` parameter is required. Will trigger an `update` **PubNub event**.
    ```json
    REQUEST
    {
        "id": "08b31508-967b-420c-b4bf-d190e306d1e2",
        "fail_safe": 0,
        "name": "My Updated Device",
        "nfc": 1,
        "notifications": 0,
        "relay_ip": "10.0.0.128",
        "unlock_duration": 4,
        "video": 0
    }
    RESPONSE
    {
        "date": "2017-02-01T22:59:31.816680",
        "fail_safe": 0,
        "id": "08b31508-967b-420c-b4bf-d190e306d1e2",
        "name": "My Updated Device",
        "nfc": 1,
        "notifications": 0,
        "opentok": {
            "expire_time": "2017-03-03T22:59:25.668831",
            "session_id": "2_MX40NTY4ODk1Mn5-MTQ4NTk4OTk3MTA4MH54bmZlNG5sSThNV2NjNG5RNzhWUHo4djF-UH4",
            "token_app": "T1==cGFydG5lcl9pZD00NTY4ODk1MiZzaWc9ZTM1NDAwZjFhMTBjYmUxYTJmM2VjNzExNGMxMTVhMTQyZWM1MGE1MDpleHBpcmVfdGltZT0xNDg4NTgxOTY1JnJvbGU9cHVibGlzaGVyJmNvbm5lY3Rpb25fZGF0YT1hcHAmY3JlYXRlX3RpbWU9MTQ4NTk4OTk3MSZub25jZT0zNjAzNTkmc2Vzc2lvbl9pZD0yX01YNDBOVFk0T0RrMU1uNS1NVFE0TlRrNE9UazNNVEE0TUg1NGJtWmxORzVzU1RoTlYyTmpORzVSTnpoV1VIbzRkakYtVUg0",
            "token_device": "T1==cGFydG5lcl9pZD00NTY4ODk1MiZzaWc9ZTcyN2RiMGE2ZTkxZWQ3NGQ5Mzk2MmY1ZmVkYmZhYTYxZjRiM2M5ZTpleHBpcmVfdGltZT0xNDg4NTgxOTY1JnJvbGU9cHVibGlzaGVyJmNvbm5lY3Rpb25fZGF0YT1kZXZpY2UmY3JlYXRlX3RpbWU9MTQ4NTk4OTk3MSZub25jZT03NDA0NTImc2Vzc2lvbl9pZD0yX01YNDBOVFk0T0RrMU1uNS1NVFE0TlRrNE9UazNNVEE0TUg1NGJtWmxORzVzU1RoTlYyTmpORzVSTnpoV1VIbzRkakYtVUg0"
        },
        "relay_ip": "10.0.0.128",
        "unlock_duration": 4,
        "video": 0
    }
    PUBNUB `update` PAYLOAD for DEVICE
    {
        "kind": "device",
        "entity": REQUEST OBJECT
    }
    ```


## Snapshot

* **[GET] /api/snapshot**

    Returns snapshots entities as a list for logged user's devices.
    ```json
    RESPONSE
    [
        {
            "date": "2017-02-03T14:35:05.221102",
            "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
            "fir": null,
            "id": "db372f4d-68a0-4448-926d-79d31bf5c8d9",
            "image": "http://localhost:8080/_ah/img/encoded_gs_file:Y2h1aS1pbWFnZXMvc25hcHNob3QtZGIzNzJmNGQtNjhhMC00NDQ4LTkyNmQtNzlkMzFiZjVjOGQ5",
            "keyholder": null
        },
        ...
    ]
    ```

* **[GET] /api/snapshot?id={snapshot_id}**

    Returns a single snapshot (by `id`).
    ```json
    RESPONSE
    {
        "date": "2017-02-03T14:35:05.221102",
        "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
        "fir": null,
        "id": "db372f4d-68a0-4448-926d-79d31bf5c8d9",
        "image": "http://localhost:8080/_ah/img/encoded_gs_file:Y2h1aS1pbWFnZXMvc25hcHNob3QtZGIzNzJmNGQtNjhhMC00NDQ4LTkyNmQtNzlkMzFiZjVjOGQ5",
        "keyholder": null
    }
    ```

* **[GET] /api/snapshot?device={device_id} & /api/snapshot?keyholder={keyholder_id}**

    Returns a single snapshot (by `device_id` or `keyholder_id`).
    ```json
    RESPONSE
    [
        {
            "date": "2017-02-03T14:35:05.221102",
            "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
            "fir": "c453e416-405e-4c1d-80d7-3d0b12f061dc",
            "id": "db372f4d-68a0-4448-926d-79d31bf5c8d9",
            "image": "http://localhost:8080/_ah/img/encoded_gs_file:Y2h1aS1pbWFnZXMvc25hcHNob3QtZGIzNzJmNGQtNjhhMC00NDQ4LTkyNmQtNzlkMzFiZjVjOGQ5",
            "keyholder": "2771925f-c06f-4bb0-8061-67a04dc1f6ce"
        },
        ...
    ]
    ```

* **[PUT] /api/snapshot**

    **Tag Person** – Updates an existing snapshot. Returns full modified entity. Only the `id ` property is required. `device_id` and `image` should not be changed.
    ```json
    REQUEST
    {
        "id": "db372f4d-68a0-4448-926d-79d31bf5c8d9",
        "keyholder": "2771925f-c06f-4bb0-8061-67a04dc1f6ce"
    }
    RESPONSE
    {
        "date": "2017-02-03T14:35:05.221102",
        "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
        "fir": "c453e416-405e-4c1d-80d7-3d0b12f061dc",
        "id": "db372f4d-68a0-4448-926d-79d31bf5c8d9",
        "image": "http://localhost:8080/_ah/img/encoded_gs_file:Y2h1aS1pbWFnZXMvc25hcHNob3QtZGIzNzJmNGQtNjhhMC00NDQ4LTkyNmQtNzlkMzFiZjVjOGQ5",
        "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2"
    }
    ```


## Keyholder

* **[POST] /api/keyholder**

    Creates a new keyholder from a snapshot. Returns the created keyholder entity with its attributed `id`. Will trigger a `wm_enroll` **PubNub event**.
    ```json
    REQUEST
    {
        "name": "My Keyholder",
        "snapshot": "db372f4d-68a0-4448-926d-79d31bf5c8d9"
    }
    RESPONSE
    {
        "access": [
            {
                "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
                "id": "b5d97af7-bbf4-4385-a72f-1213aa9cd854",
                "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
                "type": 0
            },
            ...
        ],
        "date": "2017-02-03T16:46:30.093696",
        "id": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
        "images": [
            "http://localhost:8080/_ah/img/encoded_gs_file:Y2h1aS1pbWFnZXMvc25hcHNob3QtZGIzNzJmNGQtNjhhMC00NDQ4LTkyNmQtNzlkMzFiZjVjOGQ5"
        ],
        "name": "My Keyholder"
    }
    PUBNUB `wm_enroll` PAYLOAD for DEVICE
    {
        "fir_id": "c453e416-405e-4c1d-80d7-3d0b12f061dc",
        "keyholder_id": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2"
    }
    ```

* **[GET] /api/keyholder**

    Returns all the keyholders entities as a list, for the user specified in the auth token.
    ```json
    RESPONSE
    [
        {
            "access": [
                {
                    "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
                    "id": "b5d97af7-bbf4-4385-a72f-1213aa9cd854",
                    "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
                    "type": 0
                },
                ...
            ],
            "date": "2017-02-03T16:46:30.093696",
            "id": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
            "images": [
                "http://localhost:8080/_ah/img/encoded_gs_file:Y2h1aS1pbWFnZXMvc25hcHNob3QtZGIzNzJmNGQtNjhhMC00NDQ4LTkyNmQtNzlkMzFiZjVjOGQ5",
                ...
            ],
            "name": "My Keyholder"
        },
        ...
    ]
    ```

* **[GET] /api/keyholder?id={keyholder_id}**

    Returns a single keyholder (by `id`).
    ```json
    RESPONSE
    {
        "access": [
            {
                "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
                "id": "b5d97af7-bbf4-4385-a72f-1213aa9cd854",
                "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
                "type": 0
            },
            ...
        ],
        "date": "2017-02-03T16:46:30.093696",
        "id": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
        "images": [
            "http://localhost:8080/_ah/img/encoded_gs_file:Y2h1aS1pbWFnZXMvc25hcHNob3QtZGIzNzJmNGQtNjhhMC00NDQ4LTkyNmQtNzlkMzFiZjVjOGQ5",
            ...
        ],
        "name": "My Keyholder"
    }
    ```

* **[PUT] /api/keyholder**

    Updates an existing keyholder. Returns full modified entity. Only the `id ` property is required.
    ```json
    REQUEST
    {
        "id": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
        "name": "My Updated Keyholder"
    }
    RESPONSE
    {
        "access": [
            {
                "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
                "id": "b5d97af7-bbf4-4385-a72f-1213aa9cd854",
                "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
                "type": 0
            },
            ...
        ],
        "date": "2017-02-03T16:46:30.093696",
        "id": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
        "images": [
            "http://localhost:8080/_ah/img/encoded_gs_file:Y2h1aS1pbWFnZXMvc25hcHNob3QtZGIzNzJmNGQtNjhhMC00NDQ4LTkyNmQtNzlkMzFiZjVjOGQ5",
            ...
        ],
        "name": "My Updated Keyholder"
    }
    ```


## FIR

Stands for *Facial Identity Recognition*.

* **[GET] /api/fir**

    Returns all the FIRs entities as a list.
    ```json
    RESPONSE
    [
        {
            "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
            "hash": HASH,
            "id": "c453e416-405e-4c1d-80d7-3d0b12f061dc",
            "image": "http://localhost:8080/_ah/img/encoded_gs_file:Y2h1aS1pbWFnZXMvc25hcHNob3QtZGIzNzJmNGQtNjhhMC00NDQ4LTkyNmQtNzlkMzFiZjVjOGQ5",
            "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2"
        },
        ...
    ]
    ```

* **[GET] /api/fir?id={fir_id}**

    Returns a single FIR (by `id`).
    ```json
    RESPONSE
    {
        "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
        "hash": HASH,
        "id": "c453e416-405e-4c1d-80d7-3d0b12f061dc",
        "image": "http://localhost:8080/_ah/img/encoded_gs_file:Y2h1aS1pbWFnZXMvc25hcHNob3QtZGIzNzJmNGQtNjhhMC00NDQ4LTkyNmQtNzlkMzFiZjVjOGQ5",
        "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2"
    }
    ```

* **[GET] /api/fir?keyholder={keyholder_id} & /api/fir?device={device_id}**

    Returns a list of FIRs (by `keyholder_id` or `device_id`).

    ```json
    RESPONSE
    [
        {
            "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
            "hash": HASH,
            "id": "c453e416-405e-4c1d-80d7-3d0b12f061dc",
            "image": "http://localhost:8080/_ah/img/encoded_gs_file:Y2h1aS1pbWFnZXMvc25hcHNob3QtZGIzNzJmNGQtNjhhMC00NDQ4LTkyNmQtNzlkMzFiZjVjOGQ5",
            "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2"
        },
        ...
    ]
    ```


## Access

Types can be `1` (*permanent*) or `0` (*no access*). More types of accesses (temporary, repeated...) will be implemented in a future release.

You can fetch access straight from the keyholder objects, but these endpoints are here too.

* **[GET] /api/access**

    Returns all the accesses entities as a list, for the user specified in the auth token.
    ```json
    RESPONSE
    [
        {
            "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
            "id": "b5d97af7-bbf4-4385-a72f-1213aa9cd854",
            "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
            "type": 0
        },
        ...
    ]
    ```

* **[GET] /api/access?id={access_id}**

    Returns a single access (by `id`).
    ```json
    RESPONSE
    {
        "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
        "id": "b5d97af7-bbf4-4385-a72f-1213aa9cd854",
        "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
        "type": 0
    }
    ```

* **[GET] /api/access?device={device_id} & /api/access?keyholder={keyholder_id}**

    Returns a list of accesses (by `device_id` or `keyholder_id`).

    ```json
    RESPONSE
    [
        {
            "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
            "id": "b5d97af7-bbf4-4385-a72f-1213aa9cd854",
            "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
            "type": 0
        },
        ...
    ]
    ```

* **[PUT] /api/access**

    Updates an existing access. Returns full modified entity. Only the `id ` property is required. Will trigger an `update` **PubNub event**.
    ```json
    REQUEST
    {
        "id": "b5d97af7-bbf4-4385-a72f-1213aa9cd854",
        "type": 1
    }
    RESPONSE
    {
        "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
        "id": "b5d97af7-bbf4-4385-a72f-1213aa9cd854",
        "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
        "type": 1
    }
    PUBNUB `update` PAYLOAD for DEVICE
    {
        "kind": "access",
        "entity": {
            "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
            "id": "b5d97af7-bbf4-4385-a72f-1213aa9cd854",
            "keyholder": "f0f4ceb1-7064-43e3-90c2-c4b4b0b067a2",
            "type": 1
        }
    }
    ```

## Log

* **[GET] /api/log?device={device_id}**

    Returns a list of logs (by `device_id`).
    ```json
    RESPONSE
    [
        {
            "content": "This is some log content",
            "date": "2017-02-06T15:14:59.494315",
            "device": "08b31508-967b-420c-b4bf-d190e306d1e2",
            "id": "33806a75-bbcf-4a10-9ad9-8e5386158535"
        },
        ...
    ]
    ```


## Command

* **[POST] /api/command**

    Publishes a command on **PubNub**. Also accepts `data` to send in payload.
    ```json
    REQUEST
    {
        "event": "unlock",
        "device_id": "08b31508-967b-420c-b4bf-d190e306d1e2"
    }
    RESPONSE
    {
        "device_id": "08b31508-967b-420c-b4bf-d190e306d1e2",
        "payload": {}
    }
    ```


## Analytics

* **[POST] /api/analytics**

    Returns statistics. Requires a `date` (*YYYY-MM-DD* or *ISO 8601*) and a `range` (either **day**, **month** or **year**). *Day* results will return day, month and year data, *month* results will return month and year data, and *year* only year data.
    ```json
    REQUEST
    {
        "date": "2016-11-17",
        "range": "day"
    }
    RESPONSE
    {
        "day": {
            "data": [
                {
                    "key": "2016-11-17 00:00",
                    "total": 12,
                    "08b31508-967b-420c-b4bf-d190e306d1e2": 4,
                    ...
                },
                ...
            ],
            "label": "11/17/16"
        },
        "month": {
            "data": [
                {
                    "key": "2016-11-01",
                    "total": 48,
                    "08b31508-967b-420c-b4bf-d190e306d1e2": 16,
                    ...
                },
                ...
            ],
            "label": "November 2016"
        },
        "year": {
            "data": [
                {
                    "key": "2016-01",
                    "total": 192,
                    "08b31508-967b-420c-b4bf-d190e306d1e2": 64,
                    ...
                },
                ...
            ],
            "label": 2016
        }
    }
    ```
