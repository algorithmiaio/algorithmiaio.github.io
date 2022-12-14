---
author: jon_peck
categories: sql-patterns
excerpt: "Accessing your Snowflake database from within an Algorithm"
image:
    teaser: /language_logos/snowflake_computing.png
layout: article
redirect_from:
  - /data/snowflake/
  - /other-data-sources/snowflake/
show_related: true
tags: [sql-patterns]
title:  "Snowflake"
---

Algorithms can easily access databases hosted on the Snowflake data platform using the [Snowflake Connector for Python](https://pypi.org/project/snowflake-connector-python/). To see this in action on Algorithmia, check out the [SnowflakeAsyncOrchestrator]({{site.url}}/algorithms/algorithmiahq/SnowflakeAsyncOrchestrator) algorithm, which provides a reference architecture for performing an asynchronous database write operation. If you'd like to get started with a less complex example, you can follow along below. Note that to complete the following, you'll need an active account on Snowflake.

To begin, create a [hosted data collection]({{site.url}}/data/hosted) named `SnowflakeCredentials` under the ownership of your own Algorithmia account and upload a file `credentials.json` with the following structure (see the Snowflake docs to find your [Account Name](https://docs.snowflake.net/manuals/user-guide/connecting.html)). This file contains session-level parameters that'll enable you to establish a connection with your Snowflake instance.

```json
{
  "user": "SNOWFLAKE_USERNAME",
  "password": "SNOWFLAKE_PASSWORD",
  "account": "SNOWFLAKE_ACCOUNT",
  "application": "Algorithmia"
}
```

Next, create a generic Python algorithm under the ownership of your account. Click "Dependencies" in the Web IDE (or edit your `requirements.txt` file) and add the dependency `snowflake-connector-python`.

Now paste in the following as your algorithm code, replacing `ALGORITHMIA_ACCOUNT_NAME` with your account name so that it will point to the correct data collection and have the appropriate access permissions.

```python
import Algorithmia
import snowflake.connector

# Get Snowflake credentials.
client = Algorithmia.client()
creds = client.file("data://ALGORITHMIA_ACCOUNT_NAME/SnowflakeCredentials/credentials.json").getJson()

def apply(input):
    con = snowflake.connector.connect(
        user=creds.get("user"),
        password=creds.get("password"),
        account=creds.get("account"),
        application=creds.get("application")
        )
    cs = con.cursor()
    try:
        cs.execute("USE SNOWFLAKE_SAMPLE_DATA")
        cs.execute("SELECT * FROM TPCDS_SF100TCL.CALL_CENTER")
        one_row = cs.fetchone()
        return [str(i) for i in one_row]
    finally:
        cs.close()
        ctx.close()
```

Build and test the algorithm. Assuming you haven't deleted the `SNOWFLAKE_SAMPLE_DATA` database from your Snowflake account, it should return the first row from the sample `CALL_CENTER` table. Once you verify that the connection is working, you can modify the code to point at your own database tables as appropriate.
