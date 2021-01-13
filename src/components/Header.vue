<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
            <b-nav-item disabled>Counter: {{ numCorrect }}/{{ total }}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
    export default {
        props: [
            'numCorrect',
            'total'
        ]
    }
</script>

<style>

</style>