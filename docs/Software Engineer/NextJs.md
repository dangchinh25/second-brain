## Overview

![](https://i.imgur.com/F14FSkd.png)

![](https://i.imgur.com/8LLWGbd.jpg)

- Traditional React App will have to make multiple call to the server to fetch different information  to be able to finally combine and display to the user, and all of this happens on the users' devices. And before it has finished everything, the user will pretty much just see a blank screen.
- With NextJs, the page content and all the fetching call is already processed in the server (including HTTP call to get data to Backend), so when the users receive the html, it already contains the correct data, and view, and some metadata. So now the the users' device only need to run some additional Js to `hydrate`, which means to add some functionality like `onClick` event (or basically anything that can't run on a server).
![](https://i.imgur.com/4uVu3n9.png)

![](https://i.imgur.com/uVqWXd3.jpg)
- We need to be aware that the only difference is on the initial HTML load, with NextJs, the initial HTML is already correct with the data, but with React, the initial HTML is pretty much empty and it needs to for Javascript to fetch the data.
- So basically, NextJs is a Single Page React App, but with some additional features and be able to get the data right the first time, but it is still a React App.
- Source: https://www.youtube.com/watch?v=d2yNsZd5PMs
## Learning Notes
- The new/most up to date way to do routing is via App router (so anything that has `app` is the new way, and `pages` is the old way, pretty much does the same thing so we don't have to think about that too much)
- Each component is server by default (what does that mean?)
- Can use `useRouter` to trigger rerender without triggering the whole page reload
- In addition to just being an UI framework, we can also implement server-side logic in NextJs in the same repo that we implement UI logic => Use ApiRoutes/App Router => [[NextJs#NextJs for Backend]]
## Pre-Rendering
`hydration`: This is the process of attaching react listeners to already-existing html dom nodes.

`rehydration`: This is pretty much the same thing, but is used in reference to SSR - SSR returns full html, but you still need to attach the react listeners to it.

![](https://i.imgur.com/ZWsOBLj.png)
- The server **Pre-render** the page (generate the HTML) and send it to the Client, so the Client only need to attach event listener to it to make it interactive

![](https://i.imgur.com/7to23aq.png)
- The client will have to do everything, from bundling all the component and then make interactive
	- THe client receive HTML, CSS, JS individually and then start combining them
	- And when all of this is working, the user just see a blank page

## Server Side Rendering
- Read [[NextJs#Pre-Rendering]]
In a production environment, when you deploy a Next.js application, it is typically deployed to a server that handles incoming HTTP requests from clients (usually web browsers). This server, which can be a traditional backend server or a serverless environment, is responsible for:

1. **Handling HTTP Requests:** The server receives incoming HTTP requests from clients, such as web browsers.
    
2. **Server-Side Rendering (SSR):** If a Next.js page is configured for server-side rendering, the server will generate the HTML for that page on the server side. This can involve fetching data from a database, making API calls, or performing any necessary server-side computations.
    
3. **Generating HTML:** Once the server has all the necessary data and components, it generates the HTML for the requested page. This HTML can include dynamic content and is ready to be sent to the client.
    
4. **Sending HTML Response:** The server sends the HTML response, along with appropriate HTTP headers, back to the client's web browser.
    
5. **Client-Side Hydration:** When the HTML is received by the client's browser, any client-side JavaScript code associated with the page (e.g., React components) takes over. This code "hydrates" the page, adding interactivity and enabling client-side routing, state management, and dynamic updates.
    
So, in a production environment, Next.js combines server-side rendering (SSR) with client-side rendering (CSR) to provide an optimized user experience. The server handles the initial rendering and generation of HTML, while the client handles subsequent interactions and updates, making the application dynamic and interactive.

In a production environment, when you deploy a Next.js application, it is typically deployed to a server that handles incoming HTTP requests from clients (usually web browsers). This server, which can be a traditional backend server or a serverless environment, is responsible for:

1. **Handling HTTP Requests:** The server receives incoming HTTP requests from clients, such as web browsers.
    
2. **Server-Side Rendering (SSR):** If a Next.js page is configured for server-side rendering, the server will generate the HTML for that page on the server side. This can involve fetching data from a database, making API calls, or performing any necessary server-side computations.
    
3. **Generating HTML:** Once the server has all the necessary data and components, it generates the HTML for the requested page. This HTML can include dynamic content and is ready to be sent to the client.
    
4. **Sending HTML Response:** The server sends the HTML response, along with appropriate HTTP headers, back to the client's web browser.
    
5. **Client-Side Hydration:** When the HTML is received by the client's browser, any client-side JavaScript code associated with the page (e.g., React components) takes over. This code "hydrates" the page, adding interactivity and enabling client-side routing, state management, and dynamic updates.
    
So, in a production environment, Next.js combines server-side rendering (SSR) with client-side rendering (CSR) to provide an optimized user experience. The server handles the initial rendering and generation of HTML, while the client handles subsequent interactions and updates, making the application dynamic and interactive.

If a page is fully server-side rendered (SSR), the server will send back a complete HTML document that represents the fully rendered and populated web page. This HTML document is ready to be displayed to the user, and it typically contains all the content, data, and markup needed to render the page.

Here's what the server sends back for a fully server-side rendered page:

1. **Complete HTML Document:** The server generates an HTML document that includes all the necessary HTML markup for the page, including headings, paragraphs, images, links, forms, and any other HTML elements that make up the page's structure.
    
2. **Data Integration:** The HTML document may include data that was fetched from a database or an external API. This data is typically embedded directly within the HTML as JSON or JavaScript objects.
    
3. **Client-Side Hydration Script:** In the case of a Next.js application, the server may include a small JavaScript script that's responsible for "hydrating" the page on the client side. This script initializes any client-side interactivity and reactivity, making the page dynamic without a full page reload.
    
4. **CSS:** The server may also include CSS stylesheets or inline styles within the HTML document to style the page's elements. This ensures that the page looks correctly styled when it's initially rendered.
    
5. **HTTP Headers:** The server sets appropriate HTTP headers in the response, including the content type, caching directives, and any other relevant headers.
    
6. **HTTP Status Code:** The server sets an HTTP status code in the response, indicating the success or status of the request (e.g., 200 OK).
    

When the client's browser receives this response, it renders the HTML document and executes any associated scripts, allowing the user to see the fully rendered page with all its content and interactivity immediately. Subsequent interactions within the page can still be handled on the client side, but the initial rendering is done on the server, which can be beneficial for SEO and performance.
## Server vs Client Component
- The short answer is whenever we don't need to use DOM API like onClick onChange, etc button, we should use Server Component
- Server Component will leverage the server to make request, and build the page before sending it to the user => Increase page load by a lot
- Client component will just be regular React/Javascript component
## Deployment
#### Serverless/Edge Function
- https://www.youtube.com/watch?v=M2KUAb1FH1Y
- Serverless are just prebuilt function that will only be executed when needed, so we dont waste idle time. Serverless function will be deployed to a cloud, when receive a request, the function will be executed in the cloud that it was deployed to => Potential latency if the geographical distance is large
- Edge Function are serverless function but it is deployed to multiple edge location, so when a request comes in, it will be routed to the nearest edge location where the edge function lives
- Api Route/App Router in NextJs when deploy to Vercel will be deployed as Serverless Function

## NextJs for Backend
![](https://i.imgur.com/3kTZmiv.jpg)
- The picture above is just some point to be considered, provided by https://www.youtube.com/watch?v=2cB5Fh46Vi4, it's not an exhaustive list
- NextJs is not an actual backend server, it's just a way to contain backend logic without having to manage complex infrastructure and splitting between multiple repos (suitable for solo/indie project)
- Any incoming request will invoke a Serverless Function that just build and execute the matching server-side function only, and Vercel handles all of this, we just need to write the server-side function logic and put it in the right place.
- However, doing this on the client will expose the api routes, which means basically anyone can just go to `<hostname>/api/<something>` and they will be able to access our endpoint, which is not too bad (cause as of right now if we spin up an Express server, and go to a random GET route from the browser, it's gonna be the same thing), but something to be considered
- So the problem lies in the fact that we need have both BE and FE in the same repo => Tightly coupled
	- This is fine for small/indie project and also incredibly fast to finish a project
	- But we should definitely separate BE and FE for bigger project where we work with multiple people and divided into different teams
	- This also goes as we may also need to scale BE and FE differently and not evenly
	- And it also does not make sense if we use Microservices architecture, where one BE service will be called by many other services
		- One possible pattern is that NextJs can acts as a BFF that it coordinates and call multiple BE services
- NextJs is also not really suitable if we want to do some complex BE logic, that may takes time to process (limitation with serverless) or any cronjob stuff.
- Good reference: https://www.youtube.com/watch?v=Rrz2q5uCHdE

- On the other, NextJs is awesome when we want to spin up a small API server that handles some simple logic and want it to be deployed in minutes (on Vercel)
	- This will be done by just using NextJs and its Api/App router, and nothing else, not even any NextJs components
	- It can also be beneficial that we can leverage Vercel edge deployment, so that makes our serverless code to be even faster