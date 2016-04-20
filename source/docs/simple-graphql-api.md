---
title: Simple GraphQL API
order: 110
description: 
---

graph.cool supports two different schemas: Simple GraphQL API and Relay API. If you are not using relay you should probably use the Simple GraphQL API.

To experiment with the api you can go to the playground in the graph.cool dashboard as described in [getting started](getting-started.html#Overview).

## Queries

### List all by type

To get all nodes of a specific type you can use one of the connections on the root query type. For example to list all nodes of the build in User type you could do the following:

    { allUsers{ name } }

### Get specific node

If you know the id of the User you want you can query for that specific node:

    { User(is: "the id"){ name } }

### Sub nodes

The above query returned a User node and we specified in the query that we were interested in just the name from that node. We can use the same notation to retrieve fields from nodes in a connection from the current node. For example we could use this query to get a list of all of a users friends:

    { User(is: "the id"){ name, friends{ name } } }

### Filter

In the above examples allUsers and friends are connections containing many nodes. When querying a connection you can specify one or more filters to apply. For example you could do the following to retrieve all users from Denmark:

    { allUsers(filter: { country: "Denmark" }){ name } }

### Sort

You can also sort the nodes in a connection by a specific field:

    { allUsers(orderBy: age_DESC){ name } }

Use field\_DESC to sort in descending order and field\_ASC to sort in ascending order.

Integer and Float fields are sorted in numeric order and String fields are sorted in lexicographical order.

### Skip and Take

If you have many nodes it is best to retrieve only a small set at a time. To get the first 10 users you can make this query:

    { allUsers(take: 10){ name } }
    
And to get the next 10:

    { allUsers(skip: 10, take: 10){ name } }

## Mutations

### Create

### Update

### Delete

### Connections

 - addToConnection
 - removeFromConnection