Índice de páginas relacionadas ao Backend:
## 1. Introduction

In this guide, we'll be explaining our backend structure, focusing on how the project is structured, what are the main funcionalities of our base project, and how to develop a new feature from scratch.

To fully understand this guide, you'll need to know the basics of Javascript, Typescript and the ES6 sintax. But don't worry, I got you covered, if don't know any of this subject you can read/watch some guides that can help you out:

- [Javascript with ES6](https://www.udemy.com/essentials-in-javascript-es6/)
- [Main ES6 features](https://webapplog.com/es6/)
- [Learn Typescript](https://tutorialzine.com/2016/07/learn-typescript-in-30-minutes)

## 2. A bottom-up approach

In Outsmart we use a serverless arquitecture, deploying all our services to AWS, which means that we don't have a dedicated server to host our backend, or even a dedicated virtual machine. All our functions are deployed in Lambda, an AWS feature that allows you to deploy code and add an event to trigger him.

To facilitate this process we use the [serverless framework](https://serverless.com/framework/docs/providers/aws/guide/intro/), this framework help us to automate our deploy, since we define a file that contains all our resouces, functions and events, and the framework take care of deploying them correctly into our AWS account.

We'll talk more about resources, events and a lot more about functions in this guide, but if you want to learn more about the serverless paradigm, the advantages and disadvantages, i recommend reading this articles:

- [What are Cloud Computing Services [IaaS, CaaS, PaaS, FaaS, SaaS]](https://medium.com/@nnilesh7756/what-are-cloud-computing-services-iaas-caas-paas-faas-saas-ac0f6022d36e)
- [What Is Function-as-a-Service? Serverless Architectures Are Here!](https://stackify.com/function-as-a-service-serverless-architecture/)
- [Serverless Architectures](https://martinfowler.com/articles/serverless.html)

## 3. A bit of data! How to properly use DynamoDB

Since we went full AWS (all our backend is structured using the AWS infrastructure), one of the main challenges was migrate to use DynamoDB, we were used to noSQL databases (we mainly used MongoDB before migrating), but don't fool yourself, even thought DynamoDB is a noSQL database it has a lot of differences from MongoDB.

DynamoDB has some optimizations that heavely favor the use of index in your tables, but to be cost-efficient you need to plan very well which columns of your tables you'll indexing. If you have some experience with different databases you might be confused now, tables, columns, indexes, this are common terms for SQL based databases.

Dynamo has a lot of SQL-like features, that are used to help you improve your database efficiency. For example, all tables must have a partition key, this key must be unique. In several use cases we need to create tables that don't have a primary unique key, dynamo let us define tables using the concept of sort keys.

### 3.1 Partition and Sort keys

As the name says the sort key is used to sort your data, and also used to create unique keys. When you create a table using this sort feature, your items don't need to have a unique key, but the pair (partition key + sort key) must be unique.

This is your most powerfull ally when developing backend features, since you get a more complex index, which contains two different information for your item, also allowing you to do indexed queries that are able to search in a sub-set of information, filtering and sorting then properly.

For example, imagine that you have a table that will save posts for some user, one approach you can take is creating a table that will have the userId as partition key and the creationDate(as a epoch timestamp) as sort key, this way you can easily query to get all the post for a specific user in a determined date range.

### 3.2 Global Secondary Indexes(GSI)

Indexing two columns of your database might not be enought, in some cases you have a lot information in a single table, and you need to search in it for different criterias, the GSI let you index different column (you can use the single column indexing or use the partion+sort index, which works exactly like the main table index).

Be carefull! Creating an index basically means that you are indexing the whole table again, so in terms of cost you are basically creating another table! Using indexes are usefull to help you solve some weird queries you need to do, but they are not the perfect solution, sometimes you solve your problem by creating another table containing a subset of information from this other table.

### 3.3 The query operation

As mention before the [Query operation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_Query.html) is one of the most important operations of DynamoDB, since it let you search for a set of information that match some specific conditions. The query operation is really time efficient, it can be used in tables that contain partition+sort and in GSIs and it's the most common method for searching data in some table.

### 3.4 The get operation

The [GetItem operation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html) can only be use in the primary index of a table, since this index contains unique keys if the operation finds a result it will only return one object.

### 3.5 The update and put operation

Both operations work similar, they are used to write data in Dynamo, [PutItem operation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutItem.html) can be used both to create an item or overwrite and existing item. In the otherhand the [UpdateItem operation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html) can only be used on an existing item, and it will only replace the provided properties of the item, i.e. the PutItem will completely replace your item, and the update will only change existing fields and add new ones.

### 3.6 The scan operation

Avoid this! The [Scan operation](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_Scan.html) is 

### 3.7 Batch operations

The Get and Put operations have a "Batch" version, in which you can do several operations of Get or Put in parallel, it's a more optimized way of doing this operations, since Dynamo itself manage the writing/reading of data. Always prefer to use this instead of iterating promises or using a Promise.all sintax.

### 3.8 Pagination and the size of operations

All Dynamo operations have a limitation in the amount of data that can be fetch, in the normal operations you have a bite size limit, and the batch operations have a number based limit (i.e. 25 items per batch request). 

Because of this you have to consider for pagination problems in all your operations, Dynamo has a built-in pagination feature, also the library we use to help us use Dynamo abstract this for us. So if you don't explictily say that you want to paginate a query, it will iterate over a table until it fetch all the results, depending on the size of your table this might get really slow, so be carefull!

## 4. Authorization workflow using Cognito

Cognito is another AWS service that we use to that care of our applications authorization and authentication. There are some ways to use this feature, and we have used all of them.

Basically our signup process has two parts, first we register a user in Cognito, them we register him in our database. The signin need only to be done in Cognito, after that we use a token generated by cognito to authorize our endpoints.

At first we were doing the cognito signin and signup in our frontend, but we are completing migrating this to our backend. So how it works? We have an endpoint, which don't need authorization, that will do the user's signup, this endpoint will sign the user in cognito and in the database. Also it will return a token, which will be used in the subsequent requests.

This token is valid for an hour, and when it expires, any of our backend endpoints will automatically refresh it and return on the endpoint response. That way you will have a valid token for a month, after that you won't be able to refresh it and you'll need to signin again, which will also return a valid token.

## 5. serverless.yml and what you can do with it

## 6. Clean architecture and how our projects are structured

## 7. os-bob, all the constructions you'll need in one place

## 8. Creating your first endpoint from scratch