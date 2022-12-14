---
author: jon_peck
categories: sql-patterns
excerpt: 'Run queries against MySQL databases'
image:
  teaser: /language_logos/mysql.png
layout: article
redirect_from:
  - /data/mysql/
show_related: true
tags: [sql-patterns]
title: "MySQL"
---

If your algorithm needs to read or write data from a MySQL database, you can do so by either making the database connection directly from within your own code, or by using our helper algorithms.

### Option 1: Connect directly from within your own algorithm code

There are a variety of MySQL packages publicly available. For Python, we recommend [PyMySql](https://pymysql.readthedocs.io/en/latest/). For other languages, see [w3resource](http://w3resource.com/mysql/mysql-connectors-and-apis.php).

We don't recommend storing your database credentials directly inside your algorithm, since this would require re-publishing it anytime they change, and would make them visible to anyone with access to your source code.

Instead, create a folder within a hosted data collection [Data Portal]({{site.baseurl}}/data) and set its read access to "Private to your algorithms" (this allows your algorithm to utilize the database regardless of who calls it, but does not give the caller direct access to your credentials).

Inside the collection, create a `.json` file containing your connection credentials.

```
{
  "host":"SERVER_NAME.net",
  "user":"DATABASE_USER_NAME",
  "passwd":"DATABASE_PASSWORD",
  "db":"DATABASE_NAME"
}
```

Then, in your algorithm source code's dependencies file, add whatever MySQL library you'll be using (in this example, `PyMySql`). Then, load the credentials from the JSON file and use them to make your database connection.

```
import Algorithmia
import pymysql

client = Algorithmia.client()

def apply(input):
    query = "SELECT name, address FROM employees"
    # Load the credentials file and make sure it has the required fields
    try:
        # Replace the data:// path below with your credentials file
        creds_file = 'data://.my/COLLECTION_NAME/MySqlCredentials.json'
        creds = client.file(creds_file).getJson()
        assert {'host','user','passwd','db'}.issubset(creds)
    except:
        raise Exception('Unable to load database credentials')
    # Connect to the database and run a query
    conn = pymysql.connect(**creds)
    cursor = conn.cursor()
    cursor.execute(query)
    # This example aggregates and returns the results, but generally
    # you'd use it in further calculations
    rows = cursor.fetchall()
    result = []
    for row in rows:
      result.append(row)
    return result
```

### Option 2: Use our helper algorithms to store per-user credentials automatically, and to run queries

If you don't want to add database connection code directly into your algorithm, you can use our helper algorithms instead. Keep in mind that these incur the usual 1 credit per compute-second cost to run.

First, configure your MySQL Database connection via <a href="{{site.url}}/algorithms/util/MySqlConfig">MySqlConfig</a> ( <a href="{{site.url}}/algorithms/util/MySqlConfig/docs">docs</a>). Note that this creates credentials that are available only from your account, another user wants to utilize this connection from another account, they'll need to run <a href="{{site.url}}/algorithms/util/MySqlConfig">MySqlConfig</a> as well.

Then, access your database via the <a href="{{site.url}}/algorithms/util/MySql">MySQL</a> (<a href="{{site.url}}/algorithms/util/MySql/docs">docs</a>).

Here's an example of using a preconfigured connection inside one of your own algorithms within the Web IDE:

```
import Algorithmia

client = Algorithmia.client()

def apply(input):
    query = "SELECT name, address FROM employees"
    results = client.algo('util/MySql/0.1.1').pipe(query).result
    # Additional code to do whatever you'd like with results (a list of lists)
```
