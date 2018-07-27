# Group Associations

## Retrieve Group Associations

The code snippet below demonstrates how to view Groups, Indicators, and Victims which are associated with a given Group in ThreatConnect. This example is designed to retrieve the associations from an Incident with an ID of `123456`. To test this code snippet, change the `incident_id` variable to the ID of an incident in your owner. This same process also applies to all group types. Simply change `tc.incidents()` to the group type you would like to retrieve. The available group types are: `tc.<adversaries|campaigns|documents|emails|incidents|signatures|threats>()`.

```eval_rst
.. code-block:: python
    :emphasize-lines: 28-29,38-39,52-53

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # define the ID of the group we would like to retrieve
    incident_id = 123456

    # create an incidents object
    incidents = tc.incidents()

    # set a filter to retrieve the incident with the id: 123456
    filter1 = incidents.add_filter()
    filter1.add_id(incident_id)

    try:
        # retrieve the Incidents
        incidents.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the Incidents
    for incident in incidents:
        print(incident.name)

        # iterate through all associated Groups
        for associated_group in incident.group_associations:
            # print details about the associated Group
            print(associated_group.id)
            print(associated_group.name)
            print(associated_group.resource_type)
            print(associated_group.owner_name)
            print(associated_group.date_added)
            print(associated_group.weblink)
            print('')

        # iterate through all associated Indicators
        for associated_indicator in incident.indicator_associations:
            # print details about the associated Indicator
            print(associated_indicator.id)
            print(associated_indicator.indicator)
            print(associated_indicator.type)
            print(associated_indicator.description)
            print(associated_indicator.owner_name)
            print(associated_indicator.rating)
            print(associated_indicator.confidence)
            print(associated_indicator.date_added)
            print(associated_indicator.last_modified)
            print(associated_indicator.weblink)
            print('')

        # iterate through all associated Victims
        for associated_victim in incident.victim_associations:
            # print details about the associated Victim
            print(associated_victim.id)
            print(associated_victim.name)
            print(associated_victim.description)
            print(associated_victim.owner_name)
            print(associated_victim.nationality)
            print(associated_victim.org)
            print(associated_victim.suborg)
            print(associated_victim.work_location)
            print(associated_victim.weblink)
            print('')
```

```eval_rst
.. note:: When the ``group_associations``, ``indicator_associations``, and ``victim_associations`` methods are called, an API request is invoked immediately.
```

## Create Group Associations

The code snippet below demonstrates how to create an association between an Incident and another Group, Indicator, and Victim in ThreatConnect. This example is designed to create associations with an Incident with an ID of `123456`. To test this code snippet, change the `incident_id` variable to the ID of an incident in your owner. This same process also applies to all group types. Simply change `tc.incidents()` to the group type you would like to retrieve. The available group types are: `tc.<adversaries|campaigns|documents|emails|incidents|signatures|threats>()`.

```eval_rst
.. code-block:: python
    :emphasize-lines: 1,30-31,33-34,36-37,39-40

    from threatconnect.Config.ResourceType import ResourceType

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # define the ID of the group we would like to retrieve
    incident_id = 123456

    # create an incidents object
    incidents = tc.incidents()

    # set a filter to retrieve the incident with the id: 123456
    filter1 = incidents.add_filter()
    filter1.add_id(incident_id)

    try:
        # retrieve the Incidents
        incidents.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the Incidents
    for incident in incidents:
        print(incident.name)

        # create an association between this incident and the incident with the ID: 654321
        incident.associate_group(ResourceType.INCIDENTS, 654321)

        # create an association between this incident and the URL indicator: http://example.com/
        incident.associate_indicator(ResourceType.URLS, 'http://example.com/')

        # create an association between this incident and the victim with the ID: 333333
        incident.associate_victim(333333)

        # commit the changes to ThreatConnect - no API calls are made until this line
        incident.commit()
```

## Delete Group Associations

The code snippet below demonstrates how to remove an association between an Incident and another Group, Indicator, and Victim. This example is designed to remove the associations from an Incident with an ID of `123456`. To test this code snippet, change the `incident_id` variable to the ID of an incident in your owner. This same process also applies to all group types. Simply change `tc.incidents()` to the group type you would like to retrieve. The available group types are: `tc.<adversaries|campaigns|documents|emails|incidents|signatures|threats>()`.

```eval_rst
.. code-block:: python
    :emphasize-lines: 1,30-31,33-34,36-37,39-40

    from threatconnect.Config.ResourceType import ResourceType

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # define the ID of the group we would like to retrieve
    incident_id = 123456

    # create an incidents object
    incidents = tc.incidents()

    # set a filter to retrieve the incident with the id: 123456
    filter1 = incidents.add_filter()
    filter1.add_id(incident_id)

    try:
        # retrieve the Incidents
        incidents.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the Incidents
    for incident in incidents:
        print(incident.name)

        # remove the association between this incident and the incident with the ID: 654321
        incident.disassociate_group(ResourceType.INCIDENTS, 654321)

        # remove the association between this incident and the URL indicator: http://example.com/
        incident.disassociate_indicator(ResourceType.URLS, 'http://example.com/')

        # remove the association between this incident and the victim with the ID: 333333
        incident.disassociate_victim(333333)

        # commit the changes to ThreatConnect - no API calls are made until this line
        incident.commit()
```
