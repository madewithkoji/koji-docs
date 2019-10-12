# Database

## What is Koji Database 
Koji Database is a key-value store that is included in each Koji project. You 
can use this database for all kinds of simple use cases like collecting 
information from users, aggregating survey or poll results, creating 
leaderboards for games, and many more.

## How to use Koji Database 
The Koji database is accessed via the `@withkoji/database` npm package, 
available [here](https://www.npmjs.com/package/@withkoji/database). This 
package is open source, and you can see the source and contribute on 
[GitHub](https://github.com/madewithkoji/koji-database-sdk#readme).

Note: Because of security constraints (i.e., you don't want anybody being able 
to read or write anything they want across your entire database), it is 
important that you only connect to the database from a `backend` route.

You can install the package in your backend by running 
`npm install --save @withkoji/database` in the backend folder. You can then 
import the database into your project with:
```
import Database from '@withkoji/Database'
```

and instantiate a new instance with:
```
const database = new Database();
```

Your access credentials will automatically be configured from environment 
variables.

### Database methods

Koji Database is similar to datastores like Google Firebase. You can see 
available methods by browsing the client-side SDK source code 
[here](https://github.com/madewithkoji/koji-database-sdk/blob/master/src/adapter/DatabaseAdapter.ts).

### Uploading and storing user files

If your app allows users to upload files (images, profile pictures, audio, etc.),
you can use `@withkoji/database` to upload those files to your project's CDN. 
Simply construct a new database and upload the file using:
```
const uploadedUrl = database.uploadFile(path, filename, mimetype);
```

This will return either an `images.koji-cdn.com` or an `objects.koji-cdn.com` 
URL, depending on the type of resource uploaded. The filename specified when 
uploading the file will be automatically modified to include a random string in 
order to prevent collisions.

There is a limit of 10MB per file uploaded using this method.

## Database explorer

Navigate to the "Database" page in the "Tools" section of the your project's 
editor to view a list of all the collections and records in your database. From 
here, you can delete records or export them as CSV.

### Custom views

Developers can optionally define custom views for Database collections. If your 
database collects a lot of nested objects or uses things like UUIDs, these 
custom views can make it much easier to understand data using the Database 
explorer.

By adding a `database.json` file to the `.koji` directory of a project 
containing `database` key, you can map raw database keys to friendly names, as 
well as strip unnecessary columns from the Explorer view and control the 
default ordering of columns and sorting of records.

If a custom view is available for a database collection, it will be loaded 
automatically. You can easily switch back to the raw view to see what data is 
being stored under the hood.

A custom view that renders a leaderboard might look like:
```
{
  "database": {
      "views": [{
          "name": "Leaderboard",
          "collection": "leaderboard",
          "columns": [
              {
                  "key": "score",
                  "name": "Score",
                  "type": "number"
              },
              {
                  "key": "name",
                  "name": "Name"
              },
              {
                  "key": "privateAttributes.email",
                  "name": "Email"
              },
              {
                  "key": "dateCreated",
                  "name": "Date",
                  "type": "date"
              }
          ],
          "sort": {
              "key": "score",
              "direction": "desc"
          }
      }]
  }
}
```

## Going beyond Koji Database

While Koji database can be used to enable more complex types of persistence 
in your apps (like user accounts), it is not generally designed for those 
kinds of use cases. For example, the database does not support joins or 
subqueries.

If you're building something more complex and find yourself outgrowing the use 
cases of Koji Database, we reccommend using an off-the-shelf database product 
like Google Firebase and managing access via Keystore.
