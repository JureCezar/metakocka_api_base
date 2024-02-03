
# Concept

Let's start with example :

**Request** (POST - https://main.metakocka.si/rest/eshop/v1/search) :
```javascript
{
    "secret_key":"8899",
    "company_id":"16",
    "doc_type":"sales_bill_domestic",
    "query" : "john"
}
```

* you can search by one document type (doc\_type)
* use 'query' (some as search field in MetaKocka) or 'advance\_query' (some as advance search option in MetaKocka) to limit results.
* use 'limit' and 'offset' to perform query window
* you can get maximum of 100 records. For more use query window

**Respond** :
```javascript
{
    "opr_code": "0",
    "opr_time_ms": "185",
    "result_all_records": "1",
    "result_count": "1",
    "offset": "0",
    "limit": "1",
    "result": [
        {
            "mk_id": "1600203710",
            "opr_code": "0",
            "count_code": "PRD1_494"
        }
    ]
}
```

* 'result\_all\_records' is number of all possible records for given query
* result can be list of mk\_id and count code or list of whole document (see examples below)

# Parallel search
Search requests are always run in sequence. If calls are parallel, they will be put in a sequence queue and executed in sequence. If a company needs multiple request search requests in parallel (supported only on standalone servers), the following steps must be done :
1. Support must set parameter REST_MAX_SEARCH_LOCK_GROUP_PER_COMPANY in a set number of separate groups (for example: 2)
2. Customer must put in REST search call addition Header HTTP parameter (name "searchGroup" and value number 1 or 2). All calls in the given searchGroup will use separate locks for parallel execution.

Note: if a number of separate groups are set on value 2, you can use 3 parallel search calls: searchGroup = empty, 1, 2.
