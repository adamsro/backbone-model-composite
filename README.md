# Backbone Model Composite

This library is a small extension to the basic [Backbone](http://backbonejs.org/) Model class, which supports nested models.

Define child models using a `childModels` hash.

    var ParentModel = Backbone.ModelComposite.extend({
      childModels: {
        firstChild: Backbone.Model,
        secondChild: OtherConstructor
      }
      
      //...
    });

Attributes passed to the parent model's constructor, with keys from `childModels` are used to instantiate its child models.

    var model = new ParentModel({
      firstChild: {
        a: 'abc',
        b: 2
      },
      secondChild: {}
      x: 3
    });
    
    model.firstChild.get('a'); // <-- 'abc'
    model.secondChild.set('hello', 'world');

By default, attributes used to create child models are removed from the parent object. Pass `preserveChildAttributes` in the constructor's options hash to keep these attributes on the parent.

`Backbone.Model`'s `toJSON()` method is overriden to include the nested models. Pass `excludeChildModels` in an options hash to prevent this behavior.

    model.toJSON(); // <-- {firstChild: {a: 'abc', b: 2}, secondChild: {hello: 'world'}, x: 3}    
    model.toJSON({excludeChildModels: true}); // <-- {x: 3}    

The parent object also emits its children's events, prefixed with the key from the `childModels` hash.

    var model = new ParentModel();
    var handler = function (){};
    model.on('firstChild:change', handler);
    model.firstChild.set('a', 123); // handler called
  
