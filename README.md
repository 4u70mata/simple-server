

# Simple Node.js HTTP Server

This is a basic HTTP server created with Node.js that serves static files from the current directory. It can handle different MIME types and serves a default `index.html` file when a directory is requested.

## Features

- Serves static files (HTML, JPEG, JPG, PNG, JS, CSS).
- Handles different MIME types based on file extensions.
- Redirects to `index.html` when a directory is requested.
- Returns appropriate HTTP status codes for file not found (404) and internal errors (500).

## Requirements

- Node.js (https://nodejs.org/)

## Installation

1. **Install Node.js**: If you haven't already, download and install Node.js from [Node.js website](https://nodejs.org/).

2. **Clone the repository**:
   ```bash
   git clone <repository_url>
   ```
   Or, you can simply copy the provided code into a file named `server.js`.

3. **Navigate to the project directory**:
   ```bash
   cd <project_directory>
   ```

## Usage

1. **Run the server**:
   ```bash
   node server.js
   ```

2. **Access the server**:
   Open your web browser and navigate to `http://localhost:3000`.

## Code Explanation

Here's a brief overview of what the code does:

1. **Import necessary modules**:
   ```javascript
   const http = require('http');
   const url = require('url');
   const path = require('path');
   const fs = require('fs');
   ```

2. **Define MIME types**:
   ```javascript
   const mimeTypes = {
     "html": "text/html",
     "jpeg": "image/jpeg",
     "jpg": "image/jpg",
     "png": "image/png",
     "js": "text/javascript",
     "css": "text/css"
   };
   ```

3. **Create the HTTP server**:
   ```javascript
   http.createServer(function(req, res) {
     var uri = url.parse(req.url).pathname;
     var fileName = path.join(process.cwd(), unescape(uri));
     console.log('Loading' + uri);
     var stats;

     try {
       stats = fs.lstatSync(fileName);
     } catch (e) {
       res.writeHead(404, {'Content-type': 'text/plain'});
       res.write('404 Not Found\n');
       res.end();
       return;
     }

     if (stats.isFile()) {
       var mimeType = mimeTypes[path.extname(fileName).split(".").reverse()[0]];
       res.writeHead(200, {'Content-type': mimeType});
       var fileStream = fs.createReadStream(fileName);
       fileStream.pipe(res);
     } else if (stats.isDirectory()) {
       res.writeHead(302, {'Location': 'index.html'});
       res.end();
     } else {
       res.writeHead(500, {'Content-type': 'text/plain'});
       res.write('500 Internal Error\n');
       res.end();
     }
   }).listen(3000);
   ```

4. **Start the server**:
   The server listens on port 3000. You can access it by navigating to `http://localhost:3000` in your web browser.

## License

This project is licensed under the MIT License.
