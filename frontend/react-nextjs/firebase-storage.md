## Firebase 

Using package :

```bash
npm i firebase 
npm i uuid (option)
```
ref1: [firebase](https://www.npmjs.com/package/firebase)
ref2: [uuid](https://www.npmjs.com/package/uuid) -> *option (use for name file before store)

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

