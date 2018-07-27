# Indicators

An Indicator represents an atomic piece of information that has some intelligence value. Indicators are guaranteed to be unique within an Owner. For example, a single Organization can have only one copy of the Indicator `badguy@example.com`.

```eval_rst
.. note:: When making requests for URL Indicators where the URL itself is included in the request (for example, when retrieving information about a specific URL), you will need to `URL encode <https://wikipedia.org/wiki/Percent-encoding>`_ the Indicator. For example, the URL ``http://example.com/`` must be encoded like ``http%3A%2F%2Fexample.com%2F`` before it is sent in a request.
```

- [Retrieve Indicators](retrieve.md)
- [Create Indicators](create.md)
- [Update Indicators](update.md)
- [Delete Indicators](delete.md)
- [Indicator to Indicator Associations](indicator_to_indicator_assoc.md)
- [Private Indicators](private_indicators.md)
- [Bulk Download](bulk_download.md)
- [Batch Commit](batch_commit.md)
