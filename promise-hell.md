The async hell (aka _callback hell_ or _promise hell_) is a way of programming asynchronouse operations in which consecutive steps are defined inside scope of a previous step.

```js
function getModel(req, res, next) {
  findRelatedProperties(req.model)
    .then(function assignPropertiesAndConfigurations(relatedProperties) {
      Configuration.findBaseConfiguration(req.model.id)
        .then(function respond(baseConfiguration) {
          res.json({
            configurations: baseConfiguration ? [baseConfiguration] : [],

            model: _.assign(
              relatedProperties, toCamelCase(req.model)
            )
          });
        });
    })
    .catch(next);
}
```

## Extract functional expressions into definitions

Just to make clear that code does not handled errors properly.

```js
function getModel(req, res, next) {
  findRelatedProperties(req.model).then(assignPropertiesAndConfigurations).catch(next);
  
  function assignPropertiesAndConfigurations(relatedProperties) {
    Configuration.findBaseConfiguration(req.model.id).then(respond);
    
    function respond(baseConfiguration) {
      res.json({
        configurations: baseConfiguration ? [baseConfiguration] : [],

        model: _.assign(
          relatedProperties, toCamelCase(req.model)
        )
      });
    }
  }
}
```
