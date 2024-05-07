## Firebase

Using package :

```bash
npm i firebase
npm i uuid (option)
```

ref1: [firebase](https://www.npmjs.com/package/firebase)
ref2: [uuid](https://www.npmjs.com/package/uuid) -> \*option (use for name file before store)

Basic Usege :

- Create folder **app/firebase/firebase-config.ts**

```bash
import { initializeApp } from "firebase/app";
import { getStorage } from "firebase/storage";

// generated from firebase when project created
const firebaseConfig = {
  apiKey: "YOUR API KEY",
  authDomain: "YOUR AUTH DOMAIN",
  projectId: "YOUR PROJECT ID",
  storageBucket: "YOUR STORAGE BUCKET",
  messagingSenderId: "YOUR MESSAGING SENDER ID",
  appId: "YOUR APP ID"
};

const app = initializeApp(firebaseConfig);
export const  storege = getStorage(app);
```

- Call **app/page.tsx** (NextJS14)

```bash
"use client";

import { useState, useEffect, Key } from  "react";
import {
ref,
uploadBytes,
getDownloadURL,
listAll,
UploadResult,
} from  "firebase/storage";
import { storege } from  "./firebase/firebase";
import { v4 } from  "uuid";
import Reader from  "./components/reader/page";

export  default  function  Home() {
	const [fileUpload, setFileUpload] =  useState<File  |  null>(null);
	const [fileUrls, setFileUrls] =  useState<(string  |  UploadResult)[]>([]);
	const  fileListRef  =  ref(storege, "files/");

	const  uploadFile  = () => {
		if (fileUpload  ==  null) return;
		const  fileRef  =  ref(storege, `files/${v4()}`);
		uploadBytes(fileRef, fileUpload).then((url) => {
			setFileUrls((prev) => [...prev, url]);
		});
	};

	useEffect(() => {
		listAll(fileListRef).then((response) => {
			response.items.forEach((item) => {
				getDownloadURL(item).then((url) => {
					setFileUrls((prev) => [...prev, url]);
				});
			});
		});
	}, []); // Empty dependency array to run effect only once

	return (
		<div>
			Learning firebase and use it for File reading and uploading
			<hr  />
			<div>
			<input
				type="file"
				onChange={(event) => {
					const  file  =  event.target.files  &&  event.target.files[0];
					if (file) setFileUpload(file);
					}
				}
			/>
			<button claseName="btn btn-primary" onClick={uploadFile}>Upload</button>
			</div>
			{
				fileUrls.map((url) => {
					return <div key={url as Key}>{url as string}</div>;
				})
			}
		</div>
	);
}
```

More: Snap upload progress data (%)

```bash
import { getStorage, ref, uploadBytesResumable, getDownloadURL } from "firebase/storage";

const storage = getStorage();

// Create the file metadata
/** @type {any} */
const metadata = {
  contentType: 'image/jpeg'
};

// Upload file and metadata to the object 'images/mountains.jpg'
const storageRef = ref(storage, 'images/' + file.name);
const uploadTask = uploadBytesResumable(storageRef, file, metadata);

// Listen for state changes, errors, and completion of the upload.
uploadTask.on('state_changed',
  (snapshot) => {
    // Get task progress, including the number of bytes uploaded and the total number of bytes to be uploaded
    const progress = (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
    console.log('Upload is ' + progress + '% done');
    switch (snapshot.state) {
      case 'paused':
        console.log('Upload is paused');
        break;
      case 'running':
        console.log('Upload is running');
        break;
    }
  },
  (error) => {
    // A full list of error codes is available at
    // https://firebase.google.com/docs/storage/web/handle-errors
    switch (error.code) {
      case 'storage/unauthorized':
        // User doesn't have permission to access the object
        break;
      case 'storage/canceled':
        // User canceled the upload
        break;

      // ...

      case 'storage/unknown':
        // Unknown error occurred, inspect error.serverResponse
        break;
    }
  },
  () => {
    // Upload completed successfully, now we can get the download URL
    getDownloadURL(uploadTask.snapshot.ref).then((downloadURL) => {
      console.log('File available at', downloadURL);
    });
  }
);
```
