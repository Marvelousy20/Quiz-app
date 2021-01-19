<template>
    <div>
        <nav class="navbar navbar-expand-lg navbar-light bg-primary text-white">
            <div class="container-fluid">
                <h3 class="navbar-brand text-white">Navbar</h3>
                <h4>Count: {{ correctNum }} / {{ totalNum }} </h4>
            </div>
        </nav>
    </div>
</template>

<script>
    export default {
        props: [
            'totalNum',
            'correctNum'
        ]
    }
</script>