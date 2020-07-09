<template>
    <layout class-prefix="layout">
        <NumberPad/>
        <Types/>
        <Notes/>
        <Tags :data-source="tags" />
    </layout>
</template>

<script>
    import NumberPad from '@/components/Money/NumberPad.vue';
    import Types from '@/components/Money/Types.vue';
    import Notes from "@/components/Money/Notes.vue"
    import Tags from "@/components/Money/Tags.vue"

    export default {
        name:'Money',
        components: {Tags, Notes, Types, NumberPad},
        data(){
            return{
                tags:['衣','食','住','行','意外']
            }
        }

    };
</script>
<style lang="scss">
    .layout-content {
        display: flex;
        flex-direction: column-reverse;
    }
</style>

