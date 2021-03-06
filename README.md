# Vizabi DDF Service Reader

This Vizabi DDF Service Reader is used to connect your Vizabi visualization to data that is published by a [DDF Service](https://github.com/Gapminder/big-waffle/blob/master/SERVICE_SPEC.md).  

## Usage

To use the reader include the script:

    <script src="reader-ddfservice.js"></script>

Then simply create an instance and pass it to Vizabi. 

    var ddfReader = DDFServiceReader.getReader();
    Vizabi.stores.dataSources.createAndAddType("ddfBW", ddfReader)

Next define a data specification, e.g. like this:

    const data = {
        modelType: "ddfBW",
        name: "unhcr" // e.g. version could also be in here
    }

And use that in the config of your Vizabi visualization:

    var config = {
    markers: {
        marker_destination: {
        data: {
            locale: "en",
            source: data,
            space: ["asylum_residence", "time"]
        },
        ...
        ...
        ...

## Options

Options for the reader can be given as object argument to __DDFServiceReader.getReader()__:

    var ddfReader = DDFServiceReader.getReader({service: 'http://localhost:3001'}); //reader config that's not dataset specific goes here

For now __service__ is the only option with reader scope.

In addition a dataset specific configuration object must be supplied in the __init()__ call. Vizabi supplies the
config of a data source, as shown above. The supported options are:

    {
        name: "unhcr",          // the name of the dataset, the only property that is mandatory
        version: "2019111203",   // a version string, if absent the default version supplied by the service will be used
        service: "http://localhost:3001" // it's possible to pass the service URL in here too.
    }

## Usage without vizabi
[jsfiddle example](https://jsfiddle.net/7gn91sr0/3/)

```
var ddfReader = DDFServiceReader.getReader();


ddfReader.init({
  service: 'https://big-waffle.gapminder.org', 
  name: "wdi-master"
});


ddfReader.read({
  select: {
    key: ["geo", "time"], 
    value: ["sh_sta_brtc_zs"]
  }, 
  where: {
    geo: {"$in": ["rwa"]}},
    from: "datapoints"
  })
  .then(console.log);

```