<script setup lang="ts">
import { definePageMeta, useFetch, useRequestHeaders } from "#imports";
const headers = useRequestHeaders(["cookie"]);
definePageMeta({
  layout: "topnav",
});

const { data } = useFetch("/example.json", { headers });
</script>

<template>
  <p>Try reload this page, the pre tag below will show:</p>
  <ul class="ml-6 list-disc">
    <li>
      broken =
      <strong>null</strong>
      on page reload
    </li>
    <li>working = the contents of <a href="/example.json">/public/example.json</a> on client-side route change</li>
  </ul>
  <div class="mt-10 p-2 border border-black">
    <pre>{{ JSON.stringify(data, null, 2) }}</pre>
  </div>
</template>
