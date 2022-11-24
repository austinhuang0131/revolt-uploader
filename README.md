# Revolt Uploader

Revolt.js doesn't offer the ability to upload attachments, so here is a utility package to allow easy file uploads.

## Installation

`npm install revolt-uploader`

It's as easy as that! :)

## Usage

First, you have to import and initialize an uploader object.

```javascript
// import the uploader library
const Uploader = require("revolt-uploader");

// you have to initialize a revolt.js client object as well.
// Then initialize the uploader and provide it with the client
const uploader = new Uploader(client);
```

Now you've got your uploader object. All you have to do is to login your bot client using `client.login("token")`

After that, you can upload files to revolt's servers using the `uploadFile` method.

```javascript
// you need to attach this to a message, meaning you need to have a message object
// you can get this by listening for the `message` event on the client object but this is up to you
client.on("message", async (message) => {
  // you can upload a file using the `upload` method
  // it will return an attachment id, you can add to the message
  Promise.allSettled([
    uploader.upload("path/to/file"),
    uploader.upload("path/to/another/file"),
  ]).then(attachments => { // we're using Promise.allSettled to asynchronously upload all of them
    attachments = attachments.map(attachment => attachment.value); // extracting the value from the promises
    // send the attachment to the channel
    message.channel.sendMessage({
        content: "Here is your file!",
        attachments: attachments // Note that attachments alway has to be an array, even if you're only uploading one file
    });
    // All done!
  });
});
```