## Zustand

Using package :

```bash
npm i zustand
```

ref1: [zustand](https://zustand-demo.pmnd.rs/)

Basic Usage :

1. Create **/zustand/count.ts** in root directory

```bash
import { create } from "zustand";

type CountStore = {
  count: number;
  increase: () => void;
  removeAll: () => void;
  update: (newBears: number) => void;
};

export const useCountState = create<CountStore>()((set) => ({
  count: 0,
  increase: () => set((state) => ({ count: state.count + 1 })),
  removeAll: () => set({ count: 0 }),
  update: (newBears: number) => set({ count: newBears }),
}));
```

2. Create component to test "useCountState" cross component
   **components/zustand/zustand-one.tsx**

```bash
"use client";

import { useCountState } from "@/zustand/count";
import { useEffect } from "react";

const ZustandOne = () => {
  const { count, increase, removeAll, update } = useCountState();

  useEffect(() => {
    update(2);
  }, []);

  return (
    <div className="flex flex-col justify-start items-start gap-y-2">
      <p>{count}</p>
      <button onClick={increase}>one up</button>
      <button onClick={removeAll}>reset</button>
    </div>
  );
};

export default ZustandOne;
```

**components/zustand/zustand-two.tsx**

```bash
"use client";

import { useCountState } from "@/zustand/count";
import { useEffect } from "react";

const ZustandTwo = () => {
  const { count, increase, removeAll, update } = useCountState();

  useEffect(() => {
    update(2);
  }, []);

  return (
    <div className="flex flex-col justify-start items-start gap-y-2">
      <p>{count}</p>
      <button onClick={increase}>one up</button>
      <button onClick={removeAll}>reset</button>
    </div>
  );
};

export default ZustandTwo;
```

3. Create two page with different route to test "useCountState" cross route
   **app/zustand/page.tsx**

```bash
"use client";

import ZustandOne from "@/components/zustand/zustand-one";
import ZustandTwo from "@/components/zustand/zustand-two";
import { useRouter } from "next/navigation";

const ZustandPage = () => {
  const router = useRouter();
  return (
    <div className="flex justify-start items-start gap-x-2 border-spacing-2">
      <ZustandOne />
      <ZustandTwo />
      <button onClick={() => router.push("/zustand-router")}>Next</button>
    </div>
  );
};

export default ZustandPage;

```

**app/zustand-router/page.tsx**

```bash
"use client";

import { useCountState } from "@/zustand/count";

const ZustandRouterPage = () => {
  const { count, increase, removeAll } = useCountState();

  return (
    <div className="flex flex-col justify-start items-start gap-y-2">
      <p>{count}</p>
      <button onClick={increase}>one up</button>
      <button onClick={removeAll}>reset</button>
    </div>
  );
};

export default ZustandRouterPage;
```
