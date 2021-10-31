<template>
    <div>
        <b-navbar toggleable="sm" type="light" variant="dark">
            <b-nav tab>
                <b-nav-item disabled>Quiz App</b-nav-item>
                <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal}}</b-nav-item>
            </b-nav>
        </b-navbar>
    </div>
</template>

<script>

export default {
    props: [
        'numCorrect',
        'numTotal'
    ]
    
    
}
</script>

<style>

</style>