# Campaigns

The Campaign Group represents a collection of Incidents over time.

## Retrieve Campaigns

### Retrieving a Single Campaign

This example demonstrates how to retrieve a specific Campaign using the Campaign's ID. The `add_id` filter specifies the ID of the Campaign which you would like to retrieve.

```eval_rst
.. code-block:: python
    :emphasize-lines: 10-12,15-16

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Campaigns object
    campaigns = tc.campaigns()

    # set a filter to retrieve only the Campaign with ID: 123456
    filter1 = campaigns.add_filter()
    filter1.add_id(123456)

    try:
        # retrieve the Campaign
        campaigns.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the retrieved Campaign (in this case there should only be one) and print its properties
    for campaign in campaigns:
        print(campaign.id)
        print(campaign.name)
        print(campaign.date_added)
        print(campaign.weblink)
        print('')
```

### Retrieving Multiple Campaigns

This example demonstrates how to retrieve Campaigns while applying filters. Two filters are added: one for the Owner and another for a Tag. The result set returned from this example will contain all Campaigns in the "Example Community" Owner that have the `Nation State` Tag.

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Campaigns object
    campaigns = tc.campaigns()

    owner = 'Example Community'

    # set a filter to only retrieve Campaigns in the 'Example Community' tagged: 'Nation State'
    filter1 = campaigns.add_filter()
    filter1.add_owner(owner)
    filter1.add_tag('Nation State')

    try:
        # retrieve the Campaigns
        campaigns.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the retrieved Campaigns and print their properties
    for campaign in campaigns:
        print(campaign.id)
        print(campaign.name)
        print(campaign.date_added)
        print(campaign.weblink)
        print('')
```

```eval_rst
.. note:: The ``filter1`` object contains a ``filters`` property that provides a list of supported filters for the resource type being retrieved. To display this list, ``print(filter1.filters)`` can be used. For more on using filters see the `Advanced Filter Tutorial <https://docs.threatconnect.com/en/latest/python/advanced.html#advanced-filtering>`__.
```

## Create Campaigns

The example below demonstrates how to create a Campaign Resource in the ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,25-26

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Campaigns object
    campaigns = tc.campaigns()

    owner = 'Example Community'

    # create a new Campaign in 'Example Community' with the name: 'New Campaign'
    campaign = campaigns.add('New Campaign', owner)
    # set the first seen date for the Campaign
    campaign.set_first_seen('2017-05-21T00:00:00Z')  # OPTIONAL

    # add a description attribute
    campaign.add_attribute('Description', 'Description Example')
    # add a tag
    campaign.add_tag('Example')
    # add a security label
    campaign.set_security_label('TLP Green')

    try:
        # create the Campaign - no API calls are made until this line
        campaign.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

**Supported Properties**

+---------------+------------------+----------+
| Property Name | Method           | Required |
+===============+==================+==========+
| first\_seen   | set\_first\_seen | False    |
+---------------+------------------+----------+

## Update Campaigns

The example below demonstrates how to update a Campaign Resource in the ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,20-21

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Campaigns object
    campaigns = tc.campaigns()

    owner = 'Example Community'

    # create Campaign with updated name
    campaign = campaigns.add('Updated Campaign', owner)
    # set the ID of the new Campaign to the ID of the existing Campaign you want to update
    campaign.set_id(123456)

    # you can update the Campaign metadata as described here: https://docs.threatconnect.com/en/latest/python/groups/groups.html#group-metadata

    try:
        # update the Campaign - no API calls are made until this line
        campaign.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

## Delete Campaigns

The example below demonstrates how to delete a Campaign Resource in the ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Campaigns object
    campaigns = tc.campaigns()

    owner = 'Example Community'

    # create an empty Campaign
    campaign = campaigns.add('', owner)
    # set the ID of the new Campaign to the ID of the Campaign you would like to delete
    campaign.set_id(123456)

    try:
        # delete the Campaign - no API calls are made until this line
        campaign.delete()
    except RuntimeError as e:
        print(e)
        sys.exit(1)
```
