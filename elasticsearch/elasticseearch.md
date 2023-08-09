- query context ve filter context olmak üzere 2 context var

- bool query 4 seçeneğe sahip. must(score u arttırır), should(or gibi çalışıyor, içindekilerden bir tanesi bile olsa çalışır), filter(must ile aynı sadece score u arttırmaz), must_not(score u arttırmaz)

**Term-level queries**

You can use **term-level queries** to find documents based on precise values in structured data. Examples of structured data include date ranges, IP addresses, prices, or product IDs.

Unlike [full-text queries](https://www.elastic.co/guide/en/elasticsearch/reference/current/full-text-queries.html "Full text queries"), term-level queries do not analyze search terms. Instead, term-level queries match the exact terms stored in a field.

**Term Query**

Returns documents that contain an **exact** term in a provided field.

You can use the `term` query to find documents based on a precise value such as a price, a product ID, or a username.

> Avoid using the `term` query for [`text`](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/text.html "Text datatype") fields.
> 
> By default, Elasticsearch changes the values of `text` fields as part of [analysis](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/analysis.html "Analysis"). This can make finding exact matches for `text` field values difficult.
> 
> To search `text` field values, use the [`match`](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/query-dsl-match-query.html "Match query") query instead.

**Terms Query**

Returns documents that contain one or more **exact** terms in a provided field.

The `terms` query is the same as the [`term` query](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/query-dsl-term-query.html "Term query"), except you can search for multiple values.

```json
//sample elastic query 
{
  "query": { 
    "bool": { 
      "must": [
        { "match": { "title":   "Search"        }},
        { "match": { "content": "Elasticsearch" }}
      ],
      "filter": [ 
        { "term":  { "status": "published" }},
        { "range": { "publish_date": { "gte": "2015-01-01" }}}
      ]
    }
  }
}
```
