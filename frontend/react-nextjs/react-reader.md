## Epub Reader (Updated at 4/2024)

Using package :

```bash
npm i react-reader 
```
ref: [react reader](https://www.npmjs.com/package/react-reader) 

Basic Usage :
```bash
import React, { useState } from 'react'
import { ReactReader } from 'react-reader'

export const App = () => {
  const [location, setLocation] = useState<string | number>(0)
  return (
    <div style={{ height: '100vh' }}>
      <ReactReader
        url="https://react-reader.metabits.no/files/alice.epub"
        location={location}
        locationChanged={
	        (epubcfi: string) => setLocation(epubcfi)
        }
      />
    </div>
  )
} 
```

