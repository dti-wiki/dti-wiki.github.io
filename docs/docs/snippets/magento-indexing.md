<a href="https://github.com/kholid060/MkDown2" target="_blank">
![Github](https://img.shields.io/github/stars/kholid060/MkDown2)
</a>
# MkDown

MkDown is an online markdown editor built with [vueJs](https://vuejs.org). How to use MkDown Markdown Editor:

- Type some markdown in left side
- See the preview on right side
- And Voilà

## Feature

- Import Markdown file from your pc
- Import your HTML file and convert it to markdown
- Export your document as a Markdown file, HTML or HTML styled file


# Magento Indexing

Indexing is how Magento transforms data such as products and categories, to improve the performance of your storefront. As data changes, the transformed data must be updated or reindexed. To optimize storefront performance, Magento accumulates data into special tables using indexers.

For example, if you change the price of an item from $4.99 to $3.99. Magento must reindex the price change to display it on your storefront.

## View a list of indexers

To view a list of all indexers:

```
bin/magento indexer:info
```

The list displays as follows:

```
design_config_grid                       Design Config Grid
customer_grid                            Customer Grid
catalog_category_product                 Category Products
catalog_product_category                 Product Categories
catalogrule_rule                         Catalog Rule Product
catalog_product_attribute                Product EAV
inventory                                Inventory
catalogrule_product                      Catalog Product Rule
cataloginventory_stock                   Stock
catalog_product_price                    Product Price
catalogsearch_fulltext                   Catalog Search
```

## View indexer status

Use this command to view the status of all indexers or specific indexers. For example, find out if an indexer needs to be reindexed.

```
bin/magento indexer:status [indexer]
```

To list status of all indexers:

```
bin/magento indexer:status
```

## To Reindex

Use this command to reindex all or selected indexers one time only.

```
bin/magento indexer:reindex [indexer]
```

To reindex all indexers:

```
bin/magento indexer:reindex
```

To use multiple threads for indexing use:

```
MAGE_INDEXER_THREADS_COUNT=16 php -dmemory_limit=8G bin/magento indexer:reindex
```

## Reset indexer

Use this command to invalidate the status of all indexers or specific indexers.

```
bin/magento indexer:reset [indexer]
```

To invalidate all indexers.

```
bin/magento indexer:reset
```


### References

- https://devdocs.magento.com/guides/v2.3/extension-dev-guide/indexing.html
- https://devdocs.magento.com/guides/v2.3/config-guide/cli/config-cli-subcommands-index.html
