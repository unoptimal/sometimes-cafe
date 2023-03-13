<script>
    import Counter from "../components/Counter.svelte";
    import About from "../components/About.svelte";
    import '$lib/styles.css'
</script>

<div class='container'>
    <h1>Sometimes Cafe</h1>
    <h4><i>by <a href='https://unoptimal.com/'>unoptimal</a></i></h4>
    <h5><i>Last updated: 3/13/23</i></h5>
    <Counter />
    <About />
</div>

