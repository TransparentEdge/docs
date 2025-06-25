# Manage lists via API

Lists can also be managed using our [API](https://docs.transparentedge.eu/guides/api).

Please check our [API docs](https://api.transparentcdn.com/docs) for the details of the endpoints.

### Query lists <a href="#query-lists" id="query-lists"></a>

* GET `/v1/companies/<COMPANY_ID>/lists`
* GET `/v1/companies/<COMPANY_ID>/lists/<LIST_ID>`

### Create a list <a href="#create-a-list" id="create-a-list"></a>

* POST `/v1/companies/<COMPANY_ID>/lists`

Example payload, always use the service id 5, as that is the service for auto-provision:

```
{
  "addresses": [],
  "description": "my deny list",
  "name": "my_list",
  "services": [
    {
      "id": 5
    }
  ]
}
```

### Update a list <a href="#update-a-list" id="update-a-list"></a>

#### **Add IPs**

* POST `/v1/companies/<COMPANY_ID>/lists/<LIST_ID>/<IP>/<PREFIX>`
  * Example: `/v1/companies/4/lists/25/1.1.1.1/32`

#### **Delete IPs**

* DELETE `/v1/companies/<COMPANY_ID>/lists/<LIST_ID>/<IP>/<PREFIX>`
