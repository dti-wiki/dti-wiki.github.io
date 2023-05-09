# Magento Cloud Elasticsearch Management

The elasticsearch in magento cloud maight require manual maintenanace from time to time. This document list the useful commands to manage the magento cloud elasticsearch instance using CURL http client.

## Why?

There can be several potential causes. One cause is the Elasticsearch instance running out of disk space. Another cause is duplicated indices.

## Word of advice!

1. Create a fresh mysql dump before following these steps
2. Perform this operations outside of business hours
3. On production environments enable maintenanace mode if possible
4. On production environments disble the magento cron jobs till this activity is completed.

## Procedure

### List Indices

List the indices on the elasticsearch instance

```
curl -s -X GET localhost:9200/_cat/indices?v
```

This should give something lie this:

```
health status index                        uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   magento2_stg_product_16_v3   ro_35-TIQ5KT9H4ggVOwZw   3   2          0            0        2kb           705b
green  open   magento2_stg_product_20_v94  MPZqxSLQQweP_YwjuIeTWg   3   2         19            4    955.9kb        318.6kb
green  open   magento2_stg_product_18_v208 X7FehtL5RROo_crbnHYCxw   3   2         19            0    504.1kb          168kb
green  open   magento2_stg_product_17_v4   lb-6wI_PTr-9hKVWWIQYjA   3   2          0            0        2kb           693b
green  open   magento2_stg_product_6_v109  5ibsu0JmT9uOAS98bDS3Tg   3   2         61            0    201.3kb         64.6kb
green  open   magento2_stg_product_1_v3552 fWMK8iSzRjeub_z58sT5gg   3   2        552           11     25.8mb          8.4mb
green  open   magento2_stg_product_15_v125 HwztB4diTH2fQRGWwKS14A   3   2         21            3      1.6mb        547.8kb
green  open   magento2_stg_product_9_v179  paVM9hHvT32ZG9jDNipNRg   3   2          0            0      1.8kb           624b
green  open   magento2_stg_product_14_v498 _W2lSyEsQdWChM6kMro6Xw   3   2         21            0      1.1mb        398.2kb
green  open   magento2_stg_product_5_v1094 2QfuKR42Q8q73oIsCJmNow   3   2        447            0     16.4mb          5.2mb
green  open   magento2_product_1_v1        3irjvym7T5ycPgeQV0pOcg   3   2          0            0      2.2kb           783b
```

### Delete Indices

```
curl -XDELETE localhost:9200/[your_index_name_here]
```

Then do a full reindexing.

## Source:

- https://support.magento.com/hc/en-us/articles/360039837952-Elasticsearch-Index-Status-is-yellow-or-red-
- https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-index-status-is-yellow-or-red.html?lang=en
