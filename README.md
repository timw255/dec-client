# Digital Experience Cloud Client

Unofficial DEC SDK for the Sitefinity Digital Experience Cloud.

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

Create a `new DECClient()`, make sure to provide your data denter API key, a source label.

```javascript
var client = new DECClient({
     apiKey: 'your-api-key',
     source: 'your-datasource-label',
     authToken: 'appauth your-app-auth-key'
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
client.writeSubjectMetadata({
	'subjectKey': 'the-subject-key'
	'FirstName': 'My first name',
    'LastName': 'My last name'
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
client.flushData();
```

## Personalization

You can make a call to the DEC to retrieve information about the subject's Personas, Campaigns, and Lead Scores.

Create a `new DECClient()`, make sure to provide your data denter API key, a source label.

```javascript
var client = new DECClient({
     apiKey: 'your-api-key',
     source: 'your-datasource-label',
     authToken: 'appauth your-app-auth-key'
});
```
Make sure to include an `authToken`. This token is used by an authorized application to request data from the personalization end-points. You can construct the token by concatenating the "appauth " string with the Access Key of the authorized application, for example: "appauth 94e8b2d2-931e-cd01-b57b-07fa2013cb24".

An Authorized Application can be configured by navigating to the "Authorized applications" section of your Data Center.

```javascript
client.isInPersonas('1', 'subjectKey')
.then(function (response) {
	var personas = JSON.parse(response).items;

	if (personas.length) {
		// do something
	}
})
```