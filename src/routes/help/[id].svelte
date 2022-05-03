<script context="module">
    export async function load({ fetch, params}) {
        const id = params.id;
       const res =  await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`)
       const guide = await res.json()

       if(res.ok) {
           return {
               props: {
                   guide
               }
           }
       }

       return {
           status: 301,
           redirect: '/help/help'
           //error: new Error("Could not fetch the guide")
       }
    }
</script>

<script>
    export let guide
</script>

<div class="guide">
    <div class="title">{guide.title}</div>
    <div class="content">{guide.body}</div>
</div>