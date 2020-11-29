# use-error

> _Use Errors like it's 2020_


## Installation

__yarn__
`yarn add use-error`

__npm__
`npm install use-error`


## Basic Usage

```
// In a new file at example location "./src/useErrors/index.js"
// ------------------------------------------------------------

// CommonJS
const useError = require('use-error')

// ES6
import useError from 'use-error'


// Default config options. Customize to fit your needs.
// ===============================================================================================================
// * For detailed descriptions of each config option, please continue reading below under "Configurations" section
// ===============================================================================================================
// - NOTE: use-error relies on process.env.NODE_ENV to determine how to handle error output format.
           Namely, process.env.NODE_ENV === 'development' yields the most verbose output.
           Make sure to disable this error output in production environments for performance
           and security concerns.
           Optionally, define your own custom value which process.env.NODE_ENV resolves to
           for producing verbose error output by changing the value of "devEnv" property in
           your config options (See below).   
            
const errorOptions = {
  devEnv: 'development',
  transports: [
    process.stdout,         // NodeJS default stream used by global "Console" object
    process.stderr          // Same as ^^^
  ],
  prettyPrint: true,        // By default, true if value given for "devEnv" matches the value of process.env.NODE_ENV 
  smartStackTrace: true,    // Default value is set with same logic explained for "prettyPrint"
  groupTypes: [
    'native',               // NodeJS Native Error types
    'common',               // Programming Error types
    'http'                  // Operational Error types
  ],
  patchNodeProcess: true,   // Automatically patch 'rejection' and 'exceptions' caught in NodeJS process loop    
}


// "useError" is a function which expects two arguments:
//  - First argument: 
//     String value which acts as a key refference to the Error scope visible in transport stream output.
//     Default value is "GlobalError" if no value is provided
//  - Second argument:
//     Object value which acts as the configuration options for this Error scope
//     Default value is an "Object" with key/value pairs found in the "errorOptions" object shown above


// Create useError - option 1: Global instance that inherits config options 
const useGlobalError = useError()                                 // "useError" created using all default config options
const useGlobalError = useError('MyAppNameError', errorOptions)   // "useError" created using custom value defining Error scope

// CommonJS Export
module.exports = useGlobalError
// ES6 Export
export default useGlobalError

// Then....

// In seperate file (i.e. ./src/index.js)
// CommonJS Require
const useGlobalError = require('./useErrors')
// ES6 Import
import useGlobalError from '../useErrors'

const { onError } = useGlobalError



// Create useError - option 2: Single instance that inherits config options
// CommonJS Namespaced Exports
module.exports.useAppError = useError('AppError', errorOptions)
module.exports.useServiceError = useError('ServiceError', errorOptions)
module.exports.useApiError = useError('ApiError', errorOptions)
// ES6 Namespaced Exports
export const useAppError = useError('AppError', errorOptions)
export const useServiceError = useError('ServiceError', errorOptions)
export const useApiError = useError('ApiError', errorOptions)

// Then...

// In seperate file (i.e. ./src/app.js) 
import { useAppError } from '../useErrors'
const { onError } = useAppError

// In seperate file (i.e. ./src/service.js)
import { useServiceError } from '../useErrors'
const { onError } = useServiceError

// In seperate file (i.e. ./src/api.js)
import { useApiError } from '../useErrors'
const { onError } = useApiError


// Using onError methods

const { throwError, returnError, emitError, CreateErrorAction } = onError


```

