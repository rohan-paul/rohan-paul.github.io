# Aggregation in Mongodb

What are aggregations?

![](https://cdn-images-1.medium.com/max/1200/1*6AMAmSIfHotp9XybUDqK4A.jpeg)
undefined

undefined
undefined

### What are aggregations?

When it’s time to gather the metrics from **MongoDB**, may be for some graphical representation or some other operation, there is no better technique than **MongoDB** **aggregations**. Aggregation in MongoDB is an operation used to process the data that returns the computed results. Aggregation basically groups the data from multiple documents and operates in many ways on those grouped data in order to return one combined result. It processes documents and return computed results and can perform a variety of operations on the grouped data to return a single result.

Using the CRUD’s find operation while fetching data in MongoDB may sometimes become tedious. For instance, I may want to fetch some embedded documents in a given field but the **find({})** operation will always fetch the main document and then it will be upon me to filter this data and select a field with all the embedded documents, scan through it to get ones that match your criteria. Since there is no shorter way to do this, I will be forced to use one form of a loop operation to go through all these subdocuments until I get the matching results. However, when I have a million embedded documents, the process will take a lot of your server’s random memory and may terminate the process before I get all the documents you wanted, as the server document size may be surpassed.

This limitations associated with a large number of documents, leads us to the technique of grouping them to enhance the scanning process. Aggregation framework is therefore an operation process which manipulates documents in different stages, processes them in accordance with the provided criteria and then return the computed results. Values from multiple documents are grouped together, on which more operations can be performed to return matching results.

Aggregations can be used to apply a sequence of query-operations to the documents in a collection, reducing and transforming them. With aggregations, we can perform queries that offer similar functionality to the behaviors we might expect to see in a relational database query.

In Mongo, aggregations work as a pipeline, or a list of operators/filters applied to the data. We can pipe a collection into the top and transform it though a series of operations, eventually popping a result out the bottom.

**Operators come in three varieties: stages, expressions, and accumulators.**

When calling aggregate on a collection, we pass a list of stage operators. Documents are processed through the stages in sequence, with each stage applying to each document individually. Most importantly remember, **that aggregation is a “pipeline” and is just exactly that, being “piped” processes that feed input into the each stage as it goes along**. The output from the first thing goes to the next thing and then that manipulates to give input to the next thing and so on.

![](https://cdn-images-1.medium.com/max/800/1*oayVcbweIcI2IZ4AJa5moQ.gif)
undefined

To read more about this Pipeline process their [**beautiful official documentation page.**](https://docs.mongodb.com/manual/aggregation/)

Now, lets start with an example. Lets say we have publishing house company’s dat abase that sells books in varius cities. For this example, **there are three data schemas or mongodb models -**

*   **Book** (this will have a reference to the City model, being the city where the author lives and first published the book)
*   **Author** — Will have a reference to City (being the city where the author lives )
*   **City**

And now here’s the data thats saved in Mongodb for the Books model

![](https://cdn-images-1.medium.com/max/800/1*R4_9rgR_zMDJgV7KgKgA7Q.png)
undefined

The below is data for Author model

![](https://cdn-images-1.medium.com/max/800/1*pFSronaEfpBz2oJYEE3Svw.png)
undefined

And the below is our data for the City Mongodb Schema

![](https://cdn-images-1.medium.com/max/800/1*3KThx1IYlzqYn-wXBcyK8A.png)
undefined

**Now, lets say I want to do a pie or bar-chart graph showing the total number of books sold in each city in the current month.** For this, I want to hit the database and get an aggregated data for for the total number of books sold for each city. So I would want two arrays of data, one giving me the names of cites and the other giving me the total number of books sold in those cities.

Once I get that data from the database, I can re-arrange that according to my needs to properly restructure for showing in the UI.

So here’s my soution, in my Express backend routing / controller file — I will have a “/booksanalytics” route and the below is the code using mongodb aggregate.

![](https://cdn-images-1.medium.com/max/800/1*Af2n9xGT5aOAAmKgvLF47w.png)
undefined

And when I hit the route, the response from this route is below, just what I needed.

![](https://cdn-images-1.medium.com/max/800/1*0V20d6XkuyFj288GWPEECQ.png)
undefined

Now lets understand the code above.

First of all undersand that the The **aggregate function accepts an array of data transformations which are applied to the data in the order they’re defined. This makes aggregation a lot like other data flow pipelines: the transformations that are defined first will be executed first and the result will be used by the next transformation in the sequence**.

**So Books.aggregate() takes a sequence of data aggregation operations or stages**. See the [aggregation pipeline operators](https://docs.mongodb.com/manual/reference/operator/aggregation-pipeline/) for details. Here I am only using three of them — **$match, $group, $project**

#### **$match**

The first stage of a pipeline is **matching**, and that allows to filter out the documents so that we are only manipulating the documents that we care about. So, in above I am first picking up only data falling between the starting and ending date of the month. The $gte and $lt operators are comparators that allow us to limit our dates to specific range. In our case, we want the **date\_written** to be greater than or equal to the first day in current-month and less than the first day of next month.

$match: {

  date\_written: {

     $gte: startDate,

     $lt: endDate

}

The the output of this pipeline is passed to the next set of operation for processing, which is $group

In order to achieve the best performance of the $match stage, use it early in the aggregation process since it will:

1.  Take advantage of the indexes hence become much faster
2.  Limit the number of documents that will be passed to the next stage.

#### $group

$group: {

  \_id: "$city",

  total\_books\_written: {

     $sum: "$no\_of\_books\_sold"  
  }

}

**In the above stage of the pipeline, the output will be documents that have an “**`**_id"**` **with a distinct ‘city’ code as a value and so will group together documents input to this stage that have the same ‘city’ code**, and a **“total\_books\_written**`"` field whose value is the sum of all the **“no\_of\_books\_sold”** field values from each of the documents in the group with an aggregation expressions named **$sum**

We have many more **aggregation expressions** available, following is a list

undefined
undefined

So the outpur from the above **$group operator** is the following document-structructure (and which will be passed for the next stage of the pipeline to be prcessed by the next operator, which in this case would be $project).

![](https://cdn-images-1.medium.com/max/800/1*vlhNslKF-uw-GREByB8SSg.png)
undefined

Note that it has combined the City schema’s data to with “no\_of\_books\_sold” field from the Book schema and added the new field ‘total\_books\_written” for the sum of all the “no\_of\_books\_sold” for each Book.

We can quickly check the above $group operator by running these aggregation query in MongoDB Browser Studio-3T.

You just need to install [**studio 3T**](https://studio3t.com/download/) into your machine,

*   Create a new database and connect to it.
*   And then insert a new collections named **Book** to that database**.**
*   And insert the three data-points or documents in Mongodb’s terminology, to that Book Collection (the same above dummy data (the one I am referring at the top screenshort) and then just follow [**this official guide to run the aggregation query**](https://studio3t.com/knowledge-base/articles/build-mongodb-aggregation-queries/) **on the Book collection.**

In the Aggregation wizard you have to select **$group** as the name of the operator and write the following inside

{

  \_id: "$city",

  total\_books\_written: {

    $sum: "$no\_of\_books\_sold"

  }

}

And the below will be result inside Studio

![](https://cdn-images-1.medium.com/max/800/1*43z0y9oXEQP1xl6RbtLVvg.png)
undefined

Now the next operator in the Pipeline stage, which is the $project

#### [$project](https://docs.mongodb.com/manual/reference/operator/aggregation/project/)

$project: {

  \_id: "$\_id",

  total\_books\_written: "$total\_books\_written",

  city: "$\_id"

}

The **$project** function in MongoDB passes along the documents with only the specified fields to the next stage in the pipeline. This may be the existing fields from the input documents or newly computed fields. The purpose is, we’re going to want to reduce the documents for the next stage in the Pipeline, into smaller objects — returning only the fields we want, or aliasing their names. The documents returned to the next stage of the pipeline **_will only contain_** the values specified by `$project.`

#### Few words on the [Collection.populate](https://mongoosejs.com/docs/api.html#model_Model.populate) function in the above code

![](https://cdn-images-1.medium.com/max/800/1*0P_7r66IjYOJXIRlgNto0g.png)
undefined

After the all the three previous aggreation operator’s Piping functions are done working on the Books data, I will get a sample like below as the final output to be passed to the .exec funtion.

{ \_id: 5cade9a945a3df38a9bb8503,  
  total\_books\_written: 42,  
  city: 5cade9a945a3df38a9bb8503   
}

.populate() is a way to populate referenced subdocuments in any schema. Also, note that `populate` works only for queries so you need to first pass your item into a query and then call it.

Now in the **.populate** function, I am taking the output result from all the three previous aggreation operator’s Piping functions — and passing that result (which is the above object having three fields “**\_id”, “total\_books\_written” and “city”**) to populate the City database schema. But, while invoking .populate, I am also specifying that from the ‘result’ object only look for the fieldName ‘city’ with the option

path: 'city'

and with the ‘select’, I only want to return the name field’s value from the City database. Henece I use

select: "name"

Calling .exec() just executes something once previous operations have done it’s works (which in this example are the three aggregation operations).

#### More exlanation around $project

So, consider what **$project** does in the above specific case. I specify fields I want in the output and it “spits them out” (sometime I could do some manipulation here). So in above, I get exactly three fields after $project has done its work at this stage of the Pipe.

M**ost importantly, does it output these fields in “addition” to the fields in the document ? No It does not. Only what you ask to come out actually comes out and can be used in the following pipeline stage(s).**

To understand this point lets see an example of a wrong $project implementation. Lets say I have the following sample data.

```
{    "_id": {        "$oid": "56348e7938b1ab3c382d3363"    },    "carer_id": "55e6f647f081105c299bb45d",    "user_id": "55f000a2878075c416ff9879",    "starting_date": {        "$date": "2015-10-15T05:41:00.000Z"    },    "ending_date": {        "$date": "2015-11-19T10:03:00.000Z"    },    "amount": "850",    "total_days": "25",    }{    "_id": {        "$oid": "563b5747d6e0a50300a1059a"    },    "carer_id": "55e6f647f081105c299bb45d",    "user_id": "55f000a2878075c416ff9879",    "starting_date": {        "$date": "2015-11-06T04:40:00.000Z"    },    "ending_date": {        "$date": "2015-11-16T04:40:00.000Z"    },    "amount": "25",    "total_days": "10"   }
```

And I am structure my aggregate code as below (Note that its an incorrect implementataion, so we understand how $project work)

```
{        $project: {            myyear: {$year: "$ending_date"}        }    },    {         $match: {             carer_id : req.params.carer_id,            status : 3,            $myyear : "2015"        }    }
```

The `$match` operator in the above is essentially asking for fields that are no longer present when the JSON data comes to it. Because $project has already reduces the data. The same problem will occurs further down, if I am invoking any other operator further down the pipe. Because as again I would ask for fields I have already removed earlier and there simply is nothing to reference, but also everything was already removed by a `$match` that could not match anything.

The specification for [**$project command**](https://docs.mongodb.com/manual/reference/operator/aggregation/project/) contain the inclusion of fields, the suppression of the \_id field, the addition of new fields, and the resetting the values of existing fields.

**Points to note on $project specification:**

*   By default, the \_id field is included in the output documents. To exclude the \_id field from the output documents, you must explicitly specify the suppression of the \_id field in $project.
*   If a field is described with a value of 1 or true, the document that is to be returned will have that field.
*   You can suppress the \_id field so that it cannot be returned by describing it with 0 or false value.
*   The $project operation will basically treat a numeric or boolean values as flags. For this reason, you will need to use the $literal operator for you to set a field value numeric or boolean.
*   A field that does not exist in the document are going to include, the $project ignores that field inclusion;
*   A new field can be added and value of an existing field can be reset by specifying the field name and set its value to some <expression>.We can indeed compose very complex serialization routines using $project with <expression> .

**Creating Dynamic Fields with $project**  
We can use project to add new fields to our documents based on expressions. Say we had a list of people in my User database, like below:

![](https://cdn-images-1.medium.com/max/800/1*Anp1UzJwEDtIv8_33878cw.png)
undefined

We can use $project to compose a name field dynamically, with below code (the code is using Mongo shell syntax and created inside [Mongodb browser Studio-3T](https://studio3t.com/download/))

![](https://cdn-images-1.medium.com/max/800/1*s4xVLXS1LfUmp2TyLPLABQ.png)
undefined

And here’s the output

![](https://cdn-images-1.medium.com/max/800/1*JsWRuUEG6Lf5SdbxnxPHAA.png)
undefined

And the aggregatio query inside Studio-3T was this

![](https://cdn-images-1.medium.com/max/800/1*BoWk18zywzjZAolbuWt8Fg.png)
undefined

Lets go through another exmaple to find usage of some other **aggregation expressions** like $min, $max and $avg

Lets assume we have the following Transaction collection in our mongodb.

{

"\_id" : ObjectId("5caf285d4ea2f23a4914649b"),

"productId" : "1",

"customerId" : "1",

"amount" : 20.0,

"transactionDate" : ISODate("2016-02-23T15:25:56.314+0000")

}

// ----------------------------------------------

{

"\_id" : ObjectId("5caf28874ea2f23a491464a0"),

"productId" : "2",

"customerId" : "2",

"amount" : 29.0,

"transactionDate" : ISODate("2016-04-23T15:25:56.314+0000")

}

// ----------------------------------------------

{

"\_id" : ObjectId("5caf28b04ea2f23a491464a5"),

"productId" : "3",

"customerId" : "3",

"amount" : 85.0,

"transactionDate" : ISODate("2019-02-23T15:25:56.314+0000")

}

// ----------------------------------------------

{

"\_id" : ObjectId("5caf28d44ea2f23a491464a6"),

"productId" : "4",

"customerId" : "4",

"amount" : 12.0,

"transactionDate" : ISODate("2017-06-23T15:25:56.314+0000")

}

And I want to the total transaction amount, average transaction amount, min and max transaction amount — all between two dates 2016–01–01 and 2018–01–31

So my $match and $group will look like below

![](https://cdn-images-1.medium.com/max/800/1*rS_r59YYJvFhmEL_nIa0Kw.png)
undefined

And the final output will be as follows

{

"\_id" : null,

"total" : 61.0,

"average\_transaction\_amount" : 20.333333333333332,

"min\_transaction\_amount" : 12.0,

"max\_transaction\_amount" : 29.0

}

And here was my Aggregation setup code in MongoDB Studio-3T

![](https://cdn-images-1.medium.com/max/800/1*k7eY4sdr5n9C6PTqUqCSVA.png)
undefined![](https://cdn-images-1.medium.com/max/800/1*Z3hwYBKeyI9kKUr59Vqajw.png)
undefined

By [Rohan Paul](https://medium.com/@paulrohan) on [April 11, 2019](https://medium.com/p/8195c8624337).

[Canonical link](https://medium.com/@paulrohan/aggregation-in-mongodb-8195c8624337)

Exported from [Medium](https://medium.com) on December 12, 2020.