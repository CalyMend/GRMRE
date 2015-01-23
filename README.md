# GRMRE
237TAP

Documentation
Release 1.0.2
March 14, 2014
CONTENTS
1 Getting Started
1
1.1 Introduction to Cloudant
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
1
1.2 Prerequisites and Basics
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
5
1.3 Create Read Update Delete (CRUD)
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
9
1.4 Introduction to Querying
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
19
2 API Reference
31
2.1 API Basics
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
31
2.2 Authentication Methods
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
36
2.3 Authorization Settings
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
37
2.4 Databases
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
39
2.5 Documents
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
58
2.6 Design Documents
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
71
2.7 Miscellaneous
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
96
2.8 Local (non-replicating) Documents
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
104
3 Using Cloudant With...
107
3.1 Python
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
107
3.2 Node.js
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
111
3.3 .NET / Mono
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
116
4 Guides
119
4.1 The CAP Theorem
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
119
4.2 MapReduce
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
120
4.3 Document Versioning and MVCC
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
124
4.4 CouchApps and Tiers of Application Architecture
. . . . . . . . . . . . . . . . . . . . . . . . .
127
4.5 Replication
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
127
4.6 Back up your data
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
133
4.7 How to monitor indexing and replication tasks
. . . . . . . . . . . . . . . . . . . . . . . . . . .
136
4.8 Data that Moves: Switching Clusters
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
139
4.9 Transactions in Cloudant
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
139
i
ii
CHAPTER
ONE
GETTING STARTED
The Getting Started guide is a collection of newly written tutorials, links to currently available documenation for
Cloudant and Apache CouchDB, and links to relevant API references. In addition, we will use material from
the Cloudant Blog, stackoverflow.com and other external sites. It is organized in a way to help you learn to use
Cloudant from the ground up with no prior knowledge. These will lead you from the basics of HTTP and JSON
up through to our most advanced features, including examples and discussions on application design.
1.1
Introduction to Cloudant
What is Cloudant?
Cloudant is a hosted and managed distributed database-as-a-service, based on a horizontally scalable version of
Apache CouchDB and other tools. A Cloudant database is a schemaless JSON document store sharded across a set
of nodes (a la Dynamo), featuring incrementally-updated MapReduce Views, secondary key indexing, built-in full
text search (based on
Lucene
), geo-spatial indexing, multi-master replication (supports global data distribution)
and communicates over an HTTP RESTful API. Cloudant hosts and completely manages your database, including
24-hour support, at a fraction of the cost of a full-time administrator.
These Getting Started documents will guide you through the process of learning exactly what that means and how
to use it.
1.1.1
Conceptual Introduction
We break down the concepts of the opening paragraph above and briefly explain each idea, giving you a rough
overview of the technology.
Please note, when learning to use Cloudant and other document stores, it may be useful to avoid trying to map the
concepts of relational database management systems (RDMS) to the concepts of distributed document stores that
use MapReduce. These systems are significantly different and its easier, from our personal anecdotal experience,
to accept the document store ideas at face value without trying to find the identical concepts in relational systems.
For example, it may be easier to understand MapReduce Views if you don’t try to figure out how it relates to
relational JOINs. MapReduce and JOINs are distinct and fundamental operations on the data in their respective
systems. They work in entirely different ways and they are used by developers in different ways. (Although you
can mimic a JOIN-like operation with MapReduce, which we will get to.) Once you’ve become familiar with
Cloudant, then its easier to compare the concepts to RDMS concepts and the impact on designs of applications
built with those database systems. So, clear your mind, accept the paradigm shift and let it all sink in.
Without futher ado...
Database Management System
The database management system is just that - a system that manages database-level operations related to the
distribution of data across the nodes in your cluster, the execution of MapReduce Views, load balancing of client
requests, data consistency and I/O operations. For each database in your Cloudant-hosted account, you will not
need to do anything related to the system on which those databases reside. Your interaction with your Cloudant
1
Documentation, Release 1.0.2
database will be restricted to making HTTP requests to add data, defining MapReduce Views, search index func-
tions, and other special server-side functions that are tailored to your application, and then retrieving those results.
Document Store
For most novices, when they hear the word ‘database’ a number of things come to mind. People are informed about
databases through academic course work and pop culture (movies, TV, country music, etc.). In addition, databases
had been relatively static for the past 30 years or so (before Google’s MapReduce and Amazon’s Dynamo paradigm
shifts). So, when novices hear the word ‘database’ they often think of tables, which are the typical representations
of relations in a relational database system. However, this is not at all what a Cloudant database looks like.
A Cloudant database is a collection of JSON documents. In a sense, a JSON document store is almost completely
orthogonal to a relational table. A JSON document is a set of key-value pairs, whereas a relation is a set of tuples.
Key-Value
A key-value pair is simply the name of something (key) and its value. In a JSON document it looks something
like this
{
"name"
:
"Adam"
}
JSON documents
Imagine that you have a relation with a schema that has three columns “name”, “age”, and “gender”. Each entry in
the relation would be a tuple that looks something like (“Adam”, 26, “M”) or (“Sue”, 32, “F”). A JSON document
would contain both the column name, as the key, and the value. These tuples would be stored in a JSON document
like this.
A value can be a string, number, an array or another object (set of key-value pairs). Here’s an example JSON
document
{
"name"
:
"adam"
,
"age"
:
26
,
"car"
: {
"make"
:
"ford"
,
"model"
:
"mustang"
,
"year"
:
1965
},
"numbers"
: [
7
,
21
,
"goats"
],
"colors"
: [
"blue"
,
"green"
,
{
"cheese"
:
"caprifeuille"
}
],
"friends"
: [
{
"name"
:
"sean"
,
2 Chapter 1. Getting Started
Documentation, Release 1.0.2
curl -X PUT -u username https://username.cloudant.com/users
With Python, you’d do the following
import
requests
import
json
#basic authentication
auth
=
(
’
username
’
,
’
password
’
)
post_url
=
"
http://{0}.cloudant.com/users
"
.
format(auth[
0
])
#r is a Response instance
r
=
requests
.
put(post_url, auth
=
auth)
print
json
.
dumps(r
.
json(), indent
=
1
)
Both methods should return
{"ok":true}
.
To visually verify the success of this, you can sign in to your account on cloudant.com and see your database in
the dashboard.
Create
Create a new document on your new database.
API Reference: POST /db and PUT /db/doc
For simplification, we’ll let the database generate the
"_id"
value. If you want to set the
"_id"
, just include it
in the document that you write to the database.
curl -X POST -H
’Content-Type: application/json’
-d
’{"first_name":"bob", "high_score": 550, "level": 4}’
-u username https://username.cloudant.com/users
import
requests
import
json
#the database JSON document maps to a Python dictionary
doc
=
{
#’_id’:’bob456’, #uncomment this line if you want to specify the _id
’
first_name
’
:
’
bob
’
,
’
high_score
’
:
550
,
’
level
’
:
4
}
auth
=
(
’
username
’
,
’
password
’
)
headers
=
{
’
Content-type
’
:
’
application/json
’
}
post_url
=
"
http://{0}.cloudant.com/users
"
.
format(auth[
0
])
r
=
requests
.
post(
post_url,
auth
=
auth,
headers
=
headers,
data
=
json
.
dumps(doc)
)
print
json
.
dumps(r
.
json(), indent
=
1
)
The Response object returned by the requests module contains the HTTP response code (
r.status_code
) and
data (
r.text
), which can be decoded as JSON via
r.json()
.
Additionally, one can use the PUT /db/doc to create a new document. The difference is that you must specify the
"_id"
of the document in the URI (gotcha: when using
PUT /db/doc
, if the
"_id"
key is specified in the
document, it will be ignored and the
"_id"
in the URI will be used).
10 Chapter 1. Getting Started
Documentation, Release 1.0.2
curl -X PUT -H
’Content-Type: application/json’
-d
’{"first_name":"bob", "high_score": 850, "level": 6}’
-u username https://username.cloudant.com/users/bob456
import
requests
import
json
#the database JSON document maps to a Python dictionary
doc
=
{
’
first_name
’
:
’
bob
’
,
’
high_score
’
:
850
,
’
level
’
:
6
}
auth
=
(
’
username
’
,
’
password
’
)
headers
=
{
’
Content-type
’
:
’
application/json
’
}
post_url
=
"
http://{0}.cloudant.com/users/bob456
"
.
format(auth[
0
])
r
=
requests
.
put(
post_url,
auth
=
auth,
headers
=
headers,
data
=
json
.
dumps(doc)
)
print
json
.
dumps(r
.
json(), indent
=
1
)
Read
Documents can be retrieved by knowing the value of the
"_id"
and constructing an HTTP GET request to the
proper URI.
API Reference:
GET /db/doc
In the example above, the value of
"_id"
assigned to the document was
0562df6ffcc4301b1e2c5d1061214489
. Yours, of course, will be different. The URI for this doc-
ument is
https://username.cloudant.com/users/0562df6ffcc4301b1e2c5d1061214489
If you used the
"bob456"
for the document
"_id"
, then it would be
https://username.cloudant.com/users/bob456
Here is an example curl command to retrieve this document
curl -X GET -u username https://username.cloudant.com/users/0562df6ffcc4301b1e2c5d1061214489 | jq .
and equivalent code in Python
import
requests
import
json
auth
=
(
’
username
’
,
’
password
’
)
get_url
=
"
http://{0}.cloudant.com/users/0562df6ffcc4301b1e2c5d1061214489
"
.
format(auth[
0
])
r
=
requests
.
get(get_url, auth
=
auth)
print
json
.
dumps(r
.
json(), indent
=
1
)
These examples should print the same document to screen. You will now notice that your document contains the
new keys,
"_id"
and
"_rev"
.
1.3. Create Read Update Delete (CRUD) 11
