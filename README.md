# how-to-write-kibana-plugin

# Doc reference
- https://www.timroes.de/2015/12/02/writing-kibana-4-plugins-basics/
- https://discuss.elastic.co/t/is-there-a-way-to-include-custom-d3-charts-in-kibana-4/24689/11

# How to write plugin

## Setup up env
- Install nodejs, npm
- Download kibana and install modules

```bash
git clone https://github.com/elastic/kibana
cd kibana
npm install 

# if java not installed, for OSX pls (brew cask install java)
npm run elasticsearch

# cd kibana source folder
npm start
```

## Write simplest plugin
### Every plugin is a npm module. So it needs at least two files
- package.json

```json
{
  "name": "tr-k4p-myplugin",
  "version": "0.1.0"
}
```

- index.json

```json
module.exports = function(kibana) {
  return new kibana.Plugin({
    // Your plugin configuration
  });
};
```

### Install plugin

```bash
bin/kibana plugin --install plugin-name -u https://git-url/plugin
```

### Three kinds plugins
- visType
- apps
- fieldFormats

## D3 plugin
- https://www.elastic.co/blog/data-visualization-elasticsearch-aggregations


## Highcharts plugin
