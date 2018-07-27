# Documents

The Document Group represents an actual file of interest, such as a PDF report that contains valuable intelligence or a malware sample.

## Retrieve Documents

### Retrieving a Single Document

This example demonstrates how to retrieve a specific Document using the Document's ID. The `add_id` filter specifies the ID of the Document which you would like to retrieve.

```eval_rst
.. code-block:: python
    :emphasize-lines: 10-12,15-16

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Documents object
    documents = tc.documents()

    # set a filter to retrieve only the Document with ID: 123456
    filter1 = documents.add_filter()
    filter1.add_id(123456)

    try:
        # retrieve the Document
        documents.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))

    # iterate through the retrieved Documents (in this case there should only be one) and print its properties
    for document in documents:
        print(document.id)
        print(document.name)
        print(document.date_added)
        print(document.owner_name)
        print(document.weblink)

        # Document specific property
        print(document.file_name)

        print('')
```

### Downloading a Document's Contents

Python SDK example of downloading the contents of the document stored
with the Document Resource:

```eval_rst
.. 
    no-test

.. code-block:: python

    document.download()
    if document.contents is not None:
        print(document.contents)
```

### Retrieving Multiple Documents

This example will demonstrate how to retrieve documents while applying
filters. In this example, two filters will be added, one for the Owner
and another for a Tag. The result set returned from this example will
contain any documents in the **Example Community** Owner that has a 
`Nation State` Tag.

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Documents object
    documents = tc.documents()

    owner = 'Example Community'

    # set a filter to only retrieve Documents in the 'Example Community' tagged: 'Nation State'
    filter1 = documents.add_filter()
    filter1.add_owner(owner)
    filter1.add_tag('Nation State')

    try:
        # retrieve the Documents
        documents.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))

    # iterate through the retrieved Documents and print their properties
    for document in documents:
        print(document.id)
        print(document.name)
        print(document.date_added)
        print(document.owner_name)
        print(document.weblink)
        print('')
```

```eval_rst
.. note:: The ``filter1`` object contains a ``filters`` property that provides a list of supported filters for the resource type being retrieved. To display this list, ``print(filter1.filters)`` can be used. For more on using filters see the `Advanced Filter Tutorial <https://docs.threatconnect.com/en/latest/python/advanced.html#advanced-filtering>`__.
```

## Create Documents

The example below demonstrates how to create a Document Resource in the ThreatConnect platform.

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-14,16-20,30-31

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Documents object 
    documents = tc.documents()

    owner = 'Example Community'

    # create a new Document in 'Example Community' with the name: 'New Document'
    document = documents.add('New Document', owner)
    document.set_file_name('New File.txt')

    # open a file handle for a local file and read the contents thereof
    fh = open('./sample1.zip', 'rb')
    contents = fh.read()
    # upload the contents of the file into the Document
    document.upload(contents)

    # add a description attribute
    document.add_attribute('Description', 'Description Example')
    # add a tag
    document.add_tag('Example')
    # add a security label
    document.set_security_label('TLP Green')

    try:
        # create the Document - no API calls are made until this line
        document.commit()
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
| malware       | set\_malware    | False    |
+---------------+-----------------+----------+
| password      | set\_password   | False    |
+---------------+-----------------+----------+

### Create Malware Document

To [create a malware document](http://kb.threatconnect.com/customer/en/portal/articles/2171402-uploading-malware) in ThreatConnect, make use of the `set_malware` and `set_password` functions as demonstrated below:

```eval_rst
.. code-block:: python
    :emphasize-lines: 22-24

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Documents object 
    documents = tc.documents()

    owner = 'Example Community'

    # create a new Document in 'Example Community' with the name: 'New Document'
    document = documents.add('Malicious File', owner)
    document.set_file_name('bad.exe')

    # open a file handle for a local file and read the contents thereof
    fh = open('./bad.exe.zip', 'rb')
    contents = fh.read()
    # upload the contents of the file into the Document
    document.upload(contents)

    document.set_malware(True)
    # set the archive password for the zip (the default is "TCinfected")
    document.set_password("TCinfected")

    try:
        # create the Document - no API calls are made until this line
        document.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

## Update Documents

The example below demonstrates how to update a Document Resource in the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,20-21

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Documents object
    documents = tc.documents()

    owner = 'Example Community'

    # create a Document with an updated name
    document = documents.add('Updated Document', owner)
    # set the ID of the new Document to the ID of the existing Document you want to update
    document.set_id(123456)

    # you can update the Document metadata as described here: https://docs.threatconnect.com/en/latest/python/groups/groups.html#group-metadata

    try:
        # update the Document - no API calls are made until this line
        document.commit()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)
```

## Delete Documents

The example below demonstrates how to delete a Document Resource from the
ThreatConnect platform:

```eval_rst
.. code-block:: python
    :emphasize-lines: 12-15,18-19

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # instantiate Documents object
    documents = tc.documents()

    owner = 'Example Community'

    # create an empty Document
    document = documents.add('', owner)
    # set the ID of the new Document to the ID of the Document you would like to delete
    document.set_id(123456)

    try:
        # delete the Document - no API calls are made until this line
        document.delete()
    except RuntimeError as e:
        print(e)
        sys.exit(1)
```
