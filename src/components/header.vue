<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled>quiz app</b-nav-item>
            <b-nav-item disabled>counter : {{score}}/{{numQuestion}}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
    export default {
        props: {
            score: Number,
            numQuestion: Number
        }
    }


</script>