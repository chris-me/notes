## Serviceaccount

only needed when code is not executed directly with "Firebase Functions" (but for example locally in a NodeJS environment)

- Firebase Console --> wheel beside Overview --> Serviceaccounts --> Create
- after creation the .json file containing the secrets can be downloaded here once

## Links 

- API reference: https://googlecloudplatform.github.io/google-cloud-node/#/docs/storage/1.1.0/storage/bucket


## Examples

```javascript
// config is only needed when run in untrusted env (i.e. not fb functions)
const config = {
  projectId: 'my-project-id',
  keyFilename: './my-project.json'
}

const storage = require('@google-cloud/storage')(config)
let bucket = storage.bucket("my-project.appspot.com")

bucket.getFiles(function(err, files) {
  if (!err) {
    files.forEach( (file) => {
      console.log(file.name)
    })
  } else {
    console.log(err)
  }
})

// or as a promise:

bucket.getFiles().then( (data) => {
  const files = data[0];
  files.forEach( (file) => {
    console.log(file.name)
  })
}).catch( (err) => {
    console.log(err)
})

bucket.upload('somefile.txt', function(err, file) {
  console.log(err)
  // file.createReadStream();
  // file.getMetadata();
  // ...
})

bucket.deleteFiles({ prefix: 'directoryName/' }, function(err) {
  if (err) {
    console.log(err)
  }
})
```
