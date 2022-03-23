<div id="sidebar"></div>

<script>
  new Vue({
    el: '#sidebar',
    created() {
        fetch('https://pdf.imjcker.com/sys/menu?g=娱乐接口')
          .then(response => response.json())
          .then(result => {
            document.getElementById('sidebar').innerHTML = marked.parse(result.data);
          })
          .catch(err => console.log(err));
      }
  })
</script>
