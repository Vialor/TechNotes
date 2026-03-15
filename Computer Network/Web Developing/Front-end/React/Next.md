```bash
npx create-next-app@latest
```
# Rendering Strategies
**Static Site Generation SSG**: data is static, and CDN provides them
**Client Side Rendering CSR**: the client makes AJAX calls to load data
**Server Side Rendering SSR**: the server makes AJAX calls and send the data collected to the client
> app router is using SSR by default now
``` typescript
export const getServerSideProps: GetServerSideProps<Props> = async() => {
	const res = await axios.get<{person: string}>("/api/person");
	const { person } = res.data;
	return {
		props: {
			person,
		},
	};
}
```
**Incremental Static Rendering ISR**: the server makes AJAX calls and send the data collected to the client, but this time uses a cache before AJAX
```typescript
export const getStaticProps: getStaticProps<Props> = async() => {
	const res = await axios.get<{person: string}>("/api/person");
	const { person } = res.data;
	return {
		props: {
			person,
		},
		revalidate: 10, // data is preserved for 10 secs
	};
}
```
