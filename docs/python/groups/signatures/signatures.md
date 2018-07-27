# Signatures

The Signature Group represents an actual Signature that can be used for detection or prevention in a supported format (Snort®, YARA, CybOX™, OpenIOC, ClamAV®, Suricata, Bro, and Regex).

## Retrieve Signatures

### Retrieving a Single Signature

This example demonstrates how to retrieve a specific Signature using the Signature's ID. The `add_id` filter specifies the ID of the Signature which you would like to retrieve.

```eval_rst
.. code-block:: python
    :emphasize-lines: 10-12,15-16

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Signatures object
    signatures = tc.signatures()

    # set a filter to retrieve only the Signature with ID: 123456
    filter1 = signatures.add_filter()
    filter1.add_id(123456)

    try:
        # retrieve the Signature
        signatures.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the retrieved Signatures (in this case there should only be one) and print its properties
    for signature in signatures:
        print(signature.id)
        print(signature.name)
        print(signature.date_added)
        print(signature.weblink)

        # Signature specific property
        print(signature.type)

        print('')
```

### Downloading a Signature's Content

Example Python code for downloading the Signature contents for the Signature Resource:

```eval_rst
.. 
    no-test

.. code-block:: python

    signature.download()

    if signature.contents is not None:
        print(signature.contents)
```

### Retrieving Multiple Signatures

This example will demonstrate how to retrieve Signatures while applying
filters. In this example, two filters will be added, one for the Owner
and another for a Tag. The result set returned from this example will
contain any Signatures in the **Example Community** Owner that has the `Nation State` Tag.

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Signatures object
    signatures = tc.signatures()

    owner = 'Example Community'

    # set a filter to only retrieve Signatures in the 'Example Community' tagged: 'Nation State'
    filter1 = signatures.add_filter()
    filter1.add_owner(owner)
    filter1.add_tag('Nation State')

    try:
        # retrieve the Signatures
        signatures.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the retrieved Signatures and print their properties
    for signature in signatures:
        print(signature.id)
        print(signature.name)
        print(signature.date_added)
        print(signature.weblink)
        print('')
```

```eval_rst
.. note:: The ``filter1`` object contains a ``filters`` property that provides a list of supported filters for the resource type being retrieved. To display this list, ``print(filter1.filters)`` can be used. For more on using filters see the `Advanced Filter Tutorial <https://docs.threatconnect.com/en/latest/python/advanced.html#advanced-filtering>`__.
```

## Create Signatures

The example below demonstrates how to create a Signature Resource in the
ThreatConnect platform.

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-13,15-23,25-30,40-41

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Signatures object
    signatures = tc.signatures()
        
    owner = 'Example Community'

    # create a new Signature in 'Example Community' with the name: 'New Signature'
    signature = signatures.add('New Signature', owner)

    # define Signature's content
    file_text = 'rule example_sig : example\n{\n'
    file_text += 'meta:\n        description = "This '
    file_text += 'is just an example"\n\n '
    file_text += 'strings:\n        $a = {6A 40 68 00 '
    file_text += '30 00 00 6A 14 8D 91}\n        $b = '
    file_text += '{8D 4D B0 2B C1 83 C0 27 99 6A 4E '
    file_text += '59 F7 F9}\n    condition:\n '
    file_text += '$a or $b\n}'

    # set the file name of the Signature
    signature.set_file_name('bad_file.txt')  # REQUIRED
    # set the type of the Signature
    signature.set_file_type('YARA')  # REQUIRED
    # set the contents of the signature
    signature.set_file_text(file_text)  # REQUIRED

    # add a description attribute
    signature.add_attribute('Description', 'Description Example')
    # add a tag
    signature.add_tag('Example')
    # add a security label
    signature.set_security_label('TLP Green')

    try:
        # create the Signature - no API calls are made until this line
        signature.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

**Supported Properties**

+---------------+-----------------+----------+
| Property Name | Method          | Required |
+===============+=================+==========+
| file\_name    | set\_file\_name | True     |
+---------------+-----------------+----------+
| file\_text    | set\_file\_text | True     |
+---------------+-----------------+----------+
| file\_type    | set\_file\_type | True     |
+---------------+-----------------+----------+

## Update Signatures

The example below demonstrates how to update a Signature Resource in the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,20-21

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Signatures object
    signatures = tc.signatures()

    owner = 'Example Community'

    # create a Signature with an updated name
    signature = signatures.add('Updated Signature', owner)

    # even if you are not updating these values, you need to set them
    signature.set_file_name('updated.sig')
    signature.set_file_type('Yara')

    # set the ID of the new Signature to the ID of the existing Signature you want to update
    signature.set_id(123456)

    # you can update the Signature metadata as described here: https://docs.threatconnect.com/en/latest/python/groups/groups.html#group-metadata

    try:
        # update the Signature - no API calls are made until this line
        signature.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

## Delete Signatures

The example below demonstrates how to delete a Signature Resource in the
ThreatConnect platform.

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Signatures object
    signatures = tc.signatures()

    owner = 'Example Community'

    # create an empty Signature
    signature = signatures.add('', owner)
    # set the ID of the new Signature to the ID of the Signature you would like to delete
    signature.set_id(123456)

    try:
        # delete the Signature - no API calls are made until this line
        signature.delete()
    except RuntimeError as e:
        print(e)
        sys.exit(1)
```
