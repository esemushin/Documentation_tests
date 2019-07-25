Webhooks allow you to expose custom logic as a regular REST URL. They can be useful if you integrate with a 3rd party service that posts data back to your app using a specified URL. For example, a payment processing service such as Stripe or Coinbase Commerce notify your app of successful payment by calling a URL.

Webhooks have optional parameter `path` that allow you to manually specify the final URL fragment. By default it equals the function name.
[block:code]
{
  "codes": [
    {
      "code": "functions:\n  paymentWebhook:\n    handler:\n      code: src/paymentWebhook.js\n    type: webhook\n    path: webhook_url #optional, default: function name\n    method: POST",
      "language": "yaml",
      "name": "8base.yml"
    },
    {
      "code": "module.exports = event => {\n  const payload = event.args.event;\n  if (payload.type == 'charge:confirmed') {\n    // Process completed payment\n    ...\n  }\n  \n  return {\n    statusCode: 200,\n    body: JSON.stringify({ success: true })\n  }\n};",
      "language": "javascript",
      "name": "paymentWebhook.js"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Input and output"
}
[/block]
Webhook `event` object has the following structure:
[block:code]
{
  "codes": [
    {
      "code": "event: {\n  data: {\n    arg1: \"arg1 value\",\n    arg2: \"arg2 value\"\n  },\n  headers: {\n    \"x-header-1\": \"header value\"\n  },\n  body: \"raw request body\"\n}",
      "language": "json",
      "name": "event"
    }
  ]
}
[/block]
8base attempts to parse request body and query string and add parsed values to `event.data`. You can get raw request body from `event.body`.

The format of the returned value has the following structure:
[block:code]
{
  "codes": [
    {
      "code": "return {\n  statusCode: 200, // statusCode is required\n  headers: {\n    \"x-custom-header\" : \"My Header Value\"\n  },\n  body: JSON.stringify({ message: \"Hello World!\" })\n}",
      "language": "javascript",
      "name": "return"
    }
  ]
}
[/block]
You have full control over the returned HTTP status code, headers and response body.
[block:callout]
{
  "type": "info",
  "title": "Getting webhook URL",
  "body": "In order to get your webhook URL after you have deployed it run `8base describe` to see the URL."
}
[/block]