<vuep template="#example"></vuep>

<script v-pre type="text/x-template" id="example">
  <template>
    <div>Hello, {{ name }}!</div>
  </template>

  <script>
    export default {
      data: function () {
        return { name: 'Vue' }
      }
    }
  </script>
</script>