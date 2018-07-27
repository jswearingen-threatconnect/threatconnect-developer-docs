# Threats

The Threat Group represents a group of related activity, whether or not attribution is known. This relation can be based on technology (e.g., Shellshock) or pertain to a grouping of activity that is presumed to be by the same selection of actors (e.g., Bitterbug).

## Retrieve Threats

### Retrieving a Single Threat

This example demonstrates how to retrieve a specific Threat using the Threat's ID. The `add_id` filter specifies the ID of the Threat which you would like to retrieve.

```eval_rst
.. code-block:: python
    :emphasize-lines: 10-12,15-16

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Threats object
    threats = tc.threats()

    # set a filter to retrieve only the Threat with ID: 123456
    filter1 = threats.add_filter()
    filter1.add_id(123456)

    try:
        # retrieve the Threat
        threats.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the retrieved Threats (in this case there should only be one) and print its properties
    for threat in threats:
        print(threat.id)
        print(threat.name)
        print(threat.date_added)
        print(threat.weblink)
        print('')
```

### Retrieving Multiple Threats

This example will demonstrate how to retrieve Threats while applying
filters. In this example, two filters will be added, one for the Owner
and another for a Tag. The result set returned from this example will
contain any Threats in the **Example Community** Owner that has the `Nation State` Tag. 

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Threats object
    threats = tc.threats()

    owner = 'Example Community'

    # set a filter to only retrieve Threats in the 'Example Community' tagged: 'Nation State'
    filter1 = threats.add_filter()
    filter1.add_owner(owner)
    filter1.add_tag('Nation State')

    try:
        # retrieve the Threats
        threats.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the retrieved Threats and print their properties
    for threat in threats:
        print(threat.id)
        print(threat.name)
        print(threat.date_added)
        print(threat.weblink)
        print('')
```

```eval_rst
.. note:: The ``filter1`` object contains a ``filters`` property that provides a list of supported filters for the resource type being retrieved. To display this list, ``print(filter1.filters)`` can be used. For more on using filters see the `Advanced Filter Tutorial <https://docs.threatconnect.com/en/latest/python/advanced.html#advanced-filtering>`__.
```

## Create Threats

The example below demonstrates how to create a Threat Resource in the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-13,23-24

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Threat object
    threats = tc.threats()

    owner = 'Example Community'

    # create a new Threat in 'Example Community' with the name: 'New Threat'
    threat = threats.add('New Threat', owner)

    # add a description attribute
    threat.add_attribute('Description', 'Description Example')
    # add a tag
    threat.add_tag('Example')
    # add a security label
    threat.set_security_label('TLP Green')

    try:
        # create the Threat - no API calls are made until this line
        threat.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

## Update Threats

The example below demonstrates how to update a Threat Resource in the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,20-21

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Threats object
    threats = tc.threats()

    owner = 'Example Community'

    # create a Threat with an updated name
    threat = threats.add('Updated Threat', owner)
    # set the ID of the new Threat to the ID of the existing Threat you want to update
    threat.set_id(123456)

    # you can update the Threat metadata as described here: https://docs.threatconnect.com/en/latest/python/groups/groups.html#group-metadata

    try:
        # update the Threat - no API calls are made until this line
        threat.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

### Delete Threats

The example below demonstrates how to delete an Threat Resource in the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Threats object
    threats = tc.threats()

    owner = 'Example Community'

    # create an empty Threat
    threat = threats.add('', owner)
    # set the ID of the new Threat to the ID of the Threat you would like to delete
    threat.set_id(123456)

    try:
        # delete the Threat - no API calls are made until this line
        threat.delete()
    except RuntimeError as e:
        print(e)
        sys.exit(1)
```
