# Quill ImageHandler Module

A module for Quill rich text editor to allow images to be uploaded to a server instead of being base64 encoded.
Adds a button to the toolbar for users to click, also handles drag,dropped and pasted images.

## Demo

![Image of Yaktocat](/static/quill-example.gif)

### Install

Install with npm:

```bash
npm install quill-image-uploader --save
```

### Webpack/ES6

```javascript
Quill.register("modules/imageUploader", ImageUploader);
		var toolbarOptions = [
			['bold', 'italic', 'underline', 'strike'],        // toggled buttons
			['blockquote', 'code-block'],
			['image'],
			[{ 'list': 'ordered'}, { 'list': 'bullet' }],
			[{ 'script': 'sub'}, { 'script': 'super' }],      // superscript/subscript
			[{ 'indent': '-1'}, { 'indent': '+1' }],          // outdent/indent
			[{ 'direction': 'rtl' }],                         // text direction
			[{ 'size': ['small', false, 'large', 'huge'] }],  // custom dropdown
			[{ 'header': [1, 2, 3, 4, 5, 6, false] }],
			[{ 'color': [] }, { 'background': [] }],          // dropdown with defaults from theme
			[{ 'font': [] }],
			[{ 'align': [] }],
			['clean']
		];

		var quill = new Quill('#editor', {
			modules: {
				toolbar: toolbarOptions,
				imageUploader: {
					upload: file => {
						const fileReader = new FileReader();
						return new Promise((resolve, reject) => {
							fileReader.addEventListener('load',() => {
									let base64ImageSrc = fileReader.result;

									console.log('blabla' + base64ImageSrc);
									/* upload 2 server */
									fetch("https://domain/quill-imagetoserver.php",{
												method: "POST",
												body: base64ImageSrc
											})
									.then(response => response.json())
									.then(result => {
												console.log(result);
												resolve(result.url);
												//resolve("https://upload.wikimedia.org/wikipedia/commons/thumb/6/6a/JavaScript-logo.png/480px-JavaScript-logo.png");
											})
									.catch(error => {
												reject("Upload failed");
												console.error("Error:", error);
											});

									setTimeout(() => {
										resolve(result.url);
										//reject('Issue uploading file');
									}, 1500);

								},false);

							if (file) {
								fileReader.readAsDataURL(file);
								//fileReader.readAsDataURL(result.url);
								console.log();
							} else {
								reject("No file selected");
							}

						});
					}
				}
			},
			theme: 'snow'
		});
```

### Quickstart (React with react-quill)

React Example on [CodeSandbox](https://codesandbox.io/s/react-quill-demo-qr8xd)

### Quickstart (script tag)

Example on [CodeSandbox](https://codesandbox.io/s/mutable-tdd-lrsvh)

```javascript
// A link to quill.js
<script src="/dist/quill.js"></script>
<script src="/dist/quill.imageUploader.min.js"></script>

Quill.register("modules/imageUploader", ImageUploader);

var quill = new Quill(editor, {
  // ...
  modules: {
    // ...
    imageUploader: {
      upload: file => {
        return new Promise((resolve, reject) => {
          setTimeout(() => {
            resolve(
              "https://upload.wikimedia.org/wikipedia/commons/thumb/6/6a/JavaScript-logo.png/480px-JavaScript-logo.png"
            );
          }, 3500);
        });
      }
    }
  }
});
```
