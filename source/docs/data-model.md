---
title: Data Model
order: 101
description: 
---

# Data Model

## Models

### User Model

- add fields (explain the types)

- a special system model
- create user (password, email)
- signin user (password, email, token is returned)

## Fields


## Types

In addition to the models you create for your project graph.cool has these build in types:

#### String

A string holds text. This is the type you would use for a users name, the content of a blog post or anything else that is best represented as text.

#### Integer

An integer is a number that can not have decimals. Use this to store values such as the weight of an ingredient required for a recipe or minimum age for an event.

#### Float

A float is a number that can have decimals. Use this to store values such as the price of an item in a store and teh result of complex calculations.

#### Boolean

A boolean can have the value True or Flase. This is useful to keep track of settings such as whether the user want to receive an email newsletter or if a recipe is appropriate for vegetarians.

#### Enum

Like a boolean an enum can have one of a predefined set of values. The difference is that you get to decide the possible values. For example you could specify how an article should be formatted by creating an enum with the possible values COMPACT, WIDE and COVER.

#### ID

All nodes in graph.cool are automatically assigned and id field of type ID. You use this value when quering specific nodes or adding nodes to connections.

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

```
User > friends
```

Meaning that only if the current user is in the friends connection on the user being queried will the action be allowed.

If only the author of a blog post should be able to delete it you would create a permission with the Delete action and the Related type and specify the following path:

```
Post > author
```

> note: It is currently not possible to specify a path longer than 1 step. This limitation will be removed in the near future

## Connections
- synonym: relations

If two things are related you can create a connection between them. If you are used to work with SQL databases you can think of a connection as a foreign key. Even if you are not used to work with databases I think you will find it straight forward to use connections in graph.cool.

Let's look at an example. If you are creating a blog you could have two models: User and Post. To keep track of who wrote what post you you could have a author connection from Post to User:

```
Post > author > User
```

Connections always goes in bot directions, so you would also have a connection from User to Post:

```
User > posts > Post
```

As you can imagine the `author` connection will always contain the one User who wrote the Post. The `posts` connection on the other hand will contain all the Posts made by the User.

Now, whenever you create a new post you will have to specify what User should be the author for that Post.

connections are extremely useful when making [queries](simple-graphql-api.html#Queries). This is how you would get all Posts by a specific user:

```
{User(id: "user1"){name, posts{title, text}}}
```

returns:

```
{ User: { 
    name: "Johannes Schickling",
    posts: [
      {title: "Getting Started with GraphQL", text: "..."},
      {title: "Using Relay with React Native", text: "..."}
    ]
  }
}
```

Imagine you want to make it possible to like a Post. You can accomplish this very easily by creating a likedBy connection on Post:

```
Post > likedBy > User
```

Now you can extend your query to include the names of Users who liked a Post:

```
{User(id: "user1"){name, posts{title, text, likedBy{name}}}}
```
