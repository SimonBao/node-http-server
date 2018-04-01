# node-http-server
A Node web server using no framework.

```
var http = require('http');
// require http protocols for communication
var fs = require('fs');
// require fs for interacting with file servers.
var path = require('path');
// require path utilities for working with file/directory paths

http.createServer(function(request, response){
  // create server which listens at port 8125
  console.log('requested', request.url);
  // on each server request, console log request url
  var filePath = '.' + request.url;
  // set filePath in relation of request url
  if(filePath == './'){
    filePath = './index.html';
  }
  // if filePath is './' mutate it to './index.html' 
  var extname = String(path.extname(filePath)).toLocaleLowerCase();
  // set extname in relation to filePaths extension
  var mimeTypes = {
    '.html': 'text/html',
    '.js': 'text/javascript',
    '.css': 'text/css',
    '.png': 'image/png',
    '.jpg': 'image/jpg',
    '.gif': 'image/gif',
    '.wav': 'audio/wav',
    '.mp4': 'video/mp4',
    '.json': 'application/json',
    '.woff': 'application/font-woff',
    '.ttf': 'application/font-ttf',
    '.eot': 'application/vnd.ms-fontobject',
    '.otf': 'application/font-otf',
    '.svg': 'applcation/image/svg+xml'
  };
  // set key value pairs which match extension to its mime file type

  var contentType = mimeTypes[extname] || 'application/octet-stream';
  // set contentType in relation to found mime file type else set to undefined octect-stream type

  fs.readFile(filePath, function(error, content){
    // read file at filePath
    if(error){
      // if file cannot be read
      if(error.code == 'ENOENT'){
        // and error code is ENOENT (meaning no such directory entry ||  File not found)
        fs.readFile('./404.html', function(error, content){
          // read 404 file
          response.writeHead(200, {'Content-Type': contentType});
          // send status code 200 on success and show contentType
          response.end(content, 'utf-8');
          // serve 404.html page as content
        });
      }
      else
      // if 404.html page cannot be found
      {
        response.writeHead(500);
        // status code 500 for server error
        reponse.end('Server Error, Error Code: ' + error.code + '..\n..');
        // serve error message
      }
    }
    else
    // file found at filePath
    {
      response.writeHead(200, {'Content-Type': contentType});
      // status 200 for success and show contentType
      response.end(content, 'utf-8');
      // serve content
    }
  });
}).listen(8125);
// server open for communication at port 8125
console.log('Server listening on 127.0.0.1:8125');
```
