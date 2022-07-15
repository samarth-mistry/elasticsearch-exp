#AGGREGATION with nested and QUERYING with aggs. + STATS

GET _search
{
  "query": {
    "match_all": {}
  }
}

GET /_search?q=tag.id:*

GET /_search?q=*

#value(includeing) and tag id unique

GET /data_energy/_search
{
  "aggs": {
    "time_ranged_tags": {
      "date_range": {
        "field": "event_timestamp",
        "ranges": [
          {
            "from": "2022-07-01",
            "to": "2022-07-01||-6M"
          }
        ]
      }
    }
  }
}
GET /data_energy/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "event_timestamp":
              {
                "lte": "2022-07-01",
                "gte": "2022-07-01||-6M"
              }
          }
        }
      ]
    }
  },
  "aggs": {
    "unique_terms": {
      "terms": {
        "field": "tag.id.keyword"
      }
    }
  }
}
#65041c1a-ad40-457f-bfcb-ebf297b61382 --> asset_id
GET /data_condition/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "asset_id.keyword": "65041c1a-ad40-457f-bfcb-ebf297b61382"
          }
        }
      ], 
      "filter": [
        {
          "range": {
            "event_timestamp":
              {
                "lte": "2022-07-01",
                "gte": "2022-07-01||-6M"
              }
          }
        }
      ]
    }
  },
  "aggs": {
    "unique_assets": {
      "terms": {
        "field": "asset_id.keyword"
      }
    }
  }
}

#all-assets
GET /data_condition/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "event_timestamp":
              {
                "lte": "2022-07-01",
                "gte": "2022-07-01||-6M"
              }
          }
        }
      ]
    }
  },
  "aggs": {
    "unique_assets": {
      "terms": {
        "field": "asset_id.keyword"
      }
    }
  }
}
#monitor.id/asset.id FILTER
GET /data_energy/_search
{
  "_source": ["tag.id","tag.value"], 
  "aggs": {
    "unique_terms": {
      "terms": {
        "field": "tag.id.keyword"
      },
      "aggs": {
        "time_range": {
          "date_range": {
            "field": "event_timestamp",
            "ranges": [
              {
                "from": "2022-07-01",
                "to": "2022-07-01||-6M"
              }
            ]
          }
        }
      }
    }
  }
}


GET /data_energy/_search
{
  "_source": ["tag.id","tag.value"], 
  "filter": { 
    "date_range": {
      "field": "event_timestamp",
      "ranges": [
        {
          "from": "2022-07-01",
          "to": "2022-07-01||-6M"
        }
      ]
    }
  },
  "aggs": {
    "unique_terms": {
      "terms": {
        "field": "tag.id.keyword"
      }
    }
  }
}

GET /data_energy/_search
{
  "_source": ["tag.id","tag.value"],
  "aggs": {
    "range": {
      "date_range": {
        "field": "event_timestamp",
        "format": "yyyy-MM-dd",
        "ranges": [
          {
            "from": "2022-07-01",
            "to": "2022-07-01||-6M"
          }
        ]
      }
    },
    "unique_terms": {
      "terms": {
        "field": "tag.id.keyword"
      }
    }
  }
}


GET /data_condition/_search
{
  "_source": ["tag.id","tag.value","event_timestamp"],
  "aggs": {
    "time_ranged_tags": {
      "date_range": {
        "field": "event_timestamp",
        "ranges": [
          {
            "from": "2022-07-01",
            "to": "2022-07-01||-6M"
          }
        ]
      }
    }
  }
}

GET /_search?pretty
{
  "aggs": {
    "time_ranged_tags": {
      "date_range": {
        "field": "event_timestamp",
        "ranges": [
          {
            "from": "2022-07-01",
            "to": "2022-07-01||-6M"
          }
        ]
      }
    }
  }
}
