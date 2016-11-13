# The file tree

```bash
kevinwandeMacBook-Pro:plugins kevinwan$ tree tr-k4p-clock/
tr-k4p-clock/
├── index.js
├── package.json
└── public
    ├── clock-editor.html
    ├── clock.css
    ├── clock.html
    └── clock.js
```

# Part 1
- If you want's to know index.js, package.json pls go to README.md

# Part 2 - plblic
## package.json

```json
{
        "name": "tr-k4p-clock",
        "version": "0.4.0"
}
```

## index.js

```js
module.exports = function(kibana) {
        // Return a new instance of kibana.Plugin that holds
        // information about this plugin.
        return new kibana.Plugin({
                // We have some ui components, that we need to describe
                uiExports: {
                        // Register our visualizations (a plugin can have multiple visualizations)
                        visTypes: [
                                'plugins/tr-k4p-clock/clock'
                        ]
                }
        });
};
```

## public/index.js
- First get kibana plugin "tr-k4p-clock" module object 
- Then define a Angularjs controller. Angularjs controller controls how the html int it's scope will be phrased by IE.
- All JavaScript files in the public folder of your plugin should be RequireJS (AMD) modules and must be wrapped in define(function(require) { ... }).


  
```js
 // Create an Angular module for this plugin
    var module = require('ui/modules').get('tr-k4p-clock');
    // Add a controller to this module
    module.controller('ClockController', function($scope, $timeout) {
   
        var setTime = function() {      
            $scope.time = Date.now();       
            $timeout(setTime, 1000);        
        };
        setTime();
   
    });
   
    // The provider function must return the visualization
    function ClockProvider(Private) {
        // Load TemplateVisType
        var TemplateVisType = Private(require('ui/template_vis_type/TemplateVisType'));
   
        // Return a new instance describing this visualization
        return new TemplateVisType({    
            name: 'trClock', // the internal id of the visualization
            title: 'Clock', // the name shown in the visualize list
            icon: 'fa-clock-o', // the class of the font awesome icon for this
            description: 'Add a digital clock to your dashboards.', // description shown to the user
            requiresSearch: false, // Cannot be linked to a search 
            template: require('plugins/tr-k4p-clock/clock.html'), // Load the template of the visualization
            params: {
                editor: require('plugins/tr-k4p-clock/clock-editor.html'), // Use this HTML as an options editor for this vis
                defaults: { // Set default values for paramters (that can be configured in the editor)
                    format: 'HH:mm:ss'              
                }
            }
        });
    }
   
    // Register the above provider to the visualization registry
    require('ui/registry/vis_types').register(ClockProvider);
   
    // Return the provider, so you potentially load it with RequireJS.
    // This isn't mandatory, but since all Kibana plugins do this, you might
    // want to also return the provider.
    return ClockProvider;

});
```
## public/clock-editor
- ng-model is a lab of Angularjs and for two way data bing so that any change from the web side will reflect the 
value to "vis.params.format" and also any change for "vis.param.format" will be watched by ie and refresh. 

```html
<div class="form-group">
        <label>Time Format</label>
        <input type="text" ng-model="vis.params.format" class="form-control">
</div>
```

## public/clock.html
- Get the time ($scope.time) and then formated by date according to the time format required by user (user changed 
this format value and it stored in variable "vis.params.format")

```html
<div class="clockVis" ng-controller="ClockController">
        {{ time | date:vis.params.format }}
</div>bash-3.2$ 
```

