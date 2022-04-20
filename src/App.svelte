<script>
	import Generate from "./routes/Generate.svelte";
	import Display from "./routes/Display.svelte";
	import Router from "svelte-spa-router";
	const routes = {
		"/": Generate,
		"/display/:content": Display,
	};
</script>

<Router {routes} />
