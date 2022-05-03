<script context="module">
    export async function load({ fetch }) {
       const res =  await fetch("https://jsonplaceholder.typicode.com/posts")
       const guides = await res.json()

       if(res.ok) {
           return {
               props: {
                   guides
               }
           }
       }

       return {
           status: res.status,
           error: new Error("Could not fetch the guide")
       }
    }
</script>
<script>
    export let guides
</script>
<h1>Hello</h1>
<div class="guide">
    <ul>
        {#each guides as guide}
            <li><a sveltekit:prefetch href="{`/help/${guide.id}`}">{guide.title}</a></li>
        {/each}
    </ul>
</div>