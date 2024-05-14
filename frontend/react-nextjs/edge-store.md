## Edge store

Using package :

```bash
npm install @edgestore/server @edgestore/react
```

ref1: [edge-store](https://edgestore.dev/docs/quick-start)

1. Create **.env** file in root directory.

```bash
EDGE_STORE_ACCESS_KEY=your-access-key
EDGE_STORE_SECRET_KEY=your-secret-key
```

2. Create api **app/api/edgestore/[...edgestore]/route.ts** file.

```bash
import { initEdgeStore } from '@edgestore/server';
import { createEdgeStoreNextHandler } from '@edgestore/server/adapters/next/app';
 
const es = initEdgeStore.create();
 
const edgeStoreRouter = es.router({
  publicFiles: es.fileBucket(),
});
 
const handler = createEdgeStoreNextHandler({
  router: edgeStoreRouter,
});
 
export { handler as GET, handler as POST };

export type EdgeStoreRouter = typeof edgeStoreRouter;
```

3. Create **lib/edgestore.ts** file.

```bash
'use client';
 
import { type EdgeStoreRouter } from '../app/api/edgestore/[...edgestore]/route';
import { createEdgeStoreProvider } from '@edgestore/react';
 
const { EdgeStoreProvider, useEdgeStore } =
  createEdgeStoreProvider<EdgeStoreRouter>();
 
export { EdgeStoreProvider, useEdgeStore };
```

4. Edit **app/layout.tsx** file

```bash
import { EdgeStoreProvider } from '../lib/edgestore'; // ==== #1
import './globals.css';

// ...

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        <EdgeStoreProvider>{children}</EdgeStoreProvider> // ==== #2
      </body>
    </html>
  );
}
```

Basic Usage :

5. Create route for use Edge store. For Example create **app/edge-store/page.tsx** file

Import:

```bash
import { useEdgeStore } from "@/lib/edgestore";
```

Upload file:

```bash
const [file, setFile] = React.useState<File>();
const { edgestore } = useEdgeStore();

const uploadFile = async () => {
if (file) {
    const res = await edgestore.publicFiles.upload({
            file,
            onProgressChange: (progress) => {
                // you can use this to show a progress bar
                console.log(progress);
            },
        });
        console.log(res);
    }
};
```

Replace file:

```bash
const [file, setFile] = React.useState<File>();
const { edgestore } = useEdgeStore();

const uploadFile = async () => {
if (file) {
    const res = await edgestore.publicFiles.upload({
            file,
            options: {
                replaceTargetUrl: oldFileUrl,
            },
            onProgressChange: (progress) => {
                // you can use this to show a progress bar
                console.log(progress);
            },
        });
        console.log(res);
    }
};
```

Delete file:

```bash
const [file, setFile] = React.useState<File>();
const { edgestore } = useEdgeStore();

const uploadFile = async () => {
if (file) {
        await edgestore.publicFiles.delete({
            url: urlToDelete,
            onProgressChange: (progress) => {
                // you can use this to show a progress bar
                console.log(progress);
            },
        });
    }
};
```
