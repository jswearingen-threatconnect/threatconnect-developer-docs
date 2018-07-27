# Associations

## Group Associations

* [Retrieve Group Associations](../groups/groups.html#retrieve-group-associations)
* [Create Group Associations](../groups/groups.html#create-group-associations)
* [Delete Group Associations](../groups/groups.html#delete-disassociate-group-associations)

## Indicator Associations

* [Retrieve Indicator Associations](../indicators/indicators.html#retrieve-indicator-associations)
* [Create Indicator Associations](../indicators/indicators.html#create-indicator-associations)
* [Delete Indicator Associations](../indicators/indicators.html#delete-disassociate-indicator-associations)

## Tag Associations

* [Retrieve Tag Associations](../tags/tags.html#retrieve-tag-associations)

## Task Associations

* [Retrieve Task Associations](../tasks/tasks.html#retrieve-task-associations)
* [Create Task Associations](../tasks/tasks.html#create-task-associations)
* [Delete Task Associations](../tasks/tasks.html#delete-disassociate-task-associations)

## Victim Associations

* [Retrieve Victim Associations](../victims/victims.html#retrieve-victim-associations)
* [Create Victim Associations](../victims/victims.html#create-victim-associations)
* [Delete Victim Associations](../victims/victims.html#delete-disassociate-victim-associations)

## Retrieving Available Associations

All of the available associations can be viewed by making a `GET` request to `/v2/types/associationTypes`. This will return the name of the association and, if applicable, the Indicator/Group Resources between which the association can be created.

```
GET /v2/types/associationTypes/
```

To retrieve information about a specific association, use the following GET request format:

```
GET /v2/types/associationTypes/{associationTypeName}/
```

For example, the GET request below will return details about the `Adversary` association type:

```
GET /v2/types/associationTypes/Adversary/
```

JSON Response:

```javascript
{
  "status": "Success",
  "data": {
    "associationType": {
      "name": "Adversary",
      "custom": "false",
      "fileAction": "false",
      "apiBranch": "adversaries"
    }
  }
}
```

```eval_rst
.. note:: Custom association types can be created on an instance of ThreatConnect by a system administrator. Please contact your system administrator to create new types of associations.
```