# Cached S3 Wrapper

[![Build Status](https://img.shields.io/travis/simlu/s3-cached/master.svg)](https://travis-ci.org/simlu/s3-cached)
[![Test Coverage](https://img.shields.io/coveralls/simlu/s3-cached/master.svg)](https://coveralls.io/github/simlu/s3-cached?branch=master)
[![Greenkeeper badge](https://badges.greenkeeper.io/simlu/s3-cached.svg)](https://greenkeeper.io/)
[![Dependencies](https://david-dm.org/simlu/s3-cached/status.svg)](https://david-dm.org/simlu/s3-cached)
[![NPM](https://img.shields.io/npm/v/s3-cached.svg)](https://www.npmjs.com/package/s3-cached)
[![Downloads](https://img.shields.io/npm/dt/s3-cached.svg)](https://www.npmjs.com/package/s3-cached)
[![Semantic-Release](https://github.com/simlu/js-gardener/blob/master/assets/icons/semver.svg)](https://github.com/semantic-release/semantic-release)
[![Gardener](https://github.com/simlu/js-gardener/blob/master/assets/badge.svg)](https://github.com/simlu/js-gardener)
[![Gitter](https://github.com/simlu/js-gardener/blob/master/assets/icons/gitter.svg)](https://gitter.im/simlu/s3-cached)

S3 File Access Abstraction providing Memory and Disk Caching Layer. Useful e.g. in lambda functions if you want to reduce the amount of s3 access for serving static files.

## What it does

- Access abstraction to access JSON and GZipped data on AWS S3
- Two Layer caching (memory and disk)
- Allows you to define cache constraints like TTL

# Getting Started

### Install

    $ npm install --save s3-cached

### Request S3 Files

```javascript
const s3 = require("s3-cached")({
  bucket: "YOUR_BUCKET_NAME",
  accessKeyId: "YOUR_ACCESS_KEY_ID",
  secretAccessKey: "YOUR_SECRET_ACCESS_KEY"
});

s3.getJsonObjectCached("large.json").then((json) => {
  // do something with the json data
}).catch((err) => {
  // there has been an error
});
```

# Available functions

- `getJsonObjectCached`: retrieve file content, parse as json and return as promise
- `getDeflatedObjectCached`: retrieve file content, deflate and return as promise

Note that you can specify the ttl on a per file basis by calling e.g. `s3.getJsonObjectCached(FILE_NAME, TTL)`.

# Options

### bucket

Type: `string`<br>
*Required*

Specify the Bucket name you want to retrieve data from.

### accessKeyId

Type: `string`<br>
Default: Globally configured access key

Allows to set a custom access key for accessing this bucket.

### secretAccessKey

Type: `string`<br>
Default: Globally configured secret access key

Allows to set a custom secret access key for accessing this bucket.

### ttlDefault

Type: `integer`<br>
Default: `600`

Define how long a cached file is kept by default. *This can be overwritten on a per-file basis* by passing a second parameter into the function.

### memoryLimit

Type: `integer`<br>
Default: `100`

Define how many cached entities can be hold in memory at the same time. If more entities are present, the earliest are discarded from memory cache.

### diskMaxSize

Type: `integer`<br>
Default: `469762048`

Maximum amount of disk space in bytes used by disk cache. Earliest files are discarded from file cache if more space is used.

### diskTmpDirectory

Type: `string`<br>
Default: `/tmp`

Location to store temporary data for disk cache.
