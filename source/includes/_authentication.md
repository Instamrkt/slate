# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Instamrkt uses API keys to allow access to the API. You can register a new Instamrkt API key at our [developer portal](http://instamrkt.com/).

Instamrkt expects for the API key to be included in all API requests to the server in a header that looks like the following:

`header: YOUR_INSTAMRKT_API_KEY`

<aside class="notice">
You must replace `YOUR_INSTAMRKT_API_KEY` with your personal API key.
</aside>

<aside class="success">
Remember — we care about security for your own good!
</aside>

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>