# Zendesk MultiObjects Batch Source


Description
-----------
This source reads multiple objects from Zendesk.
Examples of objects are Article Comments, Post Comments, Requests Comments, Ticket Comments,
Groups, Organizations, Satisfaction Ratings, Tags, Ticket Fields,
Ticket Metrics, Ticket Metric Events, Tickets, Users.
Tags object lists the 500 most popular tags in the last 60 days.

The data which should be read is specified using object list and filters for those objects.

In addition, for each Object that will be read, this plugin will set pipeline arguments where the key is 
'multisink.[ObjectName]' and the value is the schema of the Object.

Configuration
-------------

### Basic

**Reference Name:** Name used to uniquely identify this source for lineage, annotating metadata, etc.

**Admin Email:** Zendesk admin email.

**API Token:** Zendesk API token. Can be obtained from the Zendesk Support Admin interface.
Check out [Zendesk documentation](https://support.zendesk.com/hc/en-us/articles/226022787-Generating-a-new-API-token-)
for API Token generation.

**Subdomains:** List of Zendesk Subdomains to read object from.

**Object to Pull:** Objects to pull from Zendesk API.

**Object to Skip:** Objects to Skip from Zendesk API.

**Start Date:** Filter data to include only records which have Zendesk modified date is greater than 
or equal to the specified date. The date must be provided in the date format:

|              Format              |       Format Syntax       |          Example          |
| -------------------------------- | ------------------------- | ------------------------- |
| Date, time, and time zone offset | YYYY-MM-DDThh:mm:ss+hh:mm | 1999-01-01T23:01:01+01:00 |
|                                  | YYYY-MM-DDThh:mm:ss-hh:mm | 1999-01-01T23:01:01-08:00 |
|                                  | YYYY-MM-DDThh:mm:ssZ      | 1999-01-01T23:01:01Z      |

Start Date is required for batch objects like: Ticket Comments, Organizations, Ticket Metric Events, Tickets, Users.

**End Date:** Filter data to include only records which have Zendesk modified date is less than 
the specified date. The date must be provided in the date format:

|              Format              |       Format Syntax       |          Example          |
| -------------------------------- | ------------------------- | ------------------------- |
| Date, time, and time zone offset | YYYY-MM-DDThh:mm:ss+hh:mm | 1999-01-01T23:01:01+01:00 |
|                                  | YYYY-MM-DDThh:mm:ss-hh:mm | 1999-01-01T23:01:01-08:00 |
|                                  | YYYY-MM-DDThh:mm:ssZ      | 1999-01-01T23:01:01Z      |

Specifying this along with `Start Date` allows reading data modified within a specific time window. 
If no value is provided, no upper bound is applied.

**Satisfaction Ratings Score:** Filter Satisfaction Ratings object to include only records which have Zendesk score
equal to the specified score.

**Table Name Field:** The name of the field that holds the table name. Must not be the name of any table column that
will be read. Defaults to `tablename`.

### Advanced

**Max Retry Count:** Maximum number of retry attempts.

**Connect Timeout:** Maximum time in seconds connection initialization can take.

**Read Timeout:** Maximum time in seconds fetching data from the server can take.

Data Type Mappings from Zendesk to CDAP
----------
The following table lists out different Zendesk data types, as well as the
corresponding CDAP types.

| Zendesk type           | CDAP type |
|------------------------|-----------|
| Boolean                | Boolean   |
| DateTime/Time          | string    |
| Decimal                | string    |
| int16/int34/int64/long | Long      |
| String                 | String    |
| Array                  | Array     |
| Record                 | Record    |

Data Duplication issues with Zendesk APIs
----------
This plugin can return duplicate records for time-based exports as mentioned in Zendesk docs.
[Zendesk documentation](https://developer.zendesk.com/documentation/ticketing/managing-tickets/using-the-incremental-export-api/#excluding-duplicate-items)

Objects which support time-based exports:
**Users**,
**Tickets**,
**Ticket Metric Events**,
**Organizations**, and
**Ticket Comments**.

To Solve the duplication issue, use Deduplicate plugin from Analytics list in between.