<!-- @@@title:Incoming Webhooks@@@ -->


# LogZilla Incoming Webhooks

API endpoint: `/api/events`

## Authentication

The webhook endpoint requires authentication. There are two variants of token based authentication:

- Header
- Via the `AUTHTOKEN` parameter used in a request URI

## Submitting messages

* Message Format

      {
        "message": "Logged message",
        "facility": "USER",
        "severity": "DEBUG",
        "host": "test-host",
        "program": "test-hook"
      }


## Fields

- message
 - required, message text
- facility
 - optional, facility name, `default: user`
- severity
 - optional, severity name, `default: info`
- host
 - optional, `default: localhost`
- program
 - optional, `default incoming-webhook`

## Sample POST using CURL

    curl -H 'Content-Type: application/json' -X POST -d  '{"message": "Test Message", "host": "testhost"}' 'http://dev.lzil.la/api/events?AUTHTOKEN=9128b06013779b84d7ae982973bf314a43aa58b5e6fa814f'
