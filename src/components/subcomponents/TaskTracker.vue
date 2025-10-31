<script>
export default { 
    props: {
        task: Object,
        idx: Number
    },
    emits: ['remove-task']
}
</script>

<template>
    <div class="card mb-3" style="width: 18rem;">
        <div class="card-body">
            <h5 class="card-title">Deadline: {{ task.deadline }}</h5>
            <p class="card-text">{{ task.desc }}</p>
            <button class="btn btn-success" @click="$emit('remove-task', idx)">Done</button>
        </div>
    </div>
</template>
