---
title: Getting Started
order: 100
description: 
---

Modern tools like react and angular make it really easy to develop high quality UI for mobile and web applications. Graph.cool makes it just as easy to configure the backend api for your application 

 - what is graph.cool (relay short)
 - sign up (quick intro to todo examples - link to tutorial / example app)
 - the playground (show the docs. Introduce auto complete explain queries and mutations very briefly)
 - create new project

## Overview

## Prerequisites


# Data Model

## Models

### User Model

- add fields (explain the types)

- a special system model
- create user (password, email)
- signin user (password, email, token is returned)

## Fields


## Types

- Built-in types

#### String

#### Integer

#### Float

#### Boolean

#### Enum

#### ID

## Permissions

> For a high level overview of the graph.cool security system see [Security](security.html)

- synonym: ACL

In graph.cool permissions are specified on individual fields. This allows you to specify that a users name should be public while her birthday should be private and her contact details only visible by friends.

### Actions

You can specify permissions for 4 different actions. Read, Create, Update, Delete.

#### Read

Returning the value of the field in a query

#### Create

Setting the value of the field when creating a new node

#### Update

Updating the value for an existing node

#### Delete

todo: what does this even mean? deleting a node vs a field value...

### Permission Types

To specify who should be allowed to perform a certain action you use a combination of three permission types. Guest, Authenticated and Related. 

#### Guest

Guest means that anyone can perform the action, even if they are not signed in to your application. If you are creating a blog you probably want to have a Read action with the Guest permission on all blog posts and comments.

#### Authentificated

Authenticated works almost like the Guest permission except users have to be signed in to your application. If you are creating an app that requires users to sign up you can use this permission type instead of Guest.

In addition you can specify that this action is only available to users with a specific role. This is useful if you have more than one type of user in your application. For example you might want users with the MODERATOR role to be able to delete other users comments while normal users can only delete their own.

> note: It is currently not possible to add user roles in the dashboard ui. Please contact us in the [graph.cool slack group](https://slack.graph.cool/) so we can help you out!

#### Related

> This permission type is based on [connections](#Connections) in your schema

If you are used to thinking about security for traditional apis it can be helpful to forget all of that for a moment. If you have never thought about api security before you are in luck. This will be easy for you :-)

The Related permission type allows you to specify that only users who are connected to the node in a certain way are allowed to perform the action. For example you could specify that only friends of a user can view their location or that only the author of a blog post can delete comments.

When creating a Related permission you specify a path to the user who should have access from the node you are adding the permission for.

If you want to limit access to a users friends the path could ook like this:

    User > friends

Meaning that only if the current user is in the friends connection on the user being queried will the action be allowed.

If only the author of a blog post should be able to delete it you would create a permission with the Delete action and the Related type and specify the following path:

    Post > author

> note: It is currently not possible to specify a path longer than 1 step. This limitation will be removed in the near future

## Connections


- synonym: relations
- connections (the general concept. That a connection is always created in both directions)
