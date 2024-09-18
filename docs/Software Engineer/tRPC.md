Notes on learning/using tRPC

- On the client side, we can directly call `tRPC router` like this
```typescript
import { trpc } from '../utils/trpc';

export default function IndexPage() {
  const hello = trpc.hello.useQuery({ text: 'client' });
  if (!hello.data) {
    return <div>Loading...</div>;
  }
  return (
    <div>
      <p>{hello.data.greeting}</p>
    </div>
  );
}
```
- This syntax is using the power of [React Query](https://tanstack.com/query/v3/), and `tRPC` use this under the hood for both React and Next, ref [[Tanstack Query]]
- Read more about here: https://trpc.io/docs/client/react

- Specifically for NextJs, there is another approach which is using API Routes, which is similar to a traditional FE + BE architecture where we declare a few API routes in NextJS, and in client component we just make request to those API routes. In thoses API routes, we will use `tRPC` to connect to the server
```typescript
// serverClient.ts
import { appRouter } from '@/server';

import { httpBatchLink } from '@trpc/client';

import { env } from '../../../config';

export const serverClient = appRouter.createCaller(

{ links: [ httpBatchLink( { url: `https://${ env.VERCEL_URL }/api` } ) ] }

);
```

```typescript
//route.ts
import { serverClient } from '@/app/_trpc/serverClient';

import { AppRouter } from '@/server';

import { NextResponse } from 'next/server';

export const POST = async (req: Request): Promise<Awaited<ReturnType<AppRouter['createBacklog']>>> => {

	return NextResponse.json(await serverClient.createBacklog( await (req.json()) )

);

};
```

```typescript
// page.tsx
'use client';

import React, { useState } from 'react';

import axios from 'axios';

  

export default function Home() {

const [input, setInput] = useState('');

  

const onClick = async () => {

const response = await axios.post('/api/backlogs', { url: input, userId: 1 });

console.log(response.data);

};

  

return (

<div>

<input type='text' className='border' value={input} onChange={(e) => setInput(e.target.value)}/>

<button className='border' onClick={onClick}>Click me</button>

</div>

);

}
```