<script>
    import { CollapsibleCard } from 'svelte-collapsible'
    const duration = 0.2 // seconds
    const easing = 'linear'
</script>

<CollapsibleCard { duration } { easing }>
    <h2 slot='header'>About</h2>
    <div slot='body'>
        <p>This website is a proof of concept for an idea I had for a site that is sometimes available and sometimes not.</p>
        <p>No, I don't know what that means yet. Yes, almost everything is completely broken.</p>
        <p>You can view the source code <a href='https://github.com/unoptimal/sometimes-cafe'>here</a>.</p>
    </div>
</CollapsibleCard>

