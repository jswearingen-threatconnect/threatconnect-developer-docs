# Adversaries

The Adversary Group represents a malicious actor or group of actors.

## Retrieve Adversaries

### Retrieving a Single Adversary

This example demonstrates how to retrieve a specific Adversary using the Adversary's ID. The `add_id` filter specifies the ID of the Adversary which you would like to retrieve.

```eval_rst
.. code-block:: python
    :emphasize-lines: 10-12,15-16

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Adversaries object
    adversaries = tc.adversaries()

    # set a filter to retrieve only the Adversary with ID: 123456
    filter1 = adversaries.add_filter()
    filter1.add_id(123456)

    try:
        # retrieve the Adversary
        adversaries.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the retrieved Adversary (in this case there should only be one) and print its properties
    for adversary in adversaries:
        print(adversary.id)
        print(adversary.name)
        print(adversary.date_added)
        print(adversary.weblink)
        print('')
```

### Retrieving Multiple Adversaries

This example demonstrates how to retrieve Adversaries while applying filters. Two filters are added: one for the Owner and another for a Tag. The result set returned from this example will contain all Adversaries in the "Example Community" Owner that have the `Nation State` Tag.

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Adversaries object
    adversaries = tc.adversaries()

    owner = 'Example Community'

    # set a filter to only retrieve Adversaries in the 'Example Community' tagged: 'Nation State'
    filter1 = adversaries.add_filter()
    filter1.add_owner(owner)
    filter1.add_tag('Nation State')

    try:
        # retrieve the Adversaries
        adversaries.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the retrieved Adversaries and print their properties
    for adversary in adversaries:
        print(adversary.id)
        print(adversary.name)
        print(adversary.date_added)
        print(adversary.weblink)
        print('')
```

```eval_rst
.. note:: The ``filter1`` object contains a ``filters`` property that provides a list of supported filters for the resource type being retrieved. To display this list, ``print(filter1.filters)`` can be used. For more on using filters see the `Advanced Filter Tutorial <https://docs.threatconnect.com/en/latest/python/advanced.html#advanced-filtering>`__.
```

## Create Adversaries

Example Python SDK creating an adversary resource in the ThreatConnect
platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-13,23-24

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Adversaries object
    adversaries = tc.adversaries()

    owner = 'Example Community'

    # create a new Adversary in 'Example Community' with the name: 'New Adversary'
    adversary = adversaries.add('New Adversary', owner)

    # add a description attribute
    adversary.add_attribute('Description', 'Description Example')
    # add a tag
    adversary.add_tag('Example')
    # add a security label
    adversary.set_security_label('TLP Green')

    try:
        # create the Adversary - no API calls are made until this line
        adversary.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

## Update Adversaries

The example below demonstrates how to update an Adversary Resource in
the ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,20-21

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Adversaries object
    adversaries = tc.adversaries()

    owner = 'Example Community'

    # create an Adversary with an updated name
    adversary = adversaries.add('Updated Adversary', owner)
    # set the ID of the new Adversary to the ID of the existing Adversary you want to update
    adversary.set_id(123456)

    # you can update the Adversary metadata as described here: https://docs.threatconnect.com/en/latest/python/groups/groups.html#group-metadata

    try:
        # update the Adversary - no API calls are made until this line
        adversary.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

## Delete Adversaries

The example below demonstrates how to delete an Adversary Resource in
the ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Adversaries object
    adversaries = tc.adversaries()

    owner = 'Example Community'

    # create an empty Adversary
    adversary = adversaries.add('', owner)
    # set the ID of the new Adversary to the ID of the Adversary you would like to delete
    adversary.set_id(123456)

    try:
        # delete the Adversary - no API calls are made until this line
        adversary.delete()
    except RuntimeError as e:
        print(e)
        sys.exit(1)
```
