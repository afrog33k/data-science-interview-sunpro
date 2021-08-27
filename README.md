# Data Engineering Interview

This repository holds an interview problem designed for the Data Engineering role at the Data team at Sunpro Solar LLC. Here we describe everything you're going to need to complete the exercise.

## Goals

1. Understand your knowledge about data-related technologies.

2. Evaluate how you solve problems.

_If you feel that for some reason the following task doesn't meet the above goals or if you find any problems along the way, you can let us know by emailing Ronald at radonyo@theprocompanies.com. I'll be happy to help!_

## How to use this repo

In order to use this repository, you'll need to fork it and clone it locally in your machine. Because of how [Github Templates work with Git LFS](https://docs.github.com/en/github/managing-large-files/about-git-large-file-storage), this cannot be a template repository :(.

## Basic Requirements

- Knowledge of data structures and algorithms;
- Knowledge of performance oriented data transformations;
- Query languages;
- Git.

## Tooling

- Git LFS;
- A local relational database.

## Folder structure

This project contains an three json files, each of them listing data objects related to a particular struct in JSON format.

```
├── README.md       - This README file.
├── systems.json    - System data objects.
├── leads.json      - Lead data objects.
└── sellers.json    - Seller data objects.
```


## Getting started

First, make sure you have `git lfs` installed and at least 2Gi of free storage space. Then, you can run the following to download the JSON files:

```sh
git lfs pull
```

##  Context

When someone sends us a message interested in buying a Solar system, we call it a _Lead_. On a daily basis, we receive a lot of Solar system leads, but not all of them are very promising. Each _Lead_ is related to a specific _System_ the person wants to buy and each _System_ is related to a _Seller_. 

Here are the structs that represent each of them:

```go
type Lead struct {
    /**
     * The unique identifier of this Lead.
     */
    Id uuid.UUID
    /**
     * When the Lead was first created.
     */
    CreateDate *time.Time
    /**
     * When this Lead sold a System, the SaleDate is defined.
     */
    SaleDate *time.Time
    /**
     * The city in which the Lead comes from.
     */
    City int
    /**
     * Represents how interested the person
     * is in buying a system. 0 means the person
     * is not interested, 100 means the person
     * is very interested.
     */
    MessageScore int
    /**
     * The ID of the system this Lead is about.
     */
    SystemId uuid
}


type System struct {
    /**
     * The Id of the System.
     */
    Id uuid
    /**
     * The kind of offset this System gives the homeowner. (Class)
     */
    Offset int
    /**
     * The number of panels, from 1 = 10 to 4 = 40.
     */
    Panels int
    /**
     * The package the System belongs to. (Class)
     */
    Package string
    /**
     * The price of the System.
     */
    Price float64
    /**
     * The ID of the Seller who sells this System.
     */
    SellerId uuid.UUID
}

type Seller struct {
    /**
     * The ID of the Seller.
     */
    Id uuid.UUID
    /**
     * The city of the Seller.
     */
    City int
    /**
     * How likely this Seller is to
     * sale a System to a Lead, from 0 to 1.
     */
    SaleRate float64
}
```

```

The files `systems.json`, `leads.json` and `sellers.json` contains a small snippet of our database and they only cover the month of October, 2019. Here is how the `leads.json` file looks like:

```
{"Id":"9aa7ee3d-0368-4341-8153-40731ad5ef23","CreateDate":"2019-10-20T09:01:40Z","City":7,"MessageScore":46,"SystemId":"3d6a54d2-65da-4494-9eaf-547ea5c22bdc"}
{"Id":"44f31599-f967-4760-892f-e379bc502798","CreateDate":"2019-10-21T20:23:44Z","SaleDate":"2019-12-01T20:23:44Z","City":5,"MessageScore":20,"SystemId":"3d6a54d2-65da-4494-9eaf-547ea5c22bdc"}
{"Id":"1da1d0e7-d1f2-4c1e-ad56-e8f9e2771c77","CreateDate":"2019-10-28T06:20:13Z","City":2,"MessageScore":26,"SystemId":"3d6a54d2-65da-4494-9eaf-547ea5c22bdc"}
{"Id":"6f5efb2b-7ed9-4092-9f2b-eef8c2506abe","CreateDate":"2019-10-20T01:35:12Z","City":6,"MessageScore":89,"SystemId":"0469f10f-52f4-4f14-8a21-5b0d4f696ed3"}
{"Id":"bc67a2ab-806e-489e-b899-095db2cdfe5c","CreateDate":"2019-10-10T16:40:47Z","SaleDate":"2019-12-27T16:40:47Z","City":5,"MessageScore":81,"SystemId":"0469f10f-52f4-4f14-8a21-5b0d4f696ed3"}
```

When a `Lead` has a `SaleDate` it means the Lead was already sent to the `Seller` by one of our human attendants. For the purpose of our exercise, we'll assume that every Lead without a `SaleDate` is a `Lead` that the correspondent Seller never saw.

## Problem

We want to run some queries on this data and in order to do this, we want you to **design a table that can answer those queries** for us. Also, we want a **data pipeline design to update this table every 15 minutes with new information** from the JSON files. The JSON files available here are just a sample. Plan your table and pipeline to **ingest and query terabytes of data**. The table design can be relational or non-relational; it's your pick.

The queries that we want to answer are:

- **Which seller sold more systems from Leads in October?**
- **How many new deals were made in the second week of October?**
- **How many leads come from the same city as the system being sold?**
- **How many deals should we follow up and how many we shouldn't?**
- **For each day, which deals should we send to which seller so they can follow up?** For example: On the 15th of October, the deals A and B should be followed up by Seller C.


### What we want from you

1. **Propose a table design that can answer the queries described efficiently.**
You can include multiple options if you feel there isn't a perfect solution. Just remember to point out the pros and cons of each.

2. **Propose and diagram a data pipeline that can ingest and transform the data from those three JSON files and update the base table with new data every half hour.**
   You can assume that every new data point will either be a new lead with a newer `CreateDate` or an old existing lead with an updated `SaleDate` prop.

3. **Propose a predictive model for system sales by seller and region over the next three months and integrate that model into the pipeline developed above as part of the ETL process designed above

4. **If you still have time, please provide more analytics and insights based on the dataset ;)

## I finished. What now?

Now you send your solution to radonyo@theprocompanies.com and I'll get back to you as soon as possible!