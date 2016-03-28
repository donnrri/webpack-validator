# webpack-joi-schema
[![travis build](https://img.shields.io/travis/jonathanewerner/webpack-joi-schema.svg?style=flat-square)](https://travis-ci.org/jonathanewerner/webpack-joi-schema)
[![codecov.io](https://img.shields.io/codecov/c/github/jonathanewerner/webpack-joi-schema.svg?style=flat-square)](https://codecov.io/github/jonathanewerner/webpack-joi-schema?branch=master)

Writing webpack configs is a brittle and error-prone. This package provides a [joi](https://github.com/hapijs/joi) object schema for webpack configs. This gets you a) static type safety and b) "semantic" validations such as "`module.loaders.loader` and `module.loaders.loaders` can not be used simultaneously" or "`module.loaders.query` can only be used with `module.loaders.loader`, not with `module.loaders.loaders`".

**Note**: This is a work in progress. If you like it, you're welcome to give feedback & PR's.

### Usage
In your `webpack.config.js`:
```js
const validate = require('webpack-joi-schema')

const config = { /* ... your webpack config */ }
validate(config) // Will throw errors when config is not valid

module.exports = config
```

If you need to extend the schema, for example for custom top level properties or properties added by third party plugins like `eslint-loader` (which adds a toplevel `eslint` property), do it like this:

```js
const validate = require('webpack-joi-schema')
const schema = require('webpack-joi-schema').schema

// joi is installed as dependency of this package and will be available in node_modules
// if you use npm 3. Otherwise install it explicitly.
const Joi = require('joi') 

const yourSchema = schema.concat(Joi.object({
  // this would just allow the property and doesn't perform any additional validation
  eslint: Joi.any() 
}))

const config = { /* ... your webpack config */ }
validate(config, yourSchema) // Pass your config as second parameter

module.exports = config
```


#### License
MIT
