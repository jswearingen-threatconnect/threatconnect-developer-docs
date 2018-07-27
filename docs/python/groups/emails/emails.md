# Emails

The E-mail Group represents an occurrence of a specific suspicious email, such as a phishing attempt.

## Retrieve Emails

### Retrieving a Single Email

This example demonstrates how to retrieve a specific Email using the Email's ID. The `add_id` filter specifies the ID of the Email which you would like to retrieve.

```eval_rst
.. code-block:: python
    :emphasize-lines: 10-12,15-16

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Emails object
    emails = tc.emails()

    # set a filter to retrieve only the Email with ID: 123456
    filter1 = emails.add_filter()
    filter1.add_id(123456)

    try:
        # retrieve the Email
        emails.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))

    # iterate through the retrieved Emails (in this case there should only be one) and print its properties
    for email in emails:
        print(email.id)
        print(email.name)
        print(email.date_added)
        print(email.weblink)

        # Email specific properties
        print(email.header)
        print(email.subject)
        print(email.from_address)
        print(email.to)
        print(email.body)
        print(email.score)

        print('')
```

### Retrieving Multiple Emails

This example will demonstrate how to retrieve emails while applying
filters. In this example, two filters will be added, one for the Owner
and another for a Tag. The result set returned from this example will
contain any emails in the **Example Community** Owner that has a `Nation State` Tag.

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Emails object
    emails = tc.emails()

    owner = 'Example Community'

    # set a filter to only retrieve Emails in the 'Example Community' tagged: 'Nation State'
    filter1 = emails.add_filter()
    filter1.add_owner(owner)
    filter1.add_tag('Nation State')

    try:
        # retrieve the Emails
        emails.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))

    # iterate through the retrieved Emails and print their properties
    for email in emails:
        print(email.id)
        print(email.name)
        print(email.date_added)
        print(email.weblink)

        # Email specific property
        print(email.score)

        print('')
```

```eval_rst
.. note:: The ``filter1`` object contains a ``filters`` property that provides a list of supported filters for the resource type being retrieved. To display this list, ``print(filter1.filters)`` can be used. For more on using filters see the `Advanced Filter Tutorial <https://docs.threatconnect.com/en/latest/python/advanced.html#advanced-filtering>`__.
```

## Create Emails

The example below demonstrates how to create an Email Resource in the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-13,15-20,30-31

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Emails object
    emails = tc.emails()

    owner = 'Example Community'

    # create a new Email in 'Example Community' with the name: 'New Email'
    email = emails.add('New Email', owner)

    # set Email specific properties
    email.set_body('This is an email body.')  # REQUIRED
    email.set_from_address('bad_guy@example.com')  # OPTIONAL
    email.set_header('This is an improper email header.')  # REQUIRED
    email.set_subject('This is an email subject.')  # REQUIRED
    email.set_to('victim@example.com')  # OPTIONAL

    # add a description attribute
    email.add_attribute('Description', 'Description Example')
    # add a tag
    email.add_tag('EXAMPLE')
    # add a security label
    email.set_security_label('TLP Green')

    try:
        # create the Email - no API calls are made until this line
        email.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

**Supported Properties**

+---------------+--------------------+----------+
| Property Name | Method             | Required |
+===============+====================+==========+
| body          | set\_body          | True     |
+---------------+--------------------+----------+
| header        | set\_header        | True     |
+---------------+--------------------+----------+
| subject       | set\_subject       | True     |
+---------------+--------------------+----------+
| from\_address | set\_from\_address | False    |
+---------------+--------------------+----------+
| score         | set\_score         | False    |
+---------------+--------------------+----------+
| to            | set\_to            | False    |
+---------------+--------------------+----------+

## Update Emails

The example below demonstrates how to update an Email Resource in the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,20-21

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Emails object
    emails = tc.emails()

    owner = 'Example Community'

    # create an Email with an updated name
    email = emails.add('Updated Email', owner)
    # set the ID of the new Email to the ID of the existing Email you want to update
    email.set_id(123456)

    # you can update the Email metadata as described here: https://docs.threatconnect.com/en/latest/python/groups/groups.html#group-metadata

    try:
        # update the Email - no API calls are made until this line
        email.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

## Delete Emails

The example below demonstrates how to delete an Email Resource in the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Emails object
    emails = tc.emails()

    owner = 'Example Community'

    # create an empty Email
    email = emails.add('', owner)
    # set the ID of the new Email to the ID of the Email you would like to delete
    email.set_id(123456)

    try:
        # delete the Email - no API calls are made until this line
        email.delete()
    except RuntimeError as e:
        print(e)
        sys.exit(1)
```
