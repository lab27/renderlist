<template>
    <!-- <li v-for="item in items">
        This <span class="thing">{{item.type}}</span> is {{item.color}}
    </li> -->
    <li>
        This {{type}} is {{color}}.
    </li>
</template>

<script>
export default {
    props: ['color', 'type']
}
</script>