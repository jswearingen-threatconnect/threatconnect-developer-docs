# Incidents

The Incident Group represents a snapshot of a particular intrusion, breach, or other event of interest.

## Retrieve Incidents

### Retrieving a Single Incident

This example demonstrates how to retrieve a specific Incident using the Incident's ID. The `add_id` filter specifies the ID of the Incident which you would like to retrieve.

```eval_rst
.. code-block:: python
    :emphasize-lines: 10-12,15-16

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Incidents object
    incidents = tc.incidents()

    # set a filter to retrieve only the Incident with ID: 123456
    filter1 = incidents.add_filter()
    filter1.add_id(123456)

    try:
        # retrieve the Incident
        incidents.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the retrieved Incidents (in this case there should only be one) and print its properties
    for incident in incidents:
        print(incident.id)
        print(incident.name)
        print(incident.date_added)
        print(incident.weblink)

        # Incident specific property
        print(incident.event_date)

        print('')
```

### Retrieving Multiple Incidents

This example will demonstrate how to retrieve Incidents while applying
filters. In this example, two filters will be added, one for the Owner
and another for a Tag. The result set returned from this example will
contain any Incidents in the **Example Community** Owner that has a `Nation State` Tag.

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Incidents object
    incidents = tc.incidents()

    owner = 'Example Community'

    # set a filter to only retrieve Incidents in the 'Example Community' tagged: 'Nation State'
    filter1 = incidents.add_filter()
    filter1.add_owner(owner)
    filter1.add_tag('Nation State')

    try:
        # retrieve the Incidents
        incidents.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))

    # iterate through the retrieved Incidents and print their properties
    for incident in incidents:
        print(incident.id)
        print(incident.name)
        print(incident.date_added)
        print(incident.weblink)

        # Incident specific property
        print(incident.event_date)

        print('')
```

```eval_rst
.. note:: The ``filter1`` object contains a ``filters`` property that provides a list of supported filters for the resource type being retrieved. To display this list, ``print(filter1.filters)`` can be used. For more on using filters see the `Advanced Filter Tutorial <https://docs.threatconnect.com/en/latest/python/advanced.html#advanced-filtering>`__.
```

## Create Incidents

The example below demonstrates how to create an Incident Resource in the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,25-26

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Incidents object
    incidents = tc.incidents()

    owner = 'Example Community'

    # create a new Incident in 'Example Community' with the name: 'New Incident'
    incident = incidents.add('New Incident', owner)
    # set the event date for the Incident
    incident.set_event_date('2017-03-21T00:00:00Z')  # REQUIRED

    # add a description attribute
    incident.add_attribute('Description', 'Description Example')
    # add a tag
    incident.add_tag('Example')
    # add a security label
    incident.set_security_label('TLP Green')

    try:
        # create the Incident - no API calls are made until this line
        incident.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

**Supported Properties**

+-----------------+--------------------+------------+
| Property Name   | Method             | Required   |
+=================+====================+============+
| event\_date     | set\_event\_date   | True       |
+-----------------+--------------------+------------+

## Update Incidents

The example below demonstrates how to update an Incident Resource in the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,20-21

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Incidents object
    incidents = tc.incidents()

    owner = 'Example Community'

    # create an Incident with an updated name
    incident = incidents.add('Updated Incident', owner)
    # set the ID of the new Incident to the ID of the existing Incident you want to update
    incident.set_id(123456)

    # you can update the Incident metadata as described here: https://docs.threatconnect.com/en/latest/python/groups/groups.html#group-metadata

    try:
        # update the Incident - no API calls are made until this line
        incident.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

## Delete Incidents

The example below demonstrates how to delete an Incident Resource in the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Incidents object
    incidents = tc.incidents()

    owner = 'Example Community'

    # create an empty Incident
    incident = incidents.add('', owner)
    # set the ID of the new Incident to the ID of the Incident you would like to delete
    incident.set_id(123456)

    try:
        # delete the Incident - no API calls are made until this line
        incident.delete()
    except RuntimeError as e:
        print(e)
        sys.exit(1)
```
