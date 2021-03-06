![publish npm package](https://github.com/vladi03/file-api-mongodb/workflows/publish%20npm%20package/badge.svg)
![publish npm package](https://github.com/vladi03/file-api-mongodb/workflows/unit-test/badge.svg)

# file-api-mongodb
Store files in MongoDb accessed through an express middleware.

```javascript
const {multerUpload} = require("file-api-mongodb");

const {upload, storage } = multerUpload({bucketName : 'darbyBucket'});
// It's very crucial that the file name matches the name attribute in your html
appRouter.post('/', upload.single('myFile'),
    (req, res) => {
    //the successful result of the file upload is in "fileData" field.
    res.json(req.fileData);
});

appRouter.get('/', (req,res) => {
    storage.getFileList(res);
});

appRouter.get('/:fileName', (req,res) => {
    storage.getFile(res, req.params.fileName);
});

appRouter.get('/id/:id', (req,res) => {
    storage.getFileById(res, req.params.id);
});

appRouter.delete('/:id', (req,res) => {
    storage.deleteFile(req.params.id, (err) => {
        if(err)
            res.status(500).send(err.message);
        else
            res.status(204).send();
    });
});
```

To scale an image that is being uploaded, set the request parameter "imageWidth"
```javascript
appRouter.post('/image/:imageWidth', upload.single('myFile'),
    (req, res) => {
        res.json(req.fileData);
    });
```

## Options
Two options are available for the constructor:
| variable | description |
| :---: | :--- |
|`connectDb (optional)` | function that returns db connection.  If a connectDb function is not provider, the variable "process.env.DB_CONN" is used mongoClient.connect(process.env.DB_CONN)|
|`bucketName (optional)`| the default value is "fileBucket"|
|`dbName (optional)`| the default value is "identity" is the database name|

## The storage object was derived from the following template
[StorageEngine](https://github.com/expressjs/multer/blob/master/StorageEngine.md)


