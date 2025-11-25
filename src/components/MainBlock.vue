<template>
    <div class="m-0 mt-1 bg-light py-2 px-3 border rounded-4 d-flex gap-2">
        <i :class="props.icon" class="text-secondary"></i>
        {{ props.placeholder }}:
        <div style="width:1.3rem;height:1.3rem;" v-if="props.about.loading" class="spinner-border" role="status">
            <span class="visually-hidden">Loading...</span>
        </div>
        <b v-if="!props.about.loading">{{ props?.about?.continent || "Unknown" }}</b>
    </div>
</template>

<script setup>
    const props = defineEmits({
        placeholder:String,
        icon:String,
        about:Object
    })
</script>