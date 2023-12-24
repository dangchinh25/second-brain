### 2023 revision
- FE: NextJs + TailwindCSS
- BE: 
	- Express for complex large application => [[NextJs#NextJs for Backend | When not use NextJs for backend]]
	- NextJS for simple API
- Communication: 
	- tRPC + NextJs for simple solo project (read [[Random thoughts (tech)#tRPC | here]])
		- Tbh, most indie project should be fine with just tRPC + NextJs stack, and deploy everything on Vercel (with Serverless/Edge Function for NextJs `Api Routes`)
		- Ref: https://www.youtube.com/watch?v=qCLV0Iaq9zU
	- REST with separated BE/FE on different for larger project 
- Bootstrap app
	- If use NextJS fullstack => Manual but in the same repo, create-t3-app default to use PlanetScale, which we don't want
	- Otherwise, traditional bootstrap FE and BE seperately
- ORM: Prisma/Drizzle
- Authentication: Auth.JS/NextAuthJS/Clerk
- Logging: Axiom
- Deployment:
	- Vercel for NextJS
	- Render/Railway for BE ExpressJs
- Database: Neondb/RDS
- Others: Upstash
	- Redis, CRONs, event queues, kafka, rate limiting, and so much more.
- Knock.app for notifications

### 2023 version
- Tools that help boost solo developer productiviy
	- Deployment: Vercel (this is only for FE, what about BE?)
	- BE Deployment: Render? (looks pretty good, but concern about spinning down on idle) or also look into Railway
	- Database: Planetscale (MySQL but the developer experience is good?) => As MySQL does not support Vector type => Look more into Neondb or good old RDS
	- Logging: Axiom (already has integration with Vercel)
	- Others (cache, rate limit, integration etc): Upstash

- Tech stacks
	- FE + BE: NextJS => Second though: only for FE (as NextJS backend is serverless, which has some limitations)
		- Serverless has short time out on each request, which is only around few seconds
		- Serverless can't handle websocket
		- Should not use really complex logic
	- BE: ExpressJs
	- Communications: tRPC
	- Bootstrap app: create-t3-app => Nope, as it is too opinionated and relies heavily on NextJs => Still go with traditional seperate FE and BE repo
	- Prisma/Drizzle
	- Tailwind CSS
	- Auth.JS/NextAuthJS/Clerk
	- Astro for static site
	- Turborepo

- Other tools
	- Excalidraw => Drawing, diagraming
	- Notion
	- Linear => Project/task tracking

- Sources
	- https://t3.gg/blog/post/2023-tech
	- https://t3.gg/blog/post/2023-infra