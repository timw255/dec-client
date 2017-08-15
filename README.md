# Digital Experience Cloud Client

Unofficial client for the Sitefinity Digital Experience Cloud.

For additional information, please see [the Digital Experience Cloud documentation](https://docs.sitefinity.com/dec/api-v2/for-developers-track-and-integrate-data).

## Installing

```sh
npm install dec-client
```

Then within your application, you can reference the client with the following:

```javascript
var DECClient = require('dec-client');
```

## Usage

Create a `new DECClient()`. Make sure to provide your data center API key, and a source label.

```javascript
var client = new DECClient({
     apiKey: 'your-api-key',
     source: 'your-datasource-label'
});
```

### Writing Sentences

[Sentence documentation](https://docs.sitefinity.com/dec/api-v2/for-developers-sentences)

```javascript
client.writeSentence({
    subjectKey: 'the-subject-key',
    predicate: 'the-predicate',
    object: 'the-object'
});
```

### Tracking Subject Metadata

[Subject Metadata documentation](https://docs.sitefinity.com/dec/api-v2/for-developers-subjectmetadata)

```javascript
client.writeSubjectMetadata('the-subject-key', {
    FirstName: 'first-name',
    LastName: 'last-name',
    Email: 'contact-email'
    // additional meta fields...
});
```

### Adding Subject Mapping

[Subject Mapping documentation](https://docs.sitefinity.com/dec/api-v2/for-developers-subjectmapping)

```javascript
client.addMapping('the-subject-key', 'second-subject-key', 'second-datasource-label');
```

### Sending Sentences to the Digital Experience Cloud 

The client allows you to write multiple interactions [sentences] at a time. Once all interactions have been written, you must `flushData()` to actually send them to the DEC.

```javascript
// returns a promise
client.flushData();
```

## Personalization

You can make a call to the DEC to retrieve information about the subject's Personas, Campaigns, and Lead Scores.

Create a `new DECClient()`. Make sure to provide your data center API key, a source label, and an authToken.

```javascript
var client = new DECClient({
     apiKey: 'your-api-key',
     source: 'your-datasource-label',
     authToken: 'appauth your-app-auth-key'
});
```
The `authToken` is used by an authorized application to request data from the personalization end-points. You can construct the token by concatenating the "appauth " string with the Access Key of the authorized application, for example: "appauth 94e8b2d2-931e-cd01-b57b-07fa2013cb24".

An Authorized Application can be configured by navigating to the "Authorized applications" in the Administration section of your Data Center.

```javascript
client.isInPersonas('1', 'the-subject-key')
.then(function (response) {
    var personas = JSON.parse(response).items;

    if (personas.length) {
        // do something
    }
})
```

**Example Response**

```JSON
{"items":[{"Id":1,"Score":100,"ThresholdPassedOn":"2017-08-11T01:45:21.120Z","Threshold":100}]}
```
